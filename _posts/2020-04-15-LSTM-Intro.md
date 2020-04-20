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
Når RNN tilføjer ny information, så transformere den egentlig alt informationen, da den bare køre en funktion på det nye input og det gamle. Dvs. at det hele bliver transformeret. Så RNN tager ikke højde for vigtigheden af det input den får og skære alt over samme kan.

LSTM er i form af celler som data flyder igennem. Disse celler har en tilstand. Men disse celler har også informationer om sidste celles tilstand og sidste celles gemte tilstand samt det nuværende input i time steppet. Dette giver LSTM mulighed for via. dens 3 porte i cellen, at differentiere mellem vigtigt og mindre vigtigt. Så de modificere kun dataene en smule i stedet for det hele og kan derfor vælge, at glemme eller huske forskellige ting.

![](/assets/img/posts/2020-04-15-LSTM-Intro/2020-04-20-14-41-58-2020-04-15-LSTM-Intro.png)
Man kan tænke på det lidt ligesom et samlebånd, hvor dataene flyder fra den ene ende til den anden.

## Arkitekturen af LSTM
Som nævnt tidligere består en LSTM celle af 3 porte, forget, input og output portene.

![](/assets/img/posts/2020-04-15-LSTM-Intro/2020-04-20-14-44-57-2020-04-15-LSTM-Intro.png)
Som kan ses her er der 2 baner som køre igennem. Hvor der kommer et input til cellen og et output fra cellen.

# Kilder
- Introduction to Long Short Term Memory<br><https://www.analyticsvidhya.com/blog/2017/12/fundamentals-of-deep-learning-introduction-to-lstm/>
- Stock Prices Prediction Using ML and DL<br><https://www.analyticsvidhya.com/blog/2018/10/predicting-stock-price-machine-learningnd-deep-learning-techniques-python/>
- Tensorflow LSTM Tutorial<br><https://www.tensorflow.org/tutorials/structured_data/time_series#single_step_model>
- Tensorflow Dataset<br><https://www.tensorflow.org/api_docs/python/tf/data/Dataset?version=nightly>
- <https://www.tensorflow.org/api_docs/python/tf/Tensor?version=nightly>
- Keras Cheat Sheet<br><https://github.com/haribaskar/Keras_Cheat_Sheet_Python>
- Microsoft Azure Cheat Sheet<br><https://cdn.analyticsvidhya.com/wp-content/uploads/2017/02/17090804/microsoft-machine-learning-algorithm-cheat-sheet-v6.pdf>
- Keras Activations<br><https://keras.io/activations/>
- Keras Advanced Activations<br><https://keras.io/layers/advanced-activations/>
- Cheatsheet - Common Machine Learning Algorithms<br><https://www.analyticsvidhya.com/blog/2015/09/full-cheatsheet-machine-learning-algorithms/>
- scikit-learn - Choosing the right estimator<br><https://scikit-learn.org/stable/tutorial/machine_learning_map/>
- Tensorflow Keras Layers LSTM<br><https://www.tensorflow.org/api_docs/python/tf/keras/layers/LSTM?version=nightly>
- Understanding slicing in Python<br><https://stackoverflow.com/questions/509211/understanding-slice-notation>
- Python Range<br><https://www.w3schools.com/python/ref_func_range.asp>
- Tensorflow Keras Layer<br><https://www.tensorflow.org/api_docs/python/tf/keras/layers/Layer?version=nightly>
- Tensorflow Keras Dropout<br><https://www.tensorflow.org/api_docs/python/tf/keras/layers/Dropout?version=nightly>
- Introduction to Recurrent Neural Networks<br><https://www.analyticsvidhya.com/blog/2017/12/introduction-to-recurrent-neural-networks/>



- Grid Search for model tuning<br><https://towardsdatascience.com/grid-search-for-model-tuning-3319b259367e>
- Practical Recommendations for Gradient-Based Training ofDeepArchitectures<br><https://arxiv.org/pdf/1206.5533.pdf>
- Artificial Neural Networks: How can I estimate the number of neurons and layers?<br><https://www.quora.com/Artificial-Neural-Networks-How-can-I-estimate-the-number-of-neurons-and-layers>
- Babysitting the learning process<br><https://cs231n.github.io/neural-networks-3/#baby>
- Tutorial: Optimizing Neural Networks using Keras<br><https://www.analyticsvidhya.com/blog/2016/10/tutorial-optimizing-neural-networks-using-keras-with-image-recognition-case-study/#nine>
- Machine Learning: What are some tips and tricks for training deep neural networks?<br><https://www.quora.com/Machine-Learning-What-are-some-tips-and-tricks-for-training-deep-neural-networks>
- Rohan & Lenny #3: Recurrent Neural Networks & LSTMs<br><https://ayearofai.com/rohan-lenny-3-recurrent-neural-networks-10300100899b>



- NN Overkill?<br><https://missinglink.ai/guides/neural-network-concepts/neural-networks-regression-part-1-overkill-opportunity/>
- NN Activation Functions<br><https://missinglink.ai/guides/neural-network-concepts/7-types-neural-network-activation-functions-right/>
- Artificial Neural Networks: Concepts and Models<br><https://missinglink.ai/guides/neural-network-concepts/complete-guide-artificial-neural-networks/>
- Hyperparameters: Optimization Methods<br><https://missinglink.ai/guides/neural-network-concepts/hyperparameters-optimization-methods-and-real-world-model-management/>

- Nvidia Container Runtime<br><https://developer.nvidia.com/nvidia-container-runtime>

- Build TensorFlow input pipelines<br><https://www.tensorflow.org/guide/data>

- Google Cloud AutoML<br><https://cloud.google.com/automl/>
- Regular Expressions Cheat Sheet<br><https://cheatography.com/davechild/cheat-sheets/regular-expressions/>
- TPUs vs GPUs for Transformers(BERT)<br><https://timdettmers.com/2018/10/17/tpus-vs-gpus-for-transformers-bert/>
- My Experience and Advice for Using GPUs in Deep Learning<br><https://timdettmers.com/2019/04/03/which-gpu-for-deep-learning/>
- Tensorflow Use a TPU<br><https://www.tensorflow.org/guide/tpu>

## Hyperparameter optimization
- Hyperas<br><https://github.com/maxpumperla/hyperas>
- Kopt<br><https://github.com/Avsecz/kopt>
- Talos<br><https://github.com/autonomio/talos>