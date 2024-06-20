# BCI Final Project
Authors: X1096021 許絜茵 X1096023 潘和新 X1096028 余維廷

## Introduction

### Overview

- In this proposal, we try to develop an EEG-based diagnostic tool for depression could offer several advantages, including objective measurements, reduced reliance on subjective self-reporting, and potentially earlier detection and intervention for individuals at risk of depression.
- Our BCI system uses EEG signals to find out the characteristics of EEG from the dataset.

### Data description
Resting EEG data were collected from 122 college-aged participants, who are the same individuals involved in the Openneuro probe selection task. Each participant has matching task IDs across both datasets, allowing for easy correlation.

The task was programmed using the DMDX language and included instructions for alternating one-minute periods of eyes open and eyes closed, with triggers indicating these periods (e.g., OCCOCO or COOCOC). Data collection took place between 2008 and 2010 in John J.B. Allen's lab at the University of Arizona.

- **Experimental Design/Paradigm:**
The dataset involved 122 college-age participants. Participants were consistently scored as either high or low on the Beck Depression Inventory (BDI), and some underwent clinical interviews (see .xls sheet for details). It should be noted that the horizontal (HEOG) and vertical (VEOG) electrooculogram channels might be mislabeled for some, if not all, participants. Additionally, some files have already had certain channels interpolated, with no raw data available for reversion.
The first resting session lasted 6 minutes and was conducted immediately after the EEG setup, while the second session also lasted 6 minutes and was conducted about an hour after task performance. Participant 516 does not have data for the second resting session, and participant 544 was excluded from all analyses due to inconsistent BDI scores between mass and lab assessments (1-4 months apart). Despite these issues, the data quality of the first resting session is high, although the quality of the second resting session has not been reviewed.

- **Data Source:**

    - The dataset is publicly available from researchers in John J.B. Allen lab at U Arizona.
    - We download it from OpenNeuro. [[link]](https://openneuro.org/datasets/ds003478/versions/1.1.0))
      
- **Quality Evaluation:**
    - Surveying and analyzing existing literature
       - Atefeh S et al.[[4]](#ref4) aimed to review all papers focused on using deep learning to detect or predict depression using EEG signals as input data. After conducting a thorough search, they evaluated 22 articles published between 2016 and 2021. Following the systematic literature review (SLR) method, this article provides comprehensive summaries of the reviewed studies and compares their key aspects. Additionally, we performed statistical analyses to gain a deeper understanding of the prevailing ideas in recent research in this field. We identified a common five-step procedure employed by most of the reviewed articles to achieve depression detection. Finally, we discussed the open issues and challenges in using EEG for depression diagnosis or prediction, along with suggestions for future research directions.







    - Analyzing the hidden independent components within EEG using ICA with ICLabel
    
      ![表格](https://github.com/x1096023/BCI_final_project/assets/69157513/0f0bb33b-7348-436e-80ad-0c34b5261561)



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
[1] Park, S. M. (2021, August 16). EEG machine learning. Retrieved from osf.io/8bsvr

[2] Ksibi A, Zakariah M, Menzli LJ, Saidani O, Almuqren L, Hanafieh RAM. Electroencephalography-Based Depression Detection Using Multiple Machine Learning Techniques. Diagnostics (Basel). 2023 May 17;13(10):1779. doi: 10.3390/diagnostics13101779. PMID: 37238263; PMCID: PMC10217709.

[3] Jaseja H. Electroencephalography in the diagnosis and management of treatment-resistant depression with comorbid epilepsy: a novel strategy. Gen Psychiatr. 2023 Apr 17;36(2):e100868. doi: 10.1136/gpsych-2022-100868. PMID: 37082530; PMCID: PMC10111881.

<a id="ref4"></a>[4] Atefeh Safayari, Hamidreza Bolhasani(2021). Depression diagnosis by deep learning using EEG signals: A systematic review. Medicine in Novel Technology and Devices.[[pdf]](https://www.sciencedirect.com/science/article/pii/S2590093521000461)

