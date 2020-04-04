---
layout: post
title:  "Implementering af Deep Learning med MLP arkitektur i Keras"
author: Mike Meldgaard
date:   2020-4-4 #01:00:12 +0530
category: MachineLearning
summary: Implementering af Deep Learning med Multi-later Perceptron i Keras biblioteket for genkendelse af billeder.
thumbnail: development.png
---
# Implementering af Deep Learning med MLP arkitektur i Keras
I dette oplæg vil jeg forklare om, hvornår Neurale Netværk(NN) bør bruges, samtidig med en hurtig forklaring af, hvordan man håndtere en NN opgave. Jeg vil også forklare om og bruge Keras med TenforFlow som backend til, at løse opgaven.

## Hornår skal vi anvende Neurale Netværk?
Deep learning laver store fremskridt indenfor Machine Learning bl.a. ved, at genkende tale, billeder, data analyse, markeds analyse eller natural language processing (NLP) samt mange flere.

Hvad skal vi så tænke over inden vi løber ud og bruger denne fantastike teknologi?

- **Neurale Netværk kræver klar og informativ data og ofte meget af det.** Dette er fordi vi skal træne netværket og ligesom med mennesker lære den ting ved at gøre det mange gange.

- **Neurale Netværk fungere godt til komplekse problemer,** hvor vi som mennesker har svært ved, at se sammenhængen mellem mange forskellige værdier eller inputs. Dette er fordi Neurale Netværk tilhøre den gruppe af algoritmer kaldet "Representation Learning Algorithms", som nedbryder komplekse problemer ned i en simplere form, så de kan forståes. Traditionelle "Non-Representation Learning" har det svært ved samme komplekse opgaver.

- **Man skal vælge den rigtige type Neurale Netværk til den givne opgave.** Hver opgave har sit eget twist, så det er op til udvikleren, at løse opgaven effektivt. Det er f.eks. bedst, at bruge RNN til sekvens generations opgaver, eller CNN for billede relateret opgaver. Da der ikke findes en gylden arkitektur til alle opgaver.

- **Neurale Netværk kræver mange computere ressourcer.** Dette er fordi når man træner netværket udføres mange beregninger for, hver vægt eller bias. Dette kan derfor være dyrt og noget der skal tages med i overvejelsen også når man vælger type af Neuralt Netværk. Nogle netværk kan opnå et acceptabel resultat uden, at være det bedste til jobbet, men i stedet være mere effektivt, så det bruger mindre ressourcer. Hvad et acceptabelt resultat er, er noget der bedømmes ud fra opgavens krav.

Neurale Netværk er en speciel type af Machine Learning (ML) algoritmer.
ML workflow starter ofte med behandling af data, så model byggelse og til sidst model evaluering.

TODO liste for, at håndtere et Neuralt Netværks(NN) problem:
- Undersøg om NN giver en fordel over traditionelle algoritmer (Som i overvejelserne ovenover)
- Undersøg hvilke NN arkitekture der er bedst til opgaven.
- Definer NN arkitekturen i det programmeringssprog / bibliotek man har valgt.
- Konverter dataen til den rigtige format og del den op i sektioner / batches.
- Forbehandl dataen i forhold til ens behov.
- Forøg data for, at øge størrelsen og træne bedre modeller.
- Fodre netværket med batches af data.
- Træn netværket og overvåg ændringer i trænings og validerings data sættene.
- Test modellen med et andet data sæt og gem den til fremtidig brug.

### Hvordan forstår en maskine et billede?
For os som mennesker er det nemt, at se forskellen mellem en bil og en hest og vi forstår hurtigt helheden i billedet. Men en maskine er ikke opvokset med alle de sanser vi har og kan ikke "se" på samme måde vi ser. Et billede for en maskine er et 3D array af tal som repræsentere farve kanalerne for, hver pixel i billedet. Dette fenomen er kendt som "Semantic gap" fordi vi ser forskelligt fra en maskine.

Førhen forsøgte man, at bruge skabeloner, så maskinen bedre kunne forstå det. Disse skabeloner ville indeholder f.eks. ved et ansigt øjnenes placering eller afstand til næsen osv. Dette var ikke holdbart, da man skulle bruge flere og flere skabeloner for, at kunne identificere flere obejekter.

Man senere i 2012 efter et dybt neural netværk vandt ImageNet udfordringen styret mod dybe neurale netværk for mange forskellige netværk.

Forskellige sprog som Python, Lua, Java og Matlab er brugt til, at kode neurale netværk. Sammen med nogle af de mest populære biblioteker til billede opgaver:
- Caffe
- DeepLearning4j
- TensorFlow
- Theano
- Torch

