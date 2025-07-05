# GWI-solution
This repository contains my solution (4st place, solo gold medal) for the Kaggle competition of [Yale/UNC-CH - Geophysical Waveform Inversion](https://www.kaggle.com/competitions/waveform-inversion). See also my summary [on Kaggle](https://www.kaggle.com/competitions/waveform-inversion/discussion/587500).  
# The steps to reproduce my solution  
## 1. Generating more data
[This notebook demonstrate creating CurveFaultB](https://www.kaggle.com/code/shlomoron/gwi-generate-cfb-public). I had similar ones to create CurveVel and StyleA/B.
## 2. Data to TFRecords
See [example notebook](https://www.kaggle.com/code/shlomoron/gwi-tfrecs-cfb). The hidden dataset is JSON file with you Kaggle API token, you have to generate one in order to interact with Kaggle datasets via the API.  
### 3. Find buckets path to TFRecords
In order to train on Colab, I feed the model the path to GCP buckets with TFRecords that are saved as Kaggle datasets. In order to find the buckets path we need to run one more notebook. Tha buckets paths changes every few days so the notebook need tobe run each time you want to use the datasets. See an example notebook [here](https://www.kaggle.com/code/shlomoron/gwi-gcp-path-tfrecs-x-cmprsd-val-f32). It also include buckets path to the validation and test set for the inference notebook.  
## 4. Train
I attach one of my Colab notebook for refernces, I had to chain several tens of such notebooks to train each model. [See the example notebook here](https://colab.research.google.com/drive/1kshcWCi8hbZtJVijVv2pez9PvAmxGjR_?usp=drive_link).   
## 5. Infer
Inference must also be done on TPU- for some strange reason, loading the model to GPU lead to lower accuracy. To infer, just construct the model exactly as in train and predict on the test set. [here is an example for one checkpint](https://colab.research.google.com/drive/1EuTwKgrXDsfE8yNLfgWcR7FMynUhTiwx?usp=sharing).  

