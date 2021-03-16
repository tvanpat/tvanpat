---
title: "ML Model Deployment End to End Part 3: Deploying the Baseline Model"
date: 2021-03-12
tags: [Data Science, Python, FastAPI, Flask, GitLab, Rancher, Kubernetes]
excerpt: "Deploying the model and a simple web page"
---

<H2>Project Overview</H2>
This portion of the project is dedicated to deploying the baseline model that was build in the previous post <a href="https://www.descriptdata.com/e2e-model-deployment-part2/" target="_blank" rel="noopener noreferrer">ML Model Deployment End to End Part 2:</a>.  The model built in the previous section was a minimal viable model, and will require monitoring and updates, but first I need to deploy the model.  To deploy the model I am using a simple python API framework called FastAPI.  This simple framework makes deploying any type of Python based API fast and easy.  I also build a simple web application that points to the endpoint as a way of visualizing the model results.  This webpage was built on Flask and I can say with confidence it was much easier to deploy an API through FastAPI then it was to build a webpage through Flask.  Although that probably has a lot to do with my web design abilities than anything.

<H2>API File Structure</H2>
As a quick review and update here is the folder structure for this project.  You can view the project here: <a href="https://gitlab.com/vp_projects/e2e-titanic/-/tree/master/src" target="_blank" rel="noopener noreferrer">API Deployment Folder</a>

```
src
└───app
│   └───assets
|   |   └───models
|   |   |   └───current_models
|   |   |   └───data_preprocessing_models
|   |   |   └───deprecated_models
|   |   |   └───test_models
|   |   config.yml
│   └───modules
|   |   __init__.py
|   |   data_pre.py
|   |   models.py
│   main.py
│   Dockerfile
|   Dockerfile.DEV
│   fastapi-docker-compose.yml
│   setup.py
```
The primary changes to this folder were the removal the requirements.txt and the addition of the Dockerfile.dev.  These changes were made because during the build process I use the Kaniko image to build the API docker image.  Kaniko has a documented issue with using requirements.txt files so an easy way around that issue is to just use a setup.py file, also as this file in nested under the main project a couple paths had to be adjusted in the image.  This is the reason why there are two separate dockerfiles, one is for the docker-compose.yml and one is for the production build.  Some other changes include app/config.yml this was included as a configuration file for api when the api call is missing the embarked or the fare information.  These changes reflect that while the base structure helped to speed up the development process it was not 100%, nor should it be.  The file system needs to be adaptive to the needs of the project.

<H2>The API</H2>
Perhaps the reason I really like FastAPI is how easy it is code an API.  In less than 60 lines of code I have an API that has an included documents page, proper redirects, and even input validation, you can see the code here: <a href="https://gitlab.com/vp_projects/e2e-titanic/-/blob/master/src/app/main.py" target="_blank" rel="noopener noreferrer">main.py</a>.  You just can't beat that.  I wrote two endpoints for this api.  The first takes in a single prediction and returns a result.  The second endpoint takes in an array of features to and returns an array of results.  This second endpoint is very useful when a single request has multiple required predictions.  Instead of tying up CPU threads with multiple requests/responses a single request with an array can be sent which is much faster than piecemeal prediction requests.  The API documentation for this API can be seen here <a href="https://e2eapi.descriptdata.net/docs" target="_blank" rel="noopener noreferrer">E2E Titanic Endpoints</a>.  
<br>
The deployment process for this end point was fairly easy as well.  As I have the code hosted on GitLab, I take advantage of the Auto Devops feature and using the <a href="https://gitlab.com/vp_projects/e2e-titanic/-/blob/master/.gitlab-ci.yml" target="_blank" rel="noopener noreferrer">.gitlab-ci.yml</a>. In this simple two stage process, once a change has been committed to the main branch Gitlab will take the application in the src/app folder and create a docker image and save it in the Gitlab project container registry.  The second part take the image in the container registry and deploys it to my server which is running a single node rancher/kubernetes instance.  The best part of this configuration is this API is using a full CI/CD pipeline and will always be using the most current version of the API with no manual deployments.  

<H2>The Website</H2>
In order to better visualize the results I built a very simple webpage using the flask framework, this page can be viewed here <a href="https://e2eweb.descriptdata.net/" target="_blank" rel="noopener noreferrer">e2eweb</a>.  While I see this type of model working better as an API call for a data ingest process, I wanted a way to visualize the model result as it is easier for an audience to see a web page and interact with it rather than conceptualize an api call in a data pipeline.  This was deployment is the same as the API using GitLab Auto Devops and rancher/kubernetes.  While testing the webpage and API I noticed that according to the model Females almost always are predicted to survive and males are almost always predicted to die.  As the model is only 77% accurate according to Kaggle, so it looks I have some work to do, which I will cover in the next couple of articles.

<H2>Conclusion</H2>
I really enjoyed this portion of the project.  To deploy the model and interact with it through a web interface was great.  I had to make some modifications to ensure the code is able to be deployed in CI/CD framework.  This part of the project really helped me understand the project gaps.  I still need to build logging and visualization to understand the model performance and then bring in new model deployment methods.  So while there was some great work done to this point, this project is far from complete.  Three down two to go.
