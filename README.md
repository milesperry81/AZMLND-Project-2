
# Project: Operationalizing Machine Learning

This project is part of the Udacity Azure ML Nanodegree. The aim of this project is to configure a cloud-based machine learning production model using Azure AutoML, deploy it, and consume it via an HTTP endpoint. Once this was done a machine learning pipeline was created using Azure AutoML, published and then consumed, again via an HTTP endpoint.

The [Bank Marketing dataset](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) was used. The dataset contains data about direct marketing campaigns of a banking institution. We seek to predict if a banking client will subscribe to a term deposit (variable y).

## Architectural Diagram

An architectural overview of the key steps in the project can be seen below.

![title](images/project_architecture.png)

## Key Steps
*TODO*: Write a short discription of the key steps. Remeber to include all the screenshots required to demonstrate key steps. 

### Step 1 - Authentication

In this step I create a Service Principal user role with controlled permissions to access specific resources. Using a service principal reduces the scope of permissions available to the user and enhances security.

Firstly, I installed Azure CLI on my local machine so that I could interact with Azure. I logged in and then installed the Azure ML extension.

$ az extension add -n azure-cli-ml

Then I created a Service Principal account ("ml-auth") as shown in the following screenshot.

![title](images/1.1_SP_created.png)

The Service Principal was then granted "owner" access to my ML workspace.

![title](images/1.2_WS_shared_with_SP.png)

### Step 2 - Automated ML Experiment

In this step I create an experiment using AutoML, configured a compute cluster and then used that cluster to run the experiment.

The Bank Marketing dataset was registered in the workspace.

![title](images/2.1_create_dataset.png)

The AutoML experiment was set up to classify if a banking client will subscribe to a term deposit. The experiment completed and the best model was a Voting Ensemble with 91.866% accuracy.

![title](images/2.2_completed_experiment.png)

The best model can be seen here at the top of the list of models.

![title](images/2.3_best_model.png)

The top 4 features that influence the model can be seen below as per the model explanation feature.

![title](images/2.4_best_model_explained.png)

### Step 3 - Deploy the Best Model

The model was then deployed from the Azure Portal using the Azure Container Instance.

![title](images/3.1_best_model_deployed.png)

### Step 4 - Enable Logging

In this step I enabled Application Insights and then retrieved the logs. Firstly, I created a Python virtual environment, activated it and then installed the Azure Python SDK.

$ pip install azureml-sdk

Within the virtual environment I then executed the logs.py file to enable Application Insights and retrieve the logs. I added the following line of code to the logs.py file.

$ service.update(enable_app_insights=True)

Below you can see Application Insight is enabled in the Azure Portal.

![title](images/4.1_application_insights_enabled.png)

The resulting log output can be seen below.

![title](images/4.2_logs.png)

![title](images/4.3_logs.png)

### Step 5 - Swagger Documentation

In this step an instance of Swagger was ran to display the API documentation for the best model.

The swagger.json file was downloaded from the Azure Portal for the best model endpoint. The script swagger.sh was ran and downloaded the latest swagger container to be run on port 9000. Serve.py was ran to enable a Python webserv on port 8000.

the Swagger service was access via a browser on port 9000. The PAI documentation was examined by copying http://localhost:8000/swagger.json into the Explorer field towards the top of the browser screen.

The API details from Swagger can be seen below.

![title](images/5.1_swagger.png)

![title](images/5.2_swagger.png)

![title](images/5.3_swagger.png)

![title](images/5.4_swagger.png)

![title](images/5.5_swagger.png)

### Step 6 - Consume Model Endpoint

![title](images/6.1_consume_api.png)

### Step 7 - Create, Publish and Consume Pipeline

![title](images/7.1_pipeline_created.png)

![title](images/7.2_pipline_endpoint.png)

![title](images/7.3_dataset_and_automl.png)

![title](images/7.4_pipeline_overview.png)

![title](images/7.5_run_details.png)

![title](images/7.6_scheduled_pipeline.png)

![title](images/7.7_scheduled_pipeline.png)

### Step 8 - Documentation


## Screen Recording
*TODO* Provide a link to a screen recording of the project in action. Remember that the screencast should demonstrate:


