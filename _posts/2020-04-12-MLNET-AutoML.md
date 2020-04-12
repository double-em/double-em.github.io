---
layout: post
title:  "Brug ML.NET og AutoML"
author: Mike Meldgaard
date:   2020-4-12 #01:00:12 +0530
category: MachineLearning
summary: Brug af og eksperimenter med ML.NET og deres AutoML funktion
thumbnail: development.png
---

# ML.NET - Framework
Erfaringer med brugen og eksperimenter i ML.NET.

## Hvad er ML.NET?
ML.NET er et high-level machine learning bibliotek af Microsoft, som gør brug af andre ML biblioteker som Tensorflow og ONNX.

ML.NET har også en anden funktion som svare lidt til Google's Vision som prøvet, at finde den mest effektive algoritme, nemlig AutoML. AutoML prøver, at bruge en række forskellige algoritmer med forskellige parametre og træner lidt på dem for, at se hvad der giver det bedste resultat. Når den er færdig efter den tid man har givet den, så kommer den tilbage med en liste af de bedste algoritmer den fandt til jobbet.

ML.NET er lige nu i Preview som giver lidt udfordringer hen af vejen, samtidig med, at dokumentationen kan være lidt manglende, da ikke alle brugs måder er tænkt efter, enten pga. det ikke er udbredt eller fordi understøttelse først kommer senere. Dette problem stødte jeg desværre ind i.

## Brugen af ML.NET
Jeg startede med, at følge deres 10 minutters "Getting started" - guide. Da jeg ville forsøge mig med deres AutoML for, at se hvad den ville gøre med den billede data jeg havde.

Det første jeg gjorde var, at prøve at definere hovedmappen til bilederne, men jeg finder så ud af, at ML.NET forventer en, hvis mappestruktur til billederne. Så billederne skal ligge i en mappe med en undermappe per mærkat eller klasse. Og nede i de undermapper skal de dertilhørende billeder så ligge.

Microsoft giver selv dette eksempel:

```
\---flower_photos
    +---daisy
    |       100080576_f52e8ee070_n.jpg
    |       102841525_bd6628ae3c.jpg
    |       105806915_a9c13e2106_n.jpg
    |
    +---dandelion
    |       10443973_aeb97513fc_m.jpg
    |       10683189_bd6e371b97.jpg
    |       10919961_0af657c4e8.jpg
    |
    +---roses
    |       102501987_3cdb8e5394_n.jpg
    |       110472418_87b6a3aa98_m.jpg
    |       118974357_0faa23cce9_n.jpg
    |
    +---sunflowers
    |       127192624_afa3d9cb84.jpg
    |       145303599_2627e23815_n.jpg
    |       147804446_ef9244c8ce_m.jpg
    |
    \---tulips
            100930342_92e8746431_n.jpg
            107693873_86021ac4ea_n.jpg
            10791227_7168491604.jpg
```
Som er et eksempel med nogle blomsterbilleder.

Men jeg har kun alle 49000 billeder i en mappe og en csv fil der passer til. Så jeg undersøgte, hvordan jeg nemt kunne sortere disse billeder.

Jeg kodede så et Python script, som tager en csv fil og finder billednavnet i rækken og sortere dem efter mærkaterne.

<script src="https://gist.github.com/Zxited/a02e7af37a26ee8bf39f65094b668604.js"></script>

Efter så at have kørt mit script. Prøvede jeg så, at definere mappen med og uden csv fil. Det virkede heller ikke. 

Efter en del søgen inde i Microsofts ML.NET dokumentation og deres samples repository på GitHub. Fandt jeg noget jeg måske kunne bruge. Det var deres "MulticlassClassification_AutoML" eksempel. Hvor jeg ved et tilfælde så, at de brugte en 64 klasser og en label, da de bruger index i csv filen til, at sige, hvor meget er input og hvor meget er mærkat.

Dette kan ses nedenunder, hvor de i TextLoader definere indekset af mærkaten og datatypen.
```
var trainData = mlContext.Data.LoadFromTextFile(path: TrainDataPath,
                        columns : new[] 
                        {
                            new TextLoader.Column(nameof(InputData.PixelValues), DataKind.Single, 0, 63),
                            new TextLoader.Column("Number", DataKind.Single, 64)
                        },
                        hasHeader : false,
                        separatorChar : ','
                        );
```
Og i deres csv står et billede som følgende.

```
0,0,5,13,9,1,0,0,0,0,13,15,10,15,5,0,0,3,15,2,0,11,8,0,0,4,12,0,0,8,8,0,0,5,8,0,0,9,8,0,0,4,11,0,1,12,7,0,0,2,14,5,10,12,0,0,0,0,6,13,10,0,0,0,0
```

Jeg lavede også derfor et script til, så jeg kunne fjerne sorteringen af billederne, så de var tilbage som før.

<script src="https://gist.github.com/Zxited/43ab0ed265138ceafe2231df79d29610.js"></script>

