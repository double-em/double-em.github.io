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
Trin for trin til, at løse opgaven.

### Trin 1
Importer alle de nødvendige biblioteker
```
import os
import numpy as np
import pandas as pd

from imageio import imread
from sklearn.metrics import accuracy_score

import tensorflow as tf
from tensorflow import keras
```

Kontroller modellens tilfældighed ved, at sætte et seed
```
seed = 128
rng = np.random.RandomState(seed)
```

Sæt stierne der skal bruges









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