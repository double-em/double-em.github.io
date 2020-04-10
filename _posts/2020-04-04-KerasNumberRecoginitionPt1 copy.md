---
layout: post
title:  "Hyperparameter tuning af et Neural Netværk"
author: Mike Meldgaard
date:   2020-4-4 #01:00:12 +0530
category: MachineLearning
summary: Hyperparameter tuning af et Neuralt Netværk (Keras Number Recognition Del 2).
thumbnail: development.png
---

# Hyperparameter Tuning

## Hvad er en Hyperparameter?
En hyperparameter er nøgle parameter i et neuralt netværk. En hyperparameter er en intern parameter der ikke kan estimeres ud fra data eller træning.
Når man tuner et neuralt netværk kan man ikke bare bruge Grid Search, da der er så mange parametre, at det ville være næsten umuligt.

Nogle vigtige paramtre, at kigge på når man optimere et netværk er:

- Type af arkitektur
- Antal af lag
- Antal af neuroner i et lag
- Regulations parametre
- Learning Rate
- Type af optimering / backpropagation teknik man bruger
- Dropout rate
- Vægt deling

Afhængig af typen af ens arkitektur i ens netværk kan der være andre parametre. F.eks. i et CNN, hvor der vil være convolutional filter size og pooling value.

Man skal forstå sit problem for, at finde gode parametre. Så man kan søge ud og se, hvad andre har gjort ned lignende problemer eller finde professionelle der omtaler og giver råd omkring diverse udfordringer.

