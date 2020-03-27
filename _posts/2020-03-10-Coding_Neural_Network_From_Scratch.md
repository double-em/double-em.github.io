---
layout: post
title:  "Forståelse og Kodning af Neuralt Netværk fra bunden i Python"
author: Mike Meldgaard
date:   2020-3-27 #01:00:12 +0530
category: MachineLearning
summary: Kodning af Neuralt Netværk fra bunden i Numpy med Full Batch Gradient Decent
thumbnail: development.png
---

### Forståelse og Kodning af Neuralt Netværk fra bunden

Et neuralt netværk er en sammesætning af neuroner. Det tager flere input. Som går gennem flere gemte lag.

Grundstenen for neurale netværk er en Perceptron. En perceptron  tager flere input og producere et output.

![Perceptron](/assets/img/posts/CodingNetworkFromScratch/Perceptron.png)

Tag ovenstående illustration af en Perceptron ville have 3 input værdier kaldet x1, x2, x3 og en vægt til hvert input kaldet w1, w2, w3. Samtidig har en Perceptron en Bias som er en værdi vi kan bruge til at tilpasse funktionen, så vi kan opnå et bedre fit på dataen med vores forudsigelse. Bias har en vægt af 1. Så vi bruger Bias værdien direkte uden at blive vægtet.

Dvs. hvis vi skriver denne Perceptrons linære repræsentation får vi **(w1x1 + w2x2 + w3x3 + 1b)**, hvor vi kan sætte et threshold på 0 for, at udregne resulatet, så vi får **w1x1+w2x2+w3x3+1b>0**.

Perceptrons er linære og blev viderudviklet til Kunstige Neuroner som gør brug af ikke-linære aktiverings funktioner.

#### Aktiverings Funktioner
Der findes flere aktiverings funktioner som f.eks. nedenstående:

![Activation Functions](/assets/img/posts/CodingNetworkFromScratch/AktivationFunc.png)

Aktiverings funktioner giver os muligheden for at estimere eller tilpasse ikke linære eller komplekse funktioner for at få et bedre aktiverings punkt for neuronen.

#### Forward Propagration, Back Propagration og Epochs
Når man får netværket til at udregne resultatet kaldes det Forward Propagation.

Back Propagation bruges til at indstille vægtene og bias værdierne på baggrund af fejlmængden/tabet også kaldet Cost Function af vores Output lag.

En iteration af Forward og Back Propagration er kaldet en trænings iteration aka Epoch.

#### Multi-layer perception (MLP)
MLP kan have flere gemte lag imellem input og output lagene. Som åbner en masse muligheder for mere avanceret og komplekse modeller.

![Activation Functions](/assets/img/posts/CodingNetworkFromScratch/MLP.png)

MLP er fuldt forbundet hvilket betyder, at alle noder I hvert lag undtagen input og output – lagene er fuldt fobundet med hver node i tidligere og næste lag.

#### Gradient Decent
Er en træningsalgoritme findes to varianter
- Full Batch Gradient Descent (Full Batch)
- Stochastic Gradient Descent (SGD)

**Full Batch:** Bruger hele træningsættet per vægt. F.eks. man har 10 datapunkter og 2 vægte. Der bruger den alle 10 punkter til at beregne w1 og igen alle 10 punkter til w2.

**SGD:** Bruger 1 til flere men aldrig hele sættets datapunkter til en vægt. Dvs. At vi bruge måske punkt 1 til vægt 1 og derefter punkt 1 til vægt 2. Vi kan også bruge flere punkter til hver vægt. 

Trænings algoritmer er brugt til at minimere Cost funktionen.

#### Trin for trin hvordan et Neuralt Netværk fungere med Full Batch
Hvis vi tager følgende netværk

![Neural Network Example](/assets/img/posts/CodingNetworkFromScratch/NNExample.png)

Hvert tal ud fra en streg er en vægt og hver streg til en b - værdi er nodens bias.