Så nu skulle jeg bruge en måde, at lave mine billeder om til den her format i en csv. Men det gjorde jeg næsten på en måde i mit originale script i KerasNumberRecognition. Så jeg tog noget inspiration derfra og prøvede, at lave det om til linjer med 785 kolonner (28x28 billede = 784 + 1 til mærkaten). Der var en masse besvær med, at omforme arrays til de rigtige dimensioner. Efter en del Python dokumentation havde jeg kodet dette.

<script src="https://gist.github.com/Zxited/ce580def221b033e754e0bd5af46b33f.js"></script>

Koden indlæser csv filen og laver billederne flade og om til den rigtige størrelse array, samt dividere dem, deler dem op i TRAIN og VAL sæt og smider dem til sidst i en csv fil med mærkaten.

Jeg fejlede første gang jeg forsøgte, at træne på min data, da den ikke kunne få en høj præcision. Grundet var jeg ikke første gang havde holdt mig til 2 kolonner, en til datapunkterne og en til mærkaten. Det gjorde så, at det første datapunkt var en streng med comma separeret tal. Dette fungerede ikke ret godt. Jeg har samtidig også valgt, at dele sættet på i TRAIN og VAL for den har noget, at validere med efterhånden som den træner.

Jeg startede AutoML med denne kommando:
`mlnet auto-train -d train_images.csv -v val_images.csv -i 784`

Og boom det gav resultater!

![](/assets/img/posts/2020-04-12-MLNET-AutoML/2020-04-12-20-20-11-2020-04-12-MLNET-AutoML.png)

Efter at have givet op på AutoML og var begyndt på, at omdanne deres eksempel kode, kom jeg i tanke om, at man i AutoML kunne definere indekset for csv filerne eller datasættene som de kalder det. Så det gjorde jeg i stedet med et indeks på 784 for mærkaterne.

Så 97,37% på den bedste algoritme efter en halv time, er slet ikke dårligt.

## Begrænsninger ved ML.NET og AutoML
Deep Learning understøttelse? Neurale Netværk?

## Kilder 

### Microsoft

- ML.NET - Get started in 10 minutes<br><https://dotnet.microsoft.com/learn/ml-dotnet/get-started-tutorial/intro>

- Load data into Model Builder<br><https://docs.microsoft.com/en-us/dotnet/machine-learning/how-to-guides/load-data-model-builder#set-up-image-data-files>

- Machine Learning at Microsoft with ML.NET<br><https://arxiv.org/pdf/1905.05715.pdf>

- ML.NET CLI Command reference<br><https://docs.microsoft.com/en-us/dotnet/machine-learning/reference/ml-net-cli-reference>

- Automate model training with the ML.NET CLI<br><https://docs.microsoft.com/en-us/dotnet/machine-learning/automate-training-with-cli>

- ML.NET Machine Learning samples<br><https://github.com/dotnet/machinelearning-samples>

- ML.NET Getting started Multi class Classification AutoML sample<br><https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/getting-started/MulticlassClassification_AutoML>

- ML.NET Community Samples<br><https://github.com/dotnet/machinelearning-samples/blob/master/docs/COMMUNITY-SAMPLES.md>

### Numpy
- Transpose<br><https://docs.scipy.org/doc/numpy/reference/generated/numpy.transpose.html?highlight=transpose#numpy.transpose>
- Stack<br><https://docs.scipy.org/doc/numpy/reference/generated/numpy.stack.html#numpy.stack>
- Hstack<br><https://docs.scipy.org/doc/numpy/reference/generated/numpy.hstack.html>
- Concatenate<br><https://docs.scipy.org/doc/numpy/reference/generated/numpy.concatenate.html#numpy.concatenate>
- Reshape<br><https://docs.scipy.org/doc/numpy/reference/generated/numpy.reshape.html?highlight=reshape>
- Flatten<br><https://docs.scipy.org/doc/numpy/reference/generated/numpy.ndarray.flatten.html>

### Andet

- Conversion og list to 2D array<br><https://stackoverflow.com/questions/21046417/python-conversion-of-list-of-arrays-to-2d-array>
- String to 2D numpy array<br><https://stackoverflow.com/questions/34401709/convert-string-to-2d-numpy-array>

- Pandas Data frame<br><https://www.tutorialspoint.com/python_pandas/python_pandas_dataframe.htm>

- ML.NET GPU Support<br><https://github.com/dotnet/machinelearning/issues/86>

- Reading CSV files in python<br><https://pythonspot.com/reading-csv-files-in-python/>

- Python create directory if does not exist<br><https://www.tutorialspoint.com/How-can-I-create-a-directory-if-it-does-not-exist-using-Python>

- Create a nested directory safely in Python<br><https://stackoverflow.com/questions/273192/how-can-i-safely-create-a-nested-directory>

- List alle files of a directory using Python<br><https://stackoverflow.com/questions/3207219/how-do-i-list-all-files-of-a-directory>
