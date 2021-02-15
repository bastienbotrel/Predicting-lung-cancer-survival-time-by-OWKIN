# Predicting lung cancer survival time by OWKIN

Challenge provided by OWKIN's company and organized by College de France and Ecole Normale Supérieure of Paris.

Final leaderbord published on December 15th, 2020.

**Ranking: 2/98** 

## About me
I worked as a biostatistician during 7 years in contract reasearch organization and biotechnologie, in France, United Kingdom and Belgium.
I am seeking to move into data science and especially in artificial intelligence.

I participated to this challenge to have a first project in data science. Furthermore, it was a way to combine my knowledge in biostatistics and my new skills in machine learning and deep learning.

## Project Overview

### Objective

The aim of the challenge was to predict the survival time of a patient (remaining days to live) from one three-dimensional CT scan (grayscale image) and a set of pre-extracted quantitative imaging features, as well as clinical data.

### Context and Resources

 3 Types of data were available for this challenge:
 * Clinical data (6 variables)
 * Radiomics features (53 quantitatives features per patient, extracted from the scan)
 * 3D images (one scan and one mask per patient)

Detailed information on the context of the challenge and data can be found [here](https://challengedata.ens.fr/participants/challenges/33/) on the website of the challenges (https://challengedata.ens.fr/).

### Sample size

300 patients were included in the training set. 
The test set was splitted into two subsets: 63 patients in a public test set used to evaluate the model and 62 patients in a private test set used for the final ranking.

## Data Exploration and Cleaning

## Model Building

First of all, I performed a survival analysis using a **proportional hazard model** (Cox model), based on clinical data and radiomics features. I used this model as a *baseline model* to the deep learning models I tried afterwards. This model contains 6 variables (3 clinical and 3 radiomics features) significant at 5% threshold.

Then, I used the 3D CT scans in a **Survival Convolutional Neural Network** (SCNN) trained with a loss function specific to survival analysis (Cox PH loss function [1]). The SCNN output a risk score for each patient, which is then integrated in a Cox model on top of the clinical data previously retained in the reference model.

I tried two others CNN trained with the same loss function:
* **Survival Convolutional Neural Network** from **2D CT scans** in order to increase the sample size of the train data (3D CT scans have been cut at the center of the tumor in each axis)
* **Clinical Survival Convolutional Neural Network** combining the 3D CT scans with the clinical/radiomics features

## Results

The metric used to evaluate the model was the concordance index (C-index), a score ranging from 0 to 1.

The best model is the Cox model integrating the risk score predicted from the **SCNN using the 3D CT scans**, with a C-index of 0.77.

## Reference 

[1] Cox PH loss function computation - https://github.com/sebp/survival-cnn-estimator
