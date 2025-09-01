# transplant_deeplearning_ecganalysis_ILP2025
## **Using Deep Learning Models to Predict the Risk of Acute Cellular Rejection in Heart Transplant Recipients with Analysis of Electrocardiogram Images**

## Project Description: 
This project aims to develop and evaluate a non-invasive tool using deep learning applied to 12-lead electrocardiogram (ECG) images to predict ACR. We conducted a retrospective, single-centre study utilised ECG images from heart transplant recipients undergoing surveillance for acute cellular rejection (ACR). An ensemble of four modified EfficientNet-B3 (ENB3) models was developed using transfer learning. Three pre-training strategies were compared: a baseline model pre-trained on ImageNet, and two models further pre-trained on ECG data for either atrial fibrillation or myocardial infarction detection. Model performance was evaluated on two binary classification tasks: moderate-to-severe rejection (2/3R) versus (0/1R), and a secondary task of any rejection (1/2/3R) versus no rejection (0R).

##  Study methods: 
### 1. _Study design and data source_ 
We conducted a retrospective study to evaluate the abilities of different DL models in identifying ACR using 12-lead ECG images on a cohort of 40 patients who underwent a OHT between February 1, 2018, and March 10, 2020, at St Vincent’s Hospital, Sydney. The current study was approved by the St. Vincent’s Hospital Human Research Ethics Committee, Sydney (HREC/17/SVH/80). 

### 2. _ECG images and rejection grade collection_ 
ECG acquired within the first six months of each recipient’s OHT were reviewed. ECGs were de-identified by cropping any patient information from the images. All endomyocardial biopsies and Cardiac Magnetic Resonance Imaging scans were performed either as part of routine rejection surveillance protocol or under the request of treating clinicians. The Python scripts written in Visual Studio Code: "_ECG Auto Cropping_", "_Transplant Dataset Preparation_" were used to automatically crop out the rhythm strip on the bottom of the 12-lead ECG image and resize the image to fit the input layer of ENB3 models. 

### 3. _Model development_
#### Training methods: 
Model performance in classifying rejection-positive and rejection-negative ECGs was evaluated using three distinct data-splitting methods. First, the dataset was divided into training (60%), validation (20%), and held-out test (20%). Second, we adapted the splitting ratio to 60% for training and 40% for validation, with the final performances reflected from the validation set. The script "_Transplant Rejection Network Training_" was used for both splits. Third, a 5-fold cross-validation approach was used. The dataset was divided into five equal stes (20% each), the model was trained and validated iteratively five times. In each iteration, a unique combination of 3x20% was used for training while the remaining 40% for validation. The model's final performance was determined by averaging the results from across all five validation iterations. This approach provides a more robust estimate of the model's generalisability by assessing its performance across multiple data subsets, thereby reducing potential bias from any single data split. This is done by the script "_5-Folds Cross Validation Transplant Rejection Training_".

#### Ensemble learning and transfer learning 
Ensemble learning: The ensemble model consists of four different versions of pre-trained ENB3: one complete ENB3 and three truncated ENB3, each cut at different levels within the feature extraction layers. Each variant is followed by classification layers. Each model is trained independently on the training and validation datasets. To assess the performance of the complete ensemble, a Soft Voting technique is used to average four probability scores into one output. 

Transfer learning: Three publicly available datasets were used in model pretraining: ImageNet, PTB-XL ECG AF classification dataset, and PTB-XL ECG MI classification dataset. Transfer learning Model 1 was only pre-trained on ImageNet. Transfer learning Model 2 and Model 3 had both trained on ImageNet and PTB-XL ECG dataset. Model 2 was previously used to differentiate between AF and non-AF ECGs, whereas Model 3 was previously used to classify AMI, IMI, and the healthy control group. When these models were trained on ECG images from OHT cohort, they carried weights from their previous training.

Both strategies were adapted in the two network training scripts. 

### 4. _Model evaluation_
Model performances were evaluated for accuracy, precision, recall, specificity, the F1-score (the harmonic mean of precision and recall), and Area under the Reciever Operation Characteristic Curve. 


