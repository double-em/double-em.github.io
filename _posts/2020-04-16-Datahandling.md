---
layout: post
title:  "Forbehandling af data"
author: Mike Meldgaard
date:   2020-4-16 #01:00:12 +0530
category: MachineLearning
summary: Forbehandling og trækning af data fra API til brug i machine learning modeller.
thumbnail: development.png
---

# Forbehandling af data
I machine learning er behandlingen af data meget vigtig. Da der er flere måder, at behandle manglende værdier eller omregne værdier til brugbare værdier. Dette var noget jeg også var nød til, at beskæftige mig med i forhold til vores projekt, da jeg ikke direkte kunne bruge værdierne fra kundens API.

Man kan bruge forskellige måder til, at håndtere manglende værdier f.eks. at erstatte dem med den gennemsnitlige værdi for den kolonne. Eller f.eks. ved kategorier, at erstatter en manglende værdi med den kategori der er oftest brugt.

## Trækning af data fra API i Python
Jeg startede med, at arbejde på, at hente dataene fra API'en. Så først undersøgte jeg, hvordan API'en kaldes på deres online platform, hvor jeg fandt ud af, at den tager 7 værdier i en POST request for, at give data tilbage. Jeg trækker antal produceret produkter og nedbruds historikken.

Jeg behandler så den json string jeg får tilbage.

<script src="https://gist.github.com/Zxited/815495fa22e73d03913821c0dbb40166.js"></script>

Herover ses, hvordan jeg behandler nedbruds historikken. Det jeg gør er, at jeg laver et nyt dictionary som skal være de "rene" omformet værdier, så jeg senere kan behandle dem.

Da nogen af nedbrudene ikke har en kategori bliver de sat til "Uncategorised", da det er brugt af kunden selv, men nogen af de tidlige nedbrud har ikke en kategori kunne jeg se, da jeg undersøgte dataene.

Samtidig er nogen af de tidlige "comment" felter brugt som kategori felter, så jeg tjekker om feltet kun indeholder tal. Hvis det er tilfældet laver jeg det om til kategorien i stedet for. 

Mange "comment" felter har ikke en kommentar til nedbruddet, så de bliver også fyldt med "Uncategorised".

Jeg laver til sidst en dataframe ud fra det dictionary jeg defineret tidligere som nu har alle de "rene" værdier jeg skal bruge.

Jeg begynder så herefter, at lave dataene om til noget jeg kan bruge i mine machine learning modeller.

<script src="https://gist.github.com/Zxited/a610e66a46935e39163e494625e94cdc.js"></script>

Som kan ses ovenfor laver jeg et nyt dictionary, hvor alle mine omregnede værdier skal være.

Jeg tager den tidligere behandlet data og laver om til brugbar data. De data jeg har tænkt mig, at bruge er data der gerne skulle kunne repræsenteres som tal.

Så det jeg vil forudsige eller komme frem til er antallet af dage til næste vedligeholdelse. Antallet af dage til næste vedligehold er afhængig af:

- Dag på ugen
- Dage siden sidste vedligehold
- Nedbrud siden sidste vedligehold
- Nede tid siden sidste vedligehold
- Antal produceret vare siden sidste vedligehold

Det er de informationer jeg har mulighed for, at beregne og trække ud af den tilgængelige data som jeg synes giver mening.

Jeg ligger target værdien "days_to_maintenance" ind i sidste kolonne.

Jeg laver til sidst det sidste dictionary om til en dataframe og returnere dens værdier til ML scriptet.

Når ML scriptet modtager dataene, bliver alle felter undtagen target Min Maxed Scaled, så de er mellem 0 og 1.

# Kilder
- Converting string to Datetime in Python<br><https://stackabuse.com/converting-strings-to-datetime-in-python/>
- Datetime<br><https://docs.python.org/2/library/datetime.html>
- isDigit<br><https://docs.python.org/2/library/stdtypes.html#str.isdigit>
- Pandas Dataframe<br><https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html>
- scikit-learn LabelEncoder<br><https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html#sklearn.preprocessing.LabelEncoder.get_params>
- scikit-learn preprocessing-scaler<br><https://scikit-learn.org/stable/modules/preprocessing.html#preprocessing-scaler>
- Datacamp Credit Card approval project<br>https://projects.datacamp.com/projects/558
- Python loop control<br><https://www.tutorialspoint.com/python/python_loop_control.htm>
- Python striptime<br><https://docs.python.org/3/library/datetime.html#strftime-and-strptime-behavior>
- Pandas query<br><https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#indexing-query>
- How to iterate over rows in a Dataframe in pandas<br><https://stackoverflow.com/questions/16476924/how-to-iterate-over-rows-in-a-dataframe-in-pandas>
- How to select rows from a Dataframe based on column values<br><https://stackoverflow.com/questions/17071871/how-to-select-rows-from-a-dataframe-based-on-column-values>