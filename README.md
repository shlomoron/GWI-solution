# GWI-solution
This repository contains my solution (4st place, solo gold medal) for the Kaggle competition of [Yale/UNC-CH - Geophysical Waveform Inversion](https://www.kaggle.com/competitions/waveform-inversion). See also my summary [on Kaggle](https://www.kaggle.com/competitions/waveform-inversion/discussion/587500).  
# The steps to reproduce my solution  
## 1. Generating more data
[This notebook demonstrate creating CurveFaultB](https://www.kaggle.com/code/shlomoron/gwi-generate-cfb-public/notebook). I had similar ones to create CurveVel and StyleA/B.
## 2. Train
I attach one of my Colab notebook for refernces, I had to chain several tens of such notebooks to train each model. [See the example notebook here](https://colab.research.google.com/drive/1kshcWCi8hbZtJVijVv2pez9PvAmxGjR_?usp=drive_link).  
## 3. Infer
Inference must also be done on TPU- for some strange reason, loading the model to GPU lead to lower accuracy. To infer, just construct the model exactly as in train and predict on the test set. [here is an example](https://colab.research.google.com/drive/1EuTwKgrXDsfE8yNLfgWcR7FMynUhTiwx?usp=sharing).  

