---
layout: post
title:  "Forståelse Recurrent Neural Network (RNN)"
author: Mike Meldgaard
date:   2020-3-27 #01:00:12 +0530
category: MachineLearning
summary: Forståelse af RNN samt dens problemer med Vanishing og Exploding Gradient
thumbnail: learn.png
---

## Forståelse af RNN

### Hvad er RNN?
RNN står for Recurrent Neural Network og er kommet til pga. behovet for at håndtere varieret typer og længder af båede input og output værdier. Dvs. RNN kan håndtere sekvens input aka. den kan holde en gennemgående relation til forrige inputs i en sekvens af data for at skabe et samlet output. Dette kan være et time series problem, hvor tidspunktet / rækkefølgen for dataen i sekvensen har noget at gøre med den næste værdi. F.eks. i en sætning, som "Jeg køber ind" har rækkefølgen af bogstaverne / ordene stor betydning for forståelsen af sætningen.

### Hvordan ser det ud?
Hvis vi kigger på MLP(Multi-layer Peceptron) som kan have flere gemte lag. Hvor vi teknisk set kan smide, hvert ord ind fra en sekvens og få et output der afhænger af rækkefølgen.

![Calculated final model](/assets/img/posts/UnderstandingRNN/rnnMLP.png)

Men da alle deres vægte og bias er inviduelle kan vi ikke kombinere dem. For at kunne kombinere dem og få et lag skal de have ens vægt og bias. Grunden til vi vil kombinere dem er, at kunne have et lag med en given "historik" af tidligere inputs.

Så hvis vi kombinere de 3 lag får vi en Recurrent Neuron:

![Calculated final model](/assets/img/posts/UnderstandingRNN/rnnNeuron.png)

Som kan ses på billedet kan den tage hele sætningen som et input. Da vi kan holde værdierne internt i den nye block. Da den bare er en kombination af de gemte lag, som stadig relatere til hinanden i form af en lang kæde.

Hvis man folder RNN'en ud vil man se det som beskrevet ovenfor:

![Calculated final model](/assets/img/posts/UnderstandingRNN/rnnUnfolded.png)

Hvor x er input og y er output. t står for nuværende time step. Et time step er på et tidspunkt i rækkefølgen. F.eks. hvis vi har 4 bogstaver som input {h, e, l, l} vil det første time step være for 'h'.

I RNN tager vi tidligere udregnet tilstande for vores gemte lag med i overvejelsen. Dvs. vi tager vores tidligere udregnet tilstand og ganger med vores vægte i vores gemte lag plusset vores bias og ligger oveni vores input x ganget med vægtene for input'sne. Dette propper vi ind i en aktiverings funktion som f.eks. tanh. Til sidst får vi noget der kan skriver som:
`ht = tanh((Whh * ht-1 + bias) + (Wxh * xt))`