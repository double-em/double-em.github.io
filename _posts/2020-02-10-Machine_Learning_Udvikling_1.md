---
layout: post
title:  "Machine Learning Udvikling 1"
author: Mike Meldgaard
date:   2020-02-10 #01:00:12 +0530
category: MachineLearning
summary: Machine Learning de første skridt
thumbnail: learn.png
---
Dette er en status på min udvikling indenfor Machine Learning

<h3>Machine Learning Fundamentals</h3>

Hvad har jeg udført?:
- Datacamp Pba Track
    - Supervised Learning with scikit-learn, Chapter 1 - 2
    - Opgaver omkring Supervised Learning
        - Classification
        - Linear Regression
        - Ridge Regression
        - Lasso Regression
- LinkedIn Learning
    - Artificial Intelligence Foundations: Machine Learning
    - Artificial Intelligence Foundations: Thinking Machines

<h3>Begreber og Forståelse</h3>
Imens jeg har undersøgt Machine Learning er jeg støt på en række begreber. Disse begreber skriver jeg herunder og min forståelse af dem indtil videre.

Indenfor Machine Learning er der nogle begreber som betyder det samme.
"Features" kaldes også for "Predictor variables" og "Independent variables".
Og "Target variable" kalder også for "Dependen variable" og "Response variable".

Supervised Learning
Målet med Supervised Learning er at bygge en model ud fra kendt data med et kendt resultat, så man i fremtiden kan give et resultat på nye data. Det kræver man har en række data med markater. f.eks. at man vil finde værdien for et hus, er der en række uafhængige værdier, som kunne være "Grundstørrelse, Husstørrelse, Antal værelser, Byggeår". Ud fra disse uafhængige værdier kan vi fastslå den afhængige værdi, som er husets pris.

Inden man prøver at træne en maskine på en given data, laver man "Exploratory Data Analysis(EDA)" for at forstå ens datasæt. Det kan være med Visual EDA som kan vise forskellige grafer som illustrere værdiers sammenhæng i forhold til hinanden.

Supervised Learning kræver man har en ekspert indefor feltet som kan fortælle det rigtige resultat. I ekspemplet med huset ville det være en egendomsmægler som fortalte, hvad alle husene var værd. Hvor man herefter ville træne maskinen på en del af datasættet og teste den på den anden del af sættet.

Dette problem ved at fastsætte en huspris ville være et Reggression problem, da man prøver at finde en værdi der kan ændres og kan ikke puttes i en kategori som man ellers gør ved Classification. I Classification prøver man at finde kategorien for den givne data, dette kunne f.eks. være at sætte forskellige sange i kategorier som Rap, Jazz, Rock osv.

For at finde ud af, hvor præcis en model er findes der forskellige måder at regne det på afhængig af typen af model.

Ved Classification er nok den mest simple. Da man dividere rigtige svar med antallet af forespørgsler.

Ved Linear Regression beregner man R^2 i stedet for metrisk. Da man i Regression måler, hvor tæt den kommer på målet.

Ved Ridge Regression beregner man OLS + Sum af Koefficienterne^2 * Alpha.

Ved Lasso Regression bruger man OLS + Abselut værdi af hver Koefficient^2 * Alpha

Når man skal effektivisere en model skal man tænke over Discision Boundary, Model Complecity, Overfitting og Underfitting.