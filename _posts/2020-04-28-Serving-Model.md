---
layout: post
title:  "Servering af ML Model"
author: Mike Meldgaard
date:   2020-4-28 #01:00:12 +0530
category: DevOps
summary: Machine learning model sat i produktion.
thumbnail: development.png
---

# Servering af ML model til produktion

Kør en servering med GPU:
```
docker run -it --rm --gpus all -p 8501:8501 --mount type=bind,source=/home/marianne/container-data/models,target=/models -e MODEL_NAME=prod -t tensorflow/serving:latest-gpu
```



# Kilder
- Train and serve a TensorFlow model<br><https://www.tensorflow.org/tfx/tutorials/serving/rest_simple#install_tensorflow_serving>
- TensorFlow Serving with Docker<br><https://www.tensorflow.org/tfx/serving/docker>
- tf.keras.models.save_model<br><https://www.tensorflow.org/api_docs/python/tf/keras/models/save_model?version=nightly>
- Use a GPU<br><https://www.tensorflow.org/guide/gpu#overview>
- Storing values in a 3D array to csv<br><https://stackoverflow.com/questions/46852741/python-storing-values-in-a-3d-array-to-csv>
- Hyperparameter Tuning with the HParams Dashboard<br><https://www.tensorflow.org/tensorboard/hyperparameter_tuning_with_hparams#3_start_runs_and_log_them_all_under_one_parent_directory>
- Tensorflow Serving Github<br><https://github.com/tensorflow/serving>
- How do I check whether a file exists without exceptions?<br><https://stackoverflow.com/questions/82831/how-do-i-check-whether-a-file-exists-without-exceptions>