Vi bruger TensorFlow i dette oplæg.

## Hvad er TensorFlow?
TensforFlow er et open source bibliotek brugt til numerisk beregning. De introducere "tensors" som er et multi-dimensionelt array, som udveksles mellem noderne som repræsentere matematiske operationer. Grundet den fleksible arkitektur kan TensorFlow køres på CPUer eller GPUer i en stationær computer, server eller mobile enheder med et enkelt API.

Fordel ved TensorFlow:
- Den har et "flow of tensors", så man har nemmere ved at visualisere, hver del af en graf.
- Kan trænes på CPU/GPU for distribueret beregning.
- Kan bruges på flere platform som PC, Server og Mobil.

TensorFlow v1 workflow:
- Byg en beregnings graf ved brug af enhver matematisk operation TensorFlow understøtter.
- Instanser variabler for, at kompilere variablerne defineret tidligere.
- Lav en session.
- Kør grafen i sessionen. Den kompileret graf er vidergiver til sessionen som starter eksekveringen.
- Luk sessionen

Lig mærke til det er for "v1", da der er kommet en "v2" som simplificere en del. Der eksitere ikke længere en session, da den er erstattet med et kald til en funktion. Det er ikke nødvendigt at instanere variablerne, da de behandles som i normal Python kode, så placeholders er væk.

TensorFlow v2 workflow:
- Definer variabler.
- Lav / definer modellen.
- Kompiler modellen.
- Træn og test modellen.

Overni disse ændringer, er der også kommet en @tf.function, som kan sættes over en funktion for højere ydeevne.

## Hvad er Keras?
Keras er et high-level neuralt netværks API skrevet i Python og kan køre på toppen af Tensorflow, CNTK eller Theano. Det er er lavet med fokus på hurtig eksperimentering, så man kan gå fra idé til resultat med så små forsinkelser som muligt.

Keras er brugt som et deep learning library når:
	1. Man har brug for nemme og hurtige protyper igennem brugervenlighed, modularitet og evnen til udvidelse.
	2. Support for både Convolutional- og Recurrent neurale netværk samt en kombination af begge.
Køre glidende på både CPU og GPU.

Keras tager de low-level biblioteker den køre på og udgiver deres interface på en mere "brugervenlig" måde. Dette kommer dog også med nogle begrænsninger, da Keras ikke rigtig kan gå ud over dens backend / low-level bibliotek den køre på, som kan gøre det svært, at implementere sine egne operationer grundet den mindre fleksibilitet på den front.

## Opsætning af vores miljø
Vi vil i vores program skulle bruge en række biblioteker:

- numpy
- Pandas
- Scipy og Pillow(Brugt til Scipy.misc.imread), men Scipy.misc.imread er udagået, så vi bruger imageio i stedet.
- imageio
- scikit-learn (sklearn)

Jeg bruger følgende pakker:
- numpy==1.18.2
- pandas==0.25.3
- pandas-datareader==0.8.0
- scikit-learn==0.22.2.post1
- imageio==2.8.0

Bemærk: Hvis du gerne vil se alle dine pakker installeret:
`pip freeze`
Hvis man vil indsætte alle de pakker man bruger som en del af dem der skal installeres når containeren køre(Hvis man har konfiguret det i ens Dockerfile), kan man overføre dem til en tekstfil:
`pip freeze > requirements.txt`
Som var en teknik jeg brugte i starten, hvor jeg brugte TensorFlows eget image. Indtil jeg byggede mit eget image, med de rigtige pakker, baseret på det image jeg brugte fra TensorFlow.

Samtidig skal vi også have installeret TensorFlow(tf), men dog ikke Keras, da vi bruger tf.keras i stedet, som er Keras indbygget i TensorFlow 2.0 der er bedre vedligeholdt og tilbyder bedre integration med TensorFlows funktioner og features.

Afhængig om man køre programmet lokalt eller i en container skal man installere pakkerne lokalt eller sørge for ens image har dem.

Bemærk det er nemmere, at have GPU understøttelse ved brug af containere, da CUDA Tools ikke er nødvendig for understøttelsen i containeren.

Jeg har gjort det, at jeg har taget TensorFlows docker image med gpu og python 3 support og installeret de nødvendige pakker i det image, så jeg bagefter kunne lave mit eget image som kunnes oploades til Docker Hub til senere brug.

For at bruge ens GPU til træning af netværket, skal man have nvidia og cuda drivers intalleret, sammen med nvidia-docker eller nvidia-docker2 som har swarm support og er nyere, så den bruger jeg.

Når man skal køre en container i docker med gpu, bruger man "--runtime=nvidia" eller "--gpus all" flagene.

Man kan teste om man kan køre containere med gpu understøttelse ved, at køre:
`sudo docker run --rm --runtime=nvidia -ti nvidia/cuda`
Og så køre kommandoen
`nvidia-smi`

Et eksempel output:
```
$ sudo docker run --rm --runtime=nvidia -ti nvidia/cuda
roo@2d0aad394700:/# nvidia-smi
Sat Apr  4 11:19:25 2020
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 440.64       Driver Version: 440.64       CUDA Version: 10.2     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  Geforce GTX 1070    Off  | 00000000:01:00.0  On |                  N/A |
| 16%   55C    P0    31W / 151W |    7880MiB / 8119MiB |     12%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|                                                                             |
+-----------------------------------------------------------------------------+
```

I en docker-compose kan man også definere runtime til at være nvidia.

For at starte mit program endte jeg ud i, at lave en scripts fil i stedet for den indbygget debug launch metode i visual studio code. Grundet at det gav mig fuld kontrol af containeren og jeg kunne sætte --gpus flaget:
```
docker build -t mmgamesfull/kerasnumberrecognition:latest .
docker run -it --rm --gpus all mmgamesfull/kerasnumberrecognition:latest
```

Efter at have løst alle de forskellige fejl omkring udgået pakker og nvidia driver fejl osv. Kan vi komme videre.

## Løs opgaven
Trin for trin for, at løse opgaven.
Data kan findes her: <https://datahack.analyticsvidhya.com/contest/practice-problem-identify-the-digits/>

### Trin 1
Importer alle de nødvendige biblioteker.
```
import os
import numpy as np
import pandas as pd

from imageio import imread
from sklearn.metrics import accuracy_score

import tensorflow as tf
from tensorflow import keras
```

Kontroller modellens tilfældighed ved, at sætte et seed.
```
seed = 128
rng = np.random.RandomState(seed)
```

Sæt stierne til ressourcerne.
```
root_dir = os.path.abspath('../..')
data_dir = os.path.join(root_dir, 'data')
```

Tjek om stierne eksistere.
```
os.path.exists(root_dir)
os.path.exists(data_dir)
```

### Trin 2
Læs data sættene.
```
train = pd.read_csv(os.path.join(data_dir, 'train.csv'))
test = pd.read_csv(os.path.join(data_dir, 'test.csv'))

sample_submission = pd.read_csv(os.path.join(data_dir, 'sample_submission.csv'))
```

Hvis man vil se toppen af ens datafil kan man udskrive den med:
`train.head()`

Hvert billede er repræsenteret som et numpy array.
Så for nemmere, at håndtere dataen opbevare vi billederne i numpy arrays.
```
temp = []
for img_name in train.filename:
    image_path = os.path.join(data_dir, 'Images', 'train', img_name)
    
    img = imread(image_path, as_gray=True)
    img = img.astype('float32')
    temp.append(img)

    image_train_count += 1

train_x = np.stack(temp)

train_x /= 255.0
train_x = train_x.reshape(-1, 784).astype('float32')

image_test_count = 0
temp = []
for img_name in test.filename:
    image_path = os.path.join(data_dir, 'Images', 'test', img_name)
    img = imread(image_path, as_gray=True)
    img = img.astype('float32')
    temp.append(img)

    image_test_count += 1

test_x = np.stack(temp)

test_x /= 255.0
test_x = test_x.reshape(-1, 784).astype('float32')
```

Bemærk vi bruger as_gray=True som essentielt er det samme som flatten=True i Scipy. Vi gør det fordi vi skal kun bruge det som sort-hvid og derfor fjerner vi de andre farvekanaler som giver os end del mindre datapunkter, da farven ikke har noget, at gøre med billedet i denne omgang.

Funktionen "np.stack(temp)" er en måde vi kan tage vores billede array og stable dem til den rigtige form / dimension.

Vi omdanner så vores markater til en matrix af klasser. Som bruges ved f.eks. crossentropy.
```
train_y = keras.utils.to_categorical(train.label.values)
```

Ved ML kan det være et problem, at verificere, at ens netværk køre rigtigt. Derfor splitter vi træningsdataen op, så vi får et valideringssæt og et træningsæt.
Vi sætter splittet til 70:30, hvor det 70% er træningssættet.

```
split_size = int(train_x.shape[0]*0.7)

train_x, val_x = train_x[:split_size], train_x[split_size:]

train_y, val_y = train_y[:split_size], train_y[split_size:]

```

Det samme gør vi for vores markater i sættet.
```
train.label.loc[split_size:]
```

