---
title: "ML Model Deployment End to End Part 1: Establishing Folder Structure"
date: 2020-07-22
tags: [Data Science, Shell Script, Linux]
excerpt: "A quick and easy way to establish folder structure for a ML training and deployment project"
---

<H2>Project Overview</H2>
This is the first article in a series which will explore how to build a simple model, deploy it using FastAPI, log model performance, and use the model logs to test and build model updates.  For the project the model will use the "Hello World" of data science projects, the Titanic Dataset from Kaggle.  This project will be divided into several parts:

<ol>
<li>The first part will discuss how to establish the project folder.</li>
<li>The second part will focus on how to use the project folder to build the initial baseline model.</li>
<li>The third article will examine how to deploy the baseline model using FastAPI.</li>
<li>The fourth part will discuss the importance of logging model predictions and user interactions.</li>
<li>The last article will use the logging abilities to test and improve the baseline model using a rendezvous architecture.</li>
</ol>
    
<H2>Establishing a Project Folder</H2>
It took some work and but I found a project structure that could be used as a baseline for many projects.  By using this project structure I can build, train, and deploy a model from a single repository.

```
project
└───data
└───notebooks
│   │   01_eda.ipynb
│   │   02_data_imputation_model_build.ipynb
│   │   03_data_preprocessing_final.ipynb
│   │   04_model_training.ipynb
│   │   workbench.ipynb
└───references
└───src
│   └───app
│   |   └───assets
|   |   |   └───models
|   |   |   |   └───current_models
|   |   |   |   └───data_preprocessing_models
|   |   |   |   └───deprecated_models
|   |   |   |   └───test_models
│   |   └───modules
|   |   |   __init__.py
|   |   |   data_pre.py
|   |   |   models.py
│   |   |   .env
│   |   |   main.py
│   │   Dockerfile
│   │   fastapi-docker-compose.yml
│   │   requirements.txt
│   │   setup.py
|   .gitignore
|   Dockerfile
|   notebook-docker-compose.yml
|   README.md
|   requirements.txt
```

<H3>data</H3>
The data folder is where I store my training, test, and validation data.  This folder works well for small datasets as the maximum file size for GitHub is 100MB.  In cases where the file size is over 100MB this folder can be used to store sample data for debugging purposes.

<H3>notebooks</H3>
In this folder I have notebooks for exploratory data analysis (EDA), data imputation, data preprocessing, model training, and a workbench notebook.  These notebooks are used to develop the modules used in data preprocessing and data imputation.  These modules are stored in the modules folder in the src directory.  By storing the modules in a central location, both the notebooks and the deployed model are using the same modules.  The workbench notebook is used for testing and development before incorporation into the other notebooks.

<H3>references</H3>
This folder is primarily used to place references for the project.  For example if the ML model is using an algorithm from a research paper, the research paper can be placed in here for easy reference.

<H3>Dockerfile, notebook-docker-compose.yml, and requirements</H3>
I combined these three files as they are all related.  The Dockerfile is empty, but could be modified if required.  The same stands for the requirements file.  For this project to load the model development environment I used a docker-compose file.  The docker image referenced in the file is the Jupyter scipy image.  This image contains the scikit-learn library, which should be enough to preprocess data and train a basic model.  If additional libraries are required the Dockerfile and requirements would be used to customized the image.  The notebook-docker-compose.yml would need to be refactored.  The benefit of using this method to anyone on the development team can use the dockerfile, notebook-docker-compose.yml, and requirements file to work on this project.  This helps with the reproducibility of the project.

<H3>src</H3>
This folder contains the code to deploy the model as a api endpoint using FastAPI.  Once the new model is pushed to the GitLab code repository, GitLab will containerize the api code in this folder.  By separating the model development code and the deployment code into seperate folders but using the same repository we can use common modules as well ensure only the current models are being placed into production.  This practice takes a bit of work to set up, but it pays for itself when it is time to update a module or model.

<H4>src/Dockerfile, fastapi-docker-compose, requirements, and setup.py</H4>
The Dockerfile uses the fastapi image [tiangolo/uvicorn-gunicorn-fastapi
](https://hub.docker.com/r/tiangolo/uvicorn-gunicorn-fastapi) and will install any python requirements in the requirements.txt.  The setup.py will contain the same requirements information.  The container is built using GitLab and [kaniko](https://cloud.google.com/blog/products/gcp/introducing-kaniko-build-container-images-in-kubernetes-and-google-container-builder-even-without-root-access), and I have run into the problem installing requirements via the requirements.txt and so I am using setup.py until I can track down this issue.  The fastapi-docker-compose is used to load a local version of the api for development purposes.  

<h4>app</H4>
This folder contains the assets and modules folder as well as the main.py and .env.  The .env will be used to store tokens for local development and should be added to the .gitignore for security.  The main.py is the python script to start the FastAPI services.  

<h4>app/modules</H4>
To ensure any functions created for the use in the development notebooks are the same used in the actual api this project uses this modules folder as a singular location to store those modules.

<h4>app/assets</H4>
The purpose of this folder is to store any additional files or data required for the api.  The only folder in the intial scheme that is built is the models folder.  However; if other files were required they would be stored in this location.

<h4>app/assets/models</H4>
To me this is the most important folder as it holds all the models that will be deployed via the API.  The current_models folder would contain the models being served by the API.  The data_preprocessing_models is used to store any models for data imputation for missing features.  While this is primarly needed for model training and development, these models might be required as part of the API, and as such as stored here.  If a model is deprecated it is placed in the deprecated folder and models that are being tested are placed in test_models.  This will be important when I go to implement the logging and of deprecated and test models for a rendezvous architecture.

<H2>Shell Script</H2>
It is important to have an organized folder structure.  It is even more important for a team or organization to use a common format.  This ensures the project is reproducible across an organization.  A simple way to do this is through the use of a shell script to build the folders, initial files, and even fill in some basic information into these files, for example fill in the basic information in a .gitignore file.

For me I created a small repository on GitHub for my shell script [ds_api_shell](https://github.com/tvanpat/ds_api_shell).  To start my project these are the steps I would use:

<ol>
<li>Go to desired location and git init a new repository</li>
<li>wget https://github.com/tvanpat/ds_api_shell/ds_shell.sh</li>
<li>Read the script to ensure it does not contain dangerous information</li>
<li>sudo bash ds_shell.sh OR chmod +x ds_shell.sh && sudo ./ds_shell.sh</li>
</ol>

This was written for use on a linux system; however, if you have [WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10) and are using [Windows Terminal](https://docs.microsoft.com/en-us/windows/terminal/) you can execute the shell script in Windows 10.

<H2>Conclusion</H2>
So I am not saying everyone needs to use this same style and I am sure it has it faults I have not seen.  This folder structure and the shell script is some simple steps I am trying to use to reduce the tech debt in my personal projects with the goal of transferring those same techniques to my professional projects.