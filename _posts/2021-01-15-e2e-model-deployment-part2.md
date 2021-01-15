---
title: "ML Model Deployment End to End Part 2: Building the Baseline Model"
date: 2020-07-22
tags: [Data Science, Python, Jupyter, scikit-learn, Data Imputation, Machine Learning]
excerpt: "Using the notebook folder for data imputation and model development"
---

<H2>Project Overview</H2>
This second portion of the project will focus on the actual baseline model development.  A majority of the time spent in this section will be on EDA, data imputation, and initial model selection.  The purpose will be to build a "good enough" model which can be quickly deployed.  In the fourth part of the project I will revisit the baseline model and improve upon it, and implement a model logging system moving towards a rendezvous architecture.


<H2>Using the Notebook Folder</H2>
Most of the work for this portion of the project will be completed in the notebook folder.  In this folder there are 6 [notebooks](https://gitlab.com/tvanpat/e2e-titanic/-/tree/master/notebooks):

<ol>
<li>01_eda.ipynb</li>
<li>02_data_imputation_model_build.ipynb</li>
<li>03_data_preprocessing_final.ipynb</li>
<li>04_model_training.ipynb</li>
<li>05_kaggle_validation.ipynb</li>
<li>workbench.ipynb</li>
</ol>

<H3>01_eda.ipynb</H3>
The purpose of the EDA notebook is to have a centralized location to conduct exploratory data analysis.  This notebook should be written in a way where any person could open it and follow the EDA process.

<H3>02_data_imputation_model_build.ipynb</H3>
After EDA is complete it maybe required to impune data into missing features.  These features can be explored in the data_imputation notebook and methods impune can be explored.  If a function is created to impune data it should be saved in the data_pre.py file in the src/app/modules folder.  In the case of this project a ML model was created to impune the value of some missing features.  These models were trained in this notebook with the actual model being saved in the src/app/assets/models/data_preprocessing_models folder.

<H3>03_data_preprocessing_final.ipynb</H3>
Once the modules and modules are built for the data preprocessing the training data is preprocessed in the data_preprocessing notebook.  This notebook is built with the mentality that any person should be able to open this notebook and process the training data.  To accomplish this I used a .pipe to create a data preprocessing pipeline, so the entire process can be accomplished in one click.

<H3>4_model_training.ipynb</H3>
The model training notebook takes in the preprocessed training data and is used to create the baseline model.  By using the Scikit-learn Pipeline module several different model type and feature selections can be tested at once.  Once the baseline model is selected it is saved in the src/app/assets/models/current_modules folder.

<H3>05_kaggle_validation.ipynb</H3>
As this data came from a Kaggle competition this is where I processed the kaggle validation data.  Typically this notebook is not required, but helps to demonstrate this structure is flexible to the needs of a project.

<H3>workbench.ipynb</H3>
THis last notebook is used to prototype code or work on modules without disrupting the work in another notebook.

<H2>Exploratory Data Analysis</H2>

<H2>Data Imputation</H2>

<H2>Data Preprocessing</H2>

<H2>Model Training</H2>

<H2>Kaggle Validation</H2>

<H2>Conclusion</H2>

<ol>
<li>Go to desired location and git init a new repository</li>
<li>wget https://github.com/tvanpat/ds_api_shell/ds_shell.sh</li>
<li>Read the script to ensure it does not contain dangerous information</li>
<li>sudo bash ds_shell.sh OR chmod +x ds_shell.sh && sudo ./ds_shell.sh</li>
</ol>

This was written for use on a linux system; however, if you have [WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10) and are using [Windows Terminal](https://docs.microsoft.com/en-us/windows/terminal/) you can execute the shell script in Windows 10.