Bemærk at .ix index'eren er udgået i pandas og blevet erstattet med .loc for label index og .iloc for positions index grundet, det forvirrede brugere, at den kunne begge.

### Trin 3
Vi kommer nu til punktet, hvor vi skal have bygget en model. Vi bruger 3 lag denne gang som er input, hidden, output. Antallet af neuroner ved input og output er fastlåst, da vores input er et 28x28 billede som giver 784 neuroner og vores output er tallene 0 - 9, som så giver 10 output neuroner. Vi vælger 50 for det gemte lag, men det kan man optimere, hvis man synes.

Vi starter med, at definere variablerne.
```
input_num_units = 784
hidden_num_units = 50
output_num_units = 10

epochs = 5
batch_size = 128
```

Så importere vi modulerne vi skal bruge for vores model og bygger en model som beskrevet oppe over. Vu bruger "relu" som aktiverings funktion og softmax til vores output, da vi gerne vil have en sandsynlighed for, hvilket af de 10 tal den tror det er på billedet.

```
from tensorflow.keras import Sequential
from tensorflow.keras.layers import Dense

model = Sequential([
    Dense(hidden_num_units, input_dim=input_num_units, activation='relu'),
    Dropout(dropout_ratio),
    Dense(hidden_num_units, input_dim=hidden_num_units, activation='relu'),
    Dense(output_num_units, input_dim=hidden_num_units, activation='softmax'),
])
```
Bemærk Dense() bruger ikke længere "output_dim=" for, at definere outputtets dimensioner, men det hedder i stedet "units" og kan bare skriver som det første tal.

Efter første lag i modellen er det teknisk set ikke nødvendigt, at definere input størrelsen længere. Men jeg har gjort dette for, at det er nemmere, at forstå.

Vi bruger "Adam" som vores optimerings funktions algoritme, som er en effektiv variant af "Gradient Decent" algoritmen. Tab bliver beregnet med kategorisk crossentropy.

Vi kompilere nu vores model med de ønskede funtioner.
```
model.compile(loss='categorical_crossentropy', optimizer='adam', metrics=['accuracy'])
```

Vi kan nu træne vores model!
```
trained_model = model.fit(train_x, train_y, epochs=epochs, batch_size=batch_size, validation_data=(val_x, val_y))
```
Bemærk førhen var "epochs" parameteren kaldet "np_epochs".

Ved kørsel af programmet skulle vi gerne få et output svarende til følgende:
```
1
Epoch 1/5
2020-04-04 14:55:55.946689: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcublas.so.10
268/268 - 1s - loss: 1.0428 - accuracy: 0.6165 - val_loss: 0.3044 - val_accuracy: 0.9229
2
Epoch 2/5
268/268 - 1s - loss: 0.3171 - accuracy: 0.9208 - val_loss: 0.2010 - val_accuracy: 0.9490
3
Epoch 3/5
268/268 - 1s - loss: 0.2372 - accuracy: 0.9428 - val_loss: 0.1995 - val_accuracy: 0.9535
4
Epoch 4/5
268/268 - 1s - loss: 0.2145 - accuracy: 0.9491 - val_loss: 0.1738 - val_accuracy: 0.9599
5
Epoch 5/5
268/268 - 1s - loss: 0.1888 - accuracy: 0.9578 - val_loss: 0.1626 - val_accuracy: 0.9597
```

Tallene 1 - 5 imellem linjerne er vore "EpochCount()" klasse.

**(VALGFRIT)**
Hvis man køre det i en container eller remote, vil man nogle gange ikke kunne se, hvor langt den er nået i trænings processen. Det jeg har gjort for, at få en smule feedback fra den er, at jeg har lavet en Callback klasse, som udskriver, hvilken iteration den er igang med.
```
class EpochCount(keras.callbacks.Callback):
    def on_epoch_begin(self, epoch, logs={}):
        print(epoch + 1)
```

For at bruge vores Callback klasse "EpochCount()" skal vi have en instans af den og give den instans til model.fit() funktionen.
```
epoch_count = EpochCount()

trained_model = model.fit(train_x, train_y, epochs=epochs, batch_size=batch_size, validation_data=(val_x, val_y), verbose=2, callbacks=[epoch_count])

```

### Trin 4
Vi skal nu evaluere vores model.
Der findes flere måder, at evaluere ens model. Hvis man har et stort datasæt, så ville man splitte dataen op ligesom vi gjorde med validerings dataen, så man har et test sæt og et trænings sæt og derefter splitte træningsdataen op i validerings sæt og trænings sæt, for vi stadig har noget validering efter, hver epoch. Hvor man så efter, at have trænet sin model ville kunne teste den op mod ens test sæt, som netværket ikke kender til, da det er et sæt for sig selv og få en præcision på baggrund af test sættet. Det er vigtigt, at man ikke tester på den data netværket allerede har trænet på, da den kender til sættet i forvejen og det er derfor svært, at fange ting som overfitting.

