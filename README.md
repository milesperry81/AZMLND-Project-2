*NOTE:* This file is a template that you can use to create the README for your project. The *TODO* comments below will highlight the information you should be sure to include.


# Project: Operationalising Machine Learning

*TODO:* Write an overview to your project.

This project is part of the Udacity Azure ML Nanodegree. The aim of this project is to configure a cloud-based machine learning production model using Azure AutoML, deploy it, and consume it via an HTTP endpoint. Once this was done a machine learning pipeline was created using Azure AutoML, published and then consumed, again via an HTTP endpoint.

The [Bank Marketing dataset](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) was used. The dataset contains data about direct marketing campaigns of a banking institution. We seek to predict if a banking client will subscribe to a term deposit (variable y).

## Architectural Diagram
*TODO*: Provide an architectual diagram of the project and give an introduction of each step. An architectural diagram is an image that helps visualize the flow of operations from start to finish. In this case, it has to be related to the completed project, with its various stages that are critical to the overall flow. For example, one stage for managing models could be "using Automated ML to determine the best model". 

An architectural overview of the key steps in the project can be seen below.

![title](images/project_architecture.png)

## Key Steps
*TODO*: Write a short discription of the key steps. Remeber to include all the screenshots required to demonstrate key steps. 

### Step 1 - Authentication
![title](images/1.1_SP_created.png)

![title](images/1.2_WS_shared_with_SP.png)

### Step 2 - Automated ML Experiment

![title](images/2.1_create_dataset.png)

![title](images/2.2_completed_experiment.png)

![title](images/2.3_best_model.png)

### Step 3 - Deploy the Best Model

![title](images/3.1_best_model_deployed.png)

### Step 4 - Enable Logging

![title](images/4.1_application_insights_enabled.png)
![title](images/4.2_logs.png)
![title](images/4.3_logs.png)

### Step 5 - Swagger Documentation

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


