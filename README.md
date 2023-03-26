# brain-tumor-classifier
This project was done as a part of DS3010 Machine Learning course. We were able to classify MRI images of brain to 4 classes - no tumor, glioma, meningioma, pituitary - with the best accuracy of around 85-88%.

---

## Problem Statement
Classification of MRI images of the human brain into four classes - no tumor, glioma tumor, meningioma tumor and pituitary tumor and to identify the best pre-trained CNN model for feature extraction, and the best classifier.

## Method
- From a brain MRI image dataset from [Kaggle](https://www.kaggle.com/datasets/masoudnickparvar/brain-tumor-mri-dataset?select=Training), we used 5712 images.
- Four pre-trained CNN models (VGG16, ResNet50, Xception, and EfficientNetB7)
were used for extracting features.
- Variance threshold was applied for feature reduction, PCA was used for feature extraction.
Multiple classifiers such as Decision Tree, Random Forest, SVM, ExtraTreesClassifier, AdaBoost, and Naïve Bayes were separately used for classification and comparison.

## Experimental Procedure
- Features were extracted from the images separately using multiple CNN models from Tensorflow Keras, converted it into DataFrame, and was saved to Google Drive.

CNN Model|Number of features extracted|Number of non-zero variance features
----|----|----
RESNET50|32768|7599
VGG16|25088|2437
XCEPTION|18432|3901
EFFICIENTNETB7|23040|23040

$\text{Number of features extracted by the CNN models used.}$

- PCA and/or variance threshold was applied on data to reduce the number of features. Using variance threshold, zero variance columns were removed. After normalising the data, we also tried removing low variance features.

- `train_test_split()` was used to randomly split the dataset into 75% training data and 25% testing data.
- Different classifiers from sklearn — Decision Tree Classifier, Random Forest Classifier, SVM, ExtraTrees Classifer, AdaBoost Classifier, and Naïve Bayes Classifier were trained using train data. They were then used to classify the testing data, and the classification report is generated.

## Results
- Based on accuracy values observed, RESNET50 was found to outperform the other CNN models for most of the classifiers. SVM outperformed the other classifiers for all CNN models used.

CNN Model|Decision Tree|Random Forest|SVM|Extra Trees|AdaBoost|Naïve Bayes
----|----|----|----|----|----|----
RESNET50|0.7177871|0.8312324|**0.8802521**|0.8368347|0.6960784|0.6050420
VGG16|0.7107843|0.8403361|**0.8501400**|0.8424369|0.7009803|0.4656862
XCEPTION|0.6827731|0.8263305|**0.8452380**|0.8319327|0.6960784|0.5791316
EFFICIENTNETB7|0.6841736|0.7934173|**0.8186274**|0.7864145|0.6806722|0.3844537

$\text{The best observed accuracies for each CNN model, and each classifier.}$

## Statistical Hypothesis Testing
- We performed statistical hypothesis testing on SVM vs Random Forest and SVM vs AdaBoost. We used 5x2 fold CV for hypothesis testing .
- This was performed for 3 CNN models – RESNET50, VGG16 and XCEPTION.

CNN Model|SVM vs Random Forest|SVM vs AdaBoost
---|---|---
RESNET50|p-value = 2.603e-05, `diff`|p-value = 1.554e-05, `diff`
VGG16|p-value = 0.182, `same`|p-value = 2.257e-05, `diff`
XCEPTION|p-value = 0.739, `same`|p-value = 2.962e-05, `diff`

$\text{NOTE: “same”: models probably has similar performance; “diff” : Difference between mean accuracy is probably real. [ α = 0.05 ]}$

## Inferences
- From the above data we can observe that in general a model does not have more than 5-8% change in the accuracy because of the change in the dataset (created by different pretrained models). For example SVM was having accuracy above 80% for every dataset.
- Among all the models that we trained we observed that class 2 (Meningioma) and class 1 (Glioma) were misclassified considerably.
- Among the tumor classes, Meningioma was misclassified as the No Tumor class the most.

## Conclusion
- SVM was found to have the highest accuracy among all classifiers whereas the accuracy observed for Naïve Bayes was significantly low.
- From statistical testing we can see that, in general, the performance of SVM and Random Forest is probably similar. Whereas, the difference in performance of SVM and AdaBoost is probably true.
- From statistical testing on the features extracted by RESNET50, we can conclude that SVM is indeed outperforming AdaBoost and Random Forest.
