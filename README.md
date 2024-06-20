# Data descriptionxd
This dataset comprises resting EEG data from 122 college-aged participants, who also participated in the Openneuro probe selection task. Each participant engaged in alternating one-minute periods of eyes-open and eyes-closed tasks, marked by triggers such as OCCOCO or COOCOC. The data were collected between 2008 and 2010 at the John J.B. Allen lab at the University of Arizona.  

All participants were reliably scored as either high or low on the Beck Depression Inventory (BDI), with some also undergoing clinical interviews. The experiment included two resting state sessions: the first one immediately after EEG hookup, lasting 6 minutes, and the second one about an hour after task performance, also lasting 6 minutes. Note that participant 516 lacks second resting data, and participant 544 was excluded from all analyses due to unstable BDI scores between mass and lab assessments.  

It is important to note that some (possibly all) participants may have mislabeled horizontal (HEOG) and vertical (VEOG) electrooculogram channels, and some files have had certain channels interpolated, with no raw data available for reversion. Despite these issues, the first resting session's data quality is high.  


## ICA

# Introduction
EEG data presents challenges due to its low signal-to-noise ratio (SNR). Effective preprocessing is essential for models to accurately interpret EEG data, making artifact removal methods critical for EEG-based BCI.

This project aims to evaluate the effectiveness of different artifact removal methods. Utilizing an RSVP EEG visual dataset, we tested various methods such as ASR, ICA, and Autoreject. The results are evaluated based on the performance of EEGNet, with the evaluation metric being the macro F1 score.

# Model Framework
## Preprocessing steps

……

## Model

……

# Validation
……

# Usage
Environment run `pip install -r requirements.txt`

Data preparation

1. Download the **Raw EEG data** of sub1 from here and Unzip it.
2. Download the **image_metadata.npy** from here
3. Download the **category_mat_manual.mat** from here
4. Put them under data/LargeEEG/raw and execute **python preprocess.py**. Now we got **0000.set** and 1000.set. The data files are named in XXXX.set.
    * First digit: 0 means using all channels without channel selection. 1 means otherwise.
    * Second digit: 0 means not using bandPass filtering. 1 means otherwise.
    * Third digit:
       * 0: no artifact removal method used.
       * 1: ICA
       * 2: ASR
       * 3: autoreject
       * 5: ASR+ICA
    * Fourth digit: 0 means not taking the ERP mean. 1 means otherwise.
5. Use EEGLab for preprocessing to obtain 0100.set 1100.set 1110.set 1120.set 1150.set
6. execute python **preprocess2.py** and input the following 4 digits to generate respective training files.
  0000
  0020
  0100
  1000
  1100
  1110
  1120
  1130
  1150
  Run

To reproduce experiment1, excute **python exp1.py** and then **python test_exp1.py**.

To reproduce experiment2, excute **python exp2.py** and then **python test_exp2.py**.

In the **experiments** folder, each subfolder contains the training results for specific dataset. For a summary of the performance, you can refer to the **results.txt**.

# Results

## Observation
Given the imbalanced nature of the dataset, we use macro F1 as our main metric. From Experiment 2, we observe that the combination labeled 1121 (ASR) performs the best. Adding ICA appears to degrade performance, possibly because the dataset was recorded while the subject was sitting, making ICA an overkill that might remove important components. Interestingly, Autoreject achieves the highest performance in terms of accuracy and micro F1, suggesting that combining it with other methods might yield even better results.

Regarding the performance of each label, labels 1, 2, 3, and 12 (human body, clothing and accessories, food, and plant) generally perform better, except in the 1151 combination, which shows lower performance for the plant label. These categories are commonly seen objects, which likely contributes to their higher performance. Surprisingly, the animal label (0) consistently shows low performance across all artifact removal methods. This is unexpected, as one would assume that animals, being more distinct to human perception, would yield better performance.

# Reference
[1] Gifford, A. T., Dwivedi, K., Roig, G., & Cichy, R. M. (2022). A large and rich EEG dataset for modeling human visual object recognition. NeuroImage, 264, 119754.

[2] Lawhern, V. J., Solon, A. J., Waytowich, N. R., Gordon, S. M., Hung, C. P., & Lance, B. J. (2018). EEGNet: a compact convolutional neural network for EEG-based brain–computer interfaces. Journal of neural engineering, 15(5), 056013.

Code structure modified from [https://github.com/eric12345566](https://github.com/eric12345566)
