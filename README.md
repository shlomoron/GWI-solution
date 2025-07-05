# GWI-solution
This repository contains my solution (4st place, solo gold medal) for the Kaggle competition of [Yale/UNC-CH - Geophysical Waveform Inversion](https://www.kaggle.com/competitions/waveform-inversion). See also my summary [on Kaggle](https://www.kaggle.com/competitions/waveform-inversion/discussion/587500).  
# The steps to reproduce my solution  
## 1. Generating more data
[This notebook demonstrate creating CurveFaultB](https://www.kaggle.com/code/shlomoron/gwi-generate-cfb-public). I had similar ones to create CurveVel and StyleA/B.
## 2. Data to TFRecords
See [example notebook](https://www.kaggle.com/code/shlomoron/gwi-tfrecs-cfb). The hidden dataset is JSON file with you Kaggle API token, you have to generate one in order to interact with Kaggle datasets via the API.  
## 3. Find buckets path to TFRecords
In order to train on Colab, I feed the model the path to GCP buckets with TFRecords that are saved as Kaggle datasets. In order to find the buckets path we need to run one more notebook. Tha buckets paths changes every few days so the notebook need tobe run each time you want to use the datasets. See an example notebook [here](https://www.kaggle.com/code/shlomoron/gwi-gcp-path-tfrecs-x-cmprsd-val-f32). It also include buckets path to the validation and test set for the inference notebook. The output should be save to a [Kaggle dataset](https://www.kaggle.com/datasets/shlomoron/gwi-gcp-path-tfrecs-x-cmprsd-val-f32-ds).  
## 4. Train
I attach one of my Colab notebook for refernces, I had to chain several tens of such notebooks to train each model. [See the example notebook here](https://colab.research.google.com/drive/1kshcWCi8hbZtJVijVv2pez9PvAmxGjR_?usp=drive_link).   
## 5. Infer
Inference must also be done on TPU- for some strange reason, loading the model to GPU lead to lower accuracy. To infer, just construct the model exactly as in train and predict on the test set. [here is an example for one checkpint](https://colab.research.google.com/drive/1EuTwKgrXDsfE8yNLfgWcR7FMynUhTiwx?usp=sharing).  
## 6. Predicting test labels
I trained a clasiffying model to classify predicted vel images to families, [see the training notebook here](https://www.kaggle.com/code/shlomoron/gwi-classifier-1). It has the same archtecture as my seis-t-vel models, only it get as input vel instead of seis, and the ouptut is family prediction instead of vel. [Here is the inference notebook on the test set](https://www.kaggle.com/code/shlomoron/gwi-classifier-1-infer) and [here is the inferred labels as a dataset](https://www.kaggle.com/datasets/shlomoron/gwi-test-labels). It would be interesting to compare the family predictions against the groundthruth- I suspect either have a bug smewhere in my pipeline or my validation for the classifier is bad, because I expected better results, hence I thinl that maybe my classifier is not so good as I thought. But I have no way to know without comparing agains the true, hidden test labels.
## 7. Ensembling
Ensembling is, err...how shall I put it, a little bit over-complicated. I'll go over each step and hope I don't miss anything.  
At the beginning, I had one model which I trained with horizontal flip augmantation. Mid-training, I split it to two models, one comtinued to train on the data wiithout flipping, the second continued to train with all the data flipped. So I had two models, 'flipped' and 'non-flipped'. Additionaly, in the last week I trained a third model with emphasis on generated style data (while the former had more emphasis on CurveFault data). So I have 'flipped', 'non-flipped' and 'style' models. For each model, I save in each run of a colab notebook (for 'style' I had a tital of 10 runs, for the other two I had over 40) two checkpoints- the 'best' checkpoint which had the best validation score during the run, and the 'last' checkpoint, which was the last and was the checkpint from whoch I continued the next run.  
For ensembling, I ensembled for each model, separately, several of the 'last' checkpints and the 'best' checkpoints. Then I ensembles the 'last' ensembles of the three models with weights chosen separatly for family, and did the same separately for the 'best' ensembles. Then I ensembled the ensembles of akk the 'last' and 'best' with weight of 0.6-0.4, and finally applied postprocessing on the ultra-mega ensemble. And if you are tired just trying to follow it, imagine me doing it...ensembling is alwyse a bit of black magic, you see :)  
SO lets list the notebooks.  
First, the checkpoint predictions.  
[Non-flip 1](https://www.kaggle.com/code/shlomoron/gwi-preds-32-36-ds-to-notebook)  
[Non-flip 2](https://www.kaggle.com/code/shlomoron/gwi-preds-37-52-ds-to-notebook)  
[Flip 1](https://www.kaggle.com/code/shlomoron/gwi-preds-32-36-flip-ds-to-notebook)  
[Flip 2](https://www.kaggle.com/code/shlomoron/gwi-preds-37-42-flip-ds-to-notebook)  
[Flip 3](https://www.kaggle.com/code/shlomoron/gwi-preds-43-45-flip-ds-to-notebook)  
[Style](https://www.kaggle.com/code/shlomoron/gwi-preds-6-10-style-ds-to-notebook)  

Ensembles of 'last' checkpoints:  
[Ensemble non-flip last](https://www.kaggle.com/code/shlomoron/gwi-val-ensemble-noflip)  
[Ensemble flip last](https://www.kaggle.com/code/shlomoron/gwi-val-ensemble-flip)  
[Ensemble style last](https://www.kaggle.com/code/shlomoron/gwi-val-ensemble-style)  

Ensembles of 'best' checkpoints:  
[Ensemble non-flip best](https://www.kaggle.com/code/shlomoron/gwi-val-ensemble-noflip-best)  
[Ensemble flip best](https://www.kaggle.com/code/shlomoron/gwi-val-ensemble-flip-best)  
[Ensemble style best](https://www.kaggle.com/code/shlomoron/gwi-val-ensemble-style-best)  

Ensembles of three models together:  
[Last ensemble](https://www.kaggle.com/code/shlomoron/gwi-ensemble-flip-noflip-style)  
[Best ensemble](https://www.kaggle.com/code/shlomoron/gwi-ensemble-flip-noflip-style-best)  

[The final ensemble](https://www.kaggle.com/code/shlomoron/gwi-ensemble-best-nobest-w-style-4-6-weights/notebook)  

[Postprocessing the final ensemble- final submission](https://www.kaggle.com/code/shlomoron/gwi-ensemble-postprocessing-2/notebook)  