I det her tilfælde, der på siden, hvor vi fik dataen er der en submission form, hvor man kan uploade sin .csv fil når ens netværk har gået det igennem, for at få et tal på, hvor præcist netværket har klaret det.

Så vi tager vores netværk og laver forudsigelse på vores test sæt.
```
pred = np.argmax(model.predict(test_x), axis=-1)
```
Bemærk at model.predict_classes er udgået og forsvinder i fremtiden fra biblioteket.
Man skal i stedet bruge "np.argmax(model.predict(x), axis=-1)" til multiclass classification predictions ved f.eks. softmax() som giver en sandsynligheds matrix.
Ellers ved binær klassifikation (Ja eller Nej / True or False) f.eks. Sigmoid() som giver en værdi mellem 0 og 1 bruger man "model.predict(x) > 0.5).astype("int32")".

Vi tager så vores submissions fil og definere, hvilke værdier der skal være i hver kolonne og udskriver den til vores data mappe som .csv igen.
```
sample_submission.filename = test.filename
sample_submission.label = pred

sample_submission.to_csv(os.path.join(data_dir, "test.csv"), index=False)
```

Hvis programmet køre lokalt på ens computer, vil den ligge lokalt på den mappe man har valgt. Hvis man dog køre programmet i en docker container, vil det være nødvendigt, at trække filen ud af containeren og ned på en lokale computer. Dette kan gøres ved:
`docker cp CONTAINER_NAME_GOES_HERE:/path/to/test.csv /path/to/location/`

Som i mit tilfælde med denne container vil se sådan her ud:
`docker cp kerasnumberrecognition:/data/test.csv .`
Punktummet i slutningen betyder, at den skal gemme filen i den mappe jeg står i med min terminal.

Man kan nu uploade sin fil til testeren og se sit flotte resultat. Men man kan også fin-tune sit netværk med flere lag og flere neuroner, som jeg kommer ind på i næste oplæg.

## Kilder
- Introduktion til implementering af NN i TensorFlow
<https://www.analyticsvidhya.com/blog/2016/10/an-introduction-to-implementing-neural-networks-using-tensorflow/>

- Brug og optimatisering af NN med Keras
<https://www.analyticsvidhya.com/blog/2016/10/tutorial-optimizing-neural-networks-using-keras-with-image-recognition-case-study/#nine>

- Migrering fra TensorFlow v1 til v2
<https://www.tensorflow.org/guide/migrate>

- imageio imread dokumentation
<https://imageio.readthedocs.io/en/stable/userapi.html#imageio.help>

- Overgang fra Scipy imread til imageio imread
<https://imageio.readthedocs.io/en/stable/scipy.html>

- TensorFlow v2 model.fit
<https://www.tensorflow.org/api_docs/python/tf/keras/Model#fit>

- Brug af tf.keras i stedet for Keras
<https://keras.io/#multi-backend-keras-and-tfkeras>

- TensorFlow Docker Hub images
<https://hub.docker.com/r/tensorflow/tensorflow>

- Brug af GPUer i Containers
<https://devblogs.nvidia.com/gpu-containers-runtime/>

- Docker Runtime options
<https://docs.docker.com/config/containers/resource_constraints/#access-an-nvidia-gpu>

- Dockerfile cheat sheet
<https://kapeli.com/cheat_sheets/Dockerfile.docset/Contents/Resources/Documents/index>

- numpy.stack
<https://docs.scipy.org/doc/numpy/reference/generated/numpy.stack.html?highlight=numpy%20stack#numpy.stack>

- Keras Utils to_categorical
<https://keras.io/utils/>

- pandas IX indexer udgået
<https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#ix-indexer-is-deprecated>

- Keras Sequential model
<https://keras.io/getting-started/sequential-model-guide/>

- tf.keras.layers.Dense
<https://www.tensorflow.org/api_docs/python/tf/keras/layers/Dense>

- Keras Callbacks
<https://keras.io/callbacks/>

- tf.keras.callbacks.Callback
<https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/Callback>

- tf.model.predict
<https://www.tensorflow.org/api_docs/python/tf/keras/Sequential#predict>

- tf predict_classes source code
<https://github.com/tensorflow/tensorflow/blob/v2.1.0/tensorflow/python/keras/engine/sequential.py#L324-L342>