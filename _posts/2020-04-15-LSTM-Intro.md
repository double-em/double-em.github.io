---
layout: post
title:  "Introduktion til LSTM"
author: Mike Meldgaard
date:   2020-4-15 #01:00:12 +0530
category: MachineLearning
summary: Introduktion til Long Short Term Memory(LSTM) og forståelse herfor.
thumbnail: development.png
---

# Long Short Term Memory(LSTM)
Ifølge seneste gennembrud er LSTM blevet observeret som den mest effektive løsning på time series problemer. Altså problemer, hvor rækkefølgen betyder noget. Givet en række data og så forudsige næste data i den række f.eks.

LSTM har en fordel over konventionelle feed-forward neurale netværk sådan som MLP, hvor dataene er skubbet fra input til gemte lag til output (feed forward) og så noget back propagation til, at optimere netværket. Men LSTM har også en fordel over RNN, da LSTM har evnen til, at vælge, at huske mønstre i lange perioder.

## Begrænsninger ved RNN
Hvis man f.eks. vil forudsiger en akties pris, så vil man ved en konventionel feed-forward NN model have svært ved, at tage tid med i dens overvejelser, da ved sådan et netværk vil den se alle scenarier som individuelle isoleret scenarier, og derfor ikke tiden med. Så ved f.eks. ved en akties pris, der er det ikke kun åbningsprisen eller volumen af aktien, men også tidligere dages pris aka. trenden.

Så bruger vi da bare RNN? Ja man ville godt kunne løse det med RNN. Men man ville hurtigt løbe ind i problemer ved længere trends eller sammenhænge, da RNN ville fungere, men kun i det korte løb, da tidligere inputs features vil blive mindre og mindre pga. den måde den tager tidligere time steps med i dens beregning.

Så RNN er rigtig effektive til korte sammenhæng. Men hvis vi skal bruge en tidligere væsentlig værdi, så har den glemt det pga. Vanishing Gradient problemet. Det samme ved feed forward. Da ved feed forward NN's der er aktiverings funktionens afledte værdier som er i fejl funktionen ganget flere gange efterhånden som vi bevæger tilbage mod input laget, fordi et givet lags fejl er et produkt af tidligere lags fejl. Det gør, at de første lag ville være svære, at træne da gradienten ikke længere er særlig tydelig.

Så RNN husker kun kortvarigt. Men det kan fikses ved, at bruge en modificeret version af RNN som er LSTM.

## Forbedringer over RNN
