
# Project: Operationalizing Machine Learning

This project is part of the Udacity Azure ML Nanodegree. The aim of this project is to configure a cloud-based machine learning production model using Azure AutoML, deploy it, and consume it via an HTTP endpoint. Once this was done a machine learning pipeline was created using Azure AutoML, published and then consumed, again via an HTTP endpoint.

The [Bank Marketing dataset](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) was used. The dataset contains data about direct marketing campaigns of a banking institution. We seek to predict if a banking client will subscribe to a term deposit (variable y).

## Architectural Diagram

An architectural overview of the key steps in the project can be seen below.

![title](images/project_architecture.png)

## Key Steps

### Step 1 - Authentication

In this step I create a Service Principal user role with controlled permissions to access specific resources. Using a service principal reduces the scope of permissions available to the user and enhances security.

Firstly, I installed Azure CLI on my local machine so that I could interact with Azure. I logged in and then installed the Azure ML extension.

	$ az extension add -n azure-cli-ml

Then I created a Service Principal (SP) account ("ml-auth") using the following command.

	$ az ad sp create-for-rbac --sdk-auth --name ml-auth

The command completed successfully and returned a dictionary of properties for the new SP as in the following screenshot.

![title](images/1.1_SP_created.png)

The object ID of the Service Principal was then returned by passing the client ID to the following command.

	$ az ad sp show --id 8b833232-91d7-4086-ac79-7ee42fef68cf

The Service Principal was then granted "owner" access to the ML workspace by running this command.

	$ az ml workspace share -w udacity-ws -g udacity-ml --user 63936b7f-133f-45ee-a72a-d0cb1b0554a3 --role owner

In the below screenshot you can see that the command execute with no issues.

![title](images/1.2_WS_shared_with_SP.png)

### Step 2 - Automated ML Experiment

In this step I create an experiment using AutoML, configured a compute cluster and then used that cluster to run the experiment.

The Bank Marketing dataset was registered in the workspace by registering a dataset in the workspace. Below you can see the registered dataset in my workspace.

![title](images/2.1_create_dataset.png)

I then went to the Automated ML section and created a new AutoML experiment using the regsitered Bank Marketing dataset. A compute clustered was configured with Standard_DS12_v2 virtual machine and 1 as the minimum number of nodes. The experiment was of a Classification type, with Explain Best Model enabled. Exit criteria was set to 1 hour and Concurrency set to 5. The AutoML experiment was then ran.

The AutoML experiment was set up to classify if a banking client will subscribe to a term deposit. The experiment completed and the best model was a Voting Ensemble with 91.866% accuracy. Below you can see the details of the completed experiment.

![title](images/2.2_completed_experiment.png)

The best model can be seen here at the top of the list of models.

![title](images/2.3_best_model.png)

The top 4 features that influence the model can be seen below as per the model explanation feature.

![title](images/2.4_best_model_explained.png)

### Step 3 - Deploy the Best Model

The best model was then deployed from the Azure Portal using the Azure Container Instance. Below you can see the best model has a Deploy Status of "succeeded".

![title](images/3.1_best_model_deployed.png)

### Step 4 - Enable Logging

In this step I enabled Application Insights and then retrieved the logs. 

Firstly, I created a Python virtual environment in my current working directory.

	$ python -m venv .

I then activated the venv.

	$ .\Scripts\activate

Once the venv was active I then installed the Azure Python SDK.

	$ pip install azureml-sdk

Within the virtual environment I then executed the logs.py file to enable Application Insights and retrieve the logs. I added the following line of code to the logs.py file.

	$ service.update(enable_app_insights=True)

Below you can see Application Insight is enabled in the Azure Portal as a result of me running the logs.py file.

![title](images/4.1_application_insights_enabled.png)

The resulting log output returned to the command line interface after running logs.py can be seen below. This demostartes teh ability to access logs from a remote machine using the Azure CLI.

![title](images/4.2_logs.png)

![title](images/4.3_logs.png)

### Step 5 - Swagger Documentation

In this step an instance of Swagger was deployed to display the API documentation for the best model.

The swagger.json file was downloaded from the Azure Portal for the best model endpoint. The script "swagger.sh" was ran and downloaded the latest swagger container to be run on port 9000. "Serve.py" was ran to enable a Python webserver on port 8000.

The Swagger service was accessed via a browser on port 9000. The API documentation was examined by copying http://localhost:8000/swagger.json into the Explorer field towards the top of the browser screen. 

The API details from Swagger can be seen below. It outlines the GET and POST methods for the API, any parameters and expected responses types. It is used by developers when developing a program to make a call to an endpoint.

![title](images/5.1_swagger.png)

![title](images/5.2_swagger.png)

![title](images/5.3_swagger.png)

![title](images/5.4_swagger.png)

![title](images/5.5_swagger.png)

### Step 6 - Consume Model Endpoint

In this step the "endpoint.py" script was executed so as to interact with the best model endpoint. The "scoring_uri" and "key" parameters were updated using the endpoint information found in the Azure Portal. Two sets of features were passed to the best model endpoint using the script. The resulting output can be seen below.

![title](images/6.1_consume_api.png)

The result returned was ["yes","no"]. Based on the JSON data uploaded in the "endpoints.py" the first banking client is predicted to subscribe to a term deposit, the second client is not.

### Step 7 - Create, Publish and Consume Pipeline

In this step the provided Jupyter Notebook was uploaded to the workspace. The notebook uses the Azure Python SDK to interact with the workspace. Relevant parameters in the notebook were updated. The cells in the notebook were ran. The notebook is here  https://github.com/milesperry81/AZMLND-Project-2/blob/main/aml-pipelines-with-automated-machine-learning-step.ipynb. The notebook can be broadly split into 3 sections:

#### 1 Create and Submit a Pipeline

1. Identify the existing experiment from step 2.
2. Identify a compute instance.
3. Load the registered dataset.
4. Create a pipleline and an AutoML step.
5. Run the pipeline.

#### 2 Examine Model Results

1. Retrieve metrics from child runs.
2. Retrieve best model.
3. Test model.

#### 3 Publish Pipeline and run REST endpoint 

1. Publish pipeline.
2. Retrieve authentication header.
3. Trigger pipeline to run using the REST endpoint.

The pipeline run created from the notebook using the Azure Python SDK can be seen below screenshot.

![title](images/7.1_pipeline_created.png)

The pipeline was then published as an endpoint. Below you can see the endpoint displayed in the screenshot.

![title](images/7.2_pipline_endpoint.png)

The dataset and AutoML module can been seen within the pipeline overview in the belwo screenshot. Clearly shoiwng the Bank Marketing dataset was ingested into an AutoML module.

![title](images/7.3_dataset_and_automl.png)

The published pipeline overview can bee seen in the below screenshot.

![title](images/7.4_pipeline_overview.png)

The RunDetails widget from the notebook can bee seen below. This shows that the pipline was executed from the notebook.

![title](images/7.5_run_details.png)

The pipeline triggered by the REST endpoint can be seen in the below list in the Azure Portal.

![title](images/7.6_scheduled_pipeline.png)

The pipeline overview for the triggered pipeline is below.

![title](images/7.7_scheduled_pipeline.png)

### Step 8 - Documentation

For this project the key documentation was this README.md file and a screen recording (detailed below).

## Screen Recording

The project screencast can be found here: https://youtu.be/WNkzuYKP-vI

## Improvements

* Automate the testing and deployment of the best model found by the AutoML pipeline.

* Notebook seems to be using the training data to test the model. This is not a fair test as the model has already seen the training data. It should use an unseen set of test data.



