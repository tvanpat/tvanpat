---
title: "ML Model Deployment End to End Part 2: Building the Baseline Model"
date: 2020-07-22
tags: [Data Science, Python, Jupyter, scikit-learn, Data Imputation, Machine Learning]
excerpt: "Using the notebook folder for data imputation and model development"
---

<H2>Project Overview</H2>
In the previous article <a href="https://www.descriptdata.com/e2e-model-deployment-part1/" target="_blank" rel="noopener noreferrer">Establishing Folder Structure</a> the focus was on establishing the folder structure for the project, and how to automate the process using a simple shell script.  In this portion of the project I will discuss how I like to design my notebook section for EDA and model development.  The basic concept behind using the format discussed below is reproducibility.  My goal behind this structure is that anyone can go to the git repository look at the notebook folder and understand the model developer thought process when building the baseline models.  This helps with the sustainability of a project, the model developer can easily pass the project off or even they can go back themselves to understand their own thought process when developing the model.

I build this model using the minimal viable product mentality.  As such I didn't spend a great deal of time conducting EDA.  This is generally a bad practice, but my intent was to deploy a basic model and then come back to show how to improve the model in a systematic manner.  Going back to the original outline for this project, build the folder structure, build the model, deploy the model, implement model metrics, and then test and improve the model.  I also feel this represents the process of a real business need.  Generally there is not enough time for the perfect model in a business environment.  In an academic environment the goal is to find and publish the method to build the perfect model and time is not the most critical resource .  In a business environment, time is often the resource you won't have enough of.  Every day the model is not in use is a day of potentially wasted revenue.  So build and deploy a basic model, then deploy future improvements as features.  


<H2>Using the Notebook Folder</H2>
The work for this portion of the project will be completed in the notebook folder.  In this folder there are 6 <a href="https://gitlab.com/tvanpat/e2e-titanic/-/tree/master/notebooks" target="_blank" rel="noopener noreferrer">notebooks</a>

<ol>
<li>01_eda.ipynb</li>
<li>02_data_imputation_model_build.ipynb</li>
<li>03_data_preprocessing_final.ipynb</li>
<li>04_model_training.ipynb</li>
<li>05_kaggle_validation.ipynb</li>
<li>workbench.ipynb</li>
</ol>

<H3><a href="https://gitlab.com/tvanpat/e2e-titanic/-/blob/master/notebooks/01_eda.ipynb" target="_blank" rel="noopener noreferrer">01_eda.ipynb</a></H3>
The purpose of the EDA notebook is to have a centralized location to conduct exploratory data analysis.  This notebook should be written in a way where any person could open it and follow the EDA process.  

As the purpose of this project is to show how to build an end to end ML solution, I am not going to go very deep into my EDA.  This is mainly because I want to build a baseline model and deploy it.  I will come back in a later article to discuss how to read the EDA and some of the implications the data ratios will have on the model and how to normalize the data.  

<H3><a href="https://gitlab.com/tvanpat/e2e-titanic/-/blob/master/notebooks/02_data_imputation_model_build.ipynb" target="_blank" rel="noopener noreferrer">02_data_imputation_model_build.ipynb</a></H3>
After EDA is complete it maybe required to impune data into missing features.  These features can be explored in the data_imputation notebook and methods impune can be explored.  If a function is created to impune data it should be saved in the data_pre.py file in the src/app/modules folder.  In the case of this project a ML model was created to impune the value of some missing features.  These models were trained in this notebook with the actual model being saved in the src/app/assets/models/data_preprocessing_models folder.

The purpose of this data imputation model notebook is not to impute the data, but rather to build the modules used for data imputation which will be employed in the data preprocessing notebook.  Within the Titanic dataset there were several missing data points in important features such as age.  This notebook builds several methods of imputation which will be used in the model training.  The concept is to use various methods of imputation and see which model and which imputation method provides the best results.  For the age feature two different linear models one deterministic and one stochastic.  This is the notebook where these models were trained, and then saved in the models folder.

<H3><a href="https://gitlab.com/tvanpat/e2e-titanic/-/blob/master/notebooks/03_data_preprocessing_final.ipynb" target="_blank" rel="noopener noreferrer">03_data_preprocessing_final.ipynb</a></H3>
Once the are built for the data preprocessing the training data is preprocessed in the data_preprocessing notebook.  This notebook is built with the mentality that any person should be able to open this notebook and process the training data.  To accomplish this I used a .pipe to create a data preprocessing pipeline, so the entire process can be accomplished in one click.

I really like this structure even for other projects where the need is simple data conversion of a spreadsheet into another spreadsheet.  While it is better to build a simple application where a person can upload a spreadsheet and then download the results, in a case where there may not be resources (time or money), to build and develop such a project building a simple notebook any user can access can be a "good enough" answer.  However, this should be avoided to ensure you are not increasing tech debt of a project.

<H3><a href="https://gitlab.com/tvanpat/e2e-titanic/-/blob/master/notebooks/04_model_training.ipynb" target="_blank" rel="noopener noreferrer">04_model_training.ipynb</a></H3>
The model training notebook takes in the preprocessed training data and is used to create the baseline model.  By using the Scikit-learn Pipeline module several different model type and feature selections can be tested and results printed with a simple button click.  Once the baseline model is selected it is saved in the src/app/assets/models/current_modules folder.

For the baseline model the data was loaded.  This data was split between training and test data sets.  Then using sklearn pipelines and a for loop this several different models were trained against different feature selections.  The models trained and tested were a Naive Bayes Gaussian Classifier, Stochastic Gradient Descent Classifier, K-Nearest Neighbors Classifier, Decision Tree Classifier, Random Forest Classifer, Support Vector Classifier, and a Gradient Boosting Classifier.  The model with the best performance was the Gradient Boosting Classifier.  This model was saved as the baseline model.  In future articles I will discuss how to improve logically and systematically make model adjustments and compare them to the baseline model and even deploy new models into a rendezvous architecture.

<H3><a href="https://gitlab.com/tvanpat/e2e-titanic/-/blob/master/notebooks/05_kaggle_validation.ipynb" target="_blank" rel="noopener noreferrer">05_kaggle_validation.ipynb</a></H3>
As this data came from a Kaggle competition this is where I processed the kaggle validation data.  Typically this notebook is not required, but helps to demonstrate this structure is flexible to the needs of a project.  This notebook could easily be renamed and used to validate a model if there is enough data to create a validation set.  For this project I used the model API created to run the Kaggle validation set and create the csv for entry into the Titanic contest.  

<H3>workbench.ipynb</H3>
This last notebook is used to prototype code or work on modules without disrupting the work in another notebook.

<H2>Kaggle Validation</H2>
My Gradient Boosting Classifier using a determinist linear model for age imputation scored a 84% accuracy against my test set.  The same model scored against the Kaggle Validation set scored at 77.90% accuracy, which placed me in the top 23% of the models.  The results can be viewed <a href="https://www.kaggle.com/vp7799" target="_blank" rel="noopener noreferrer">Kaggle Results</a>.  So the model is pretty good, but definitely needs some work.  This means at the last stage my goal will be to build a model that breaks into the 80% accuracy range of Kaggle.

<H2>Conclusion</H2>
Far too often in model development this is the final step for the data scientist.  I think this is very short sighted.  The data scientist still has a responsibility to ensure the model is deployed accurately, to monitor model performance, and to update the model with better performing models and account for model drift.  I am glad that is started this project as it has continuously made me think and rethink how to develop models that can be easily deployed as well as a model where another person can continue development if the primary person leaves the project.  

So two parts of the project down, three more to go.
