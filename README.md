# End-to-End MLOps with Azure ML

This repo is to maintain the source code for tutorial where we want to deploy the ML model from Jupyter notebook to Azure ML workspace to mimic the production env.

## Getting started

1. Activate the Azure Portal subscription
1. Create Azure ML Workspace
1. Create a compute instance in Azure ML Workspace
1. Setup Azure CLI and verify using `az version` command
1. Activate Azure ML extension in the CLI using `az extension add -n ml -y`

## Create Azure ML Data asset
1. Register the experimentation data folder into Azure ML Data Assets for dev environment
   `cd src && az ml data create --file dataset_dev.yml -g {resource-group-name} -w {workspace-name}`
1. Register the production data folder into Azure ML Data Assets for prod environment
   `cd src && az ml data create --file dataset_prod.yml -g {resource-group-name} -w {workspace-name}`

## Create Azure ML job
1. Create Azure ML job for Dev env using `az ml job create --file job_dev.yml -g {resource-group-name} -w {workspace-name}`
1. Create Azure ML job for Prod env using `az ml job create --file job_prod.yml -g {resource-group-name} -w {workspace-name}`

## Create Service Principal for Azure

1. Create a service principal. `az ad sp create-for-rbac --name "mlops-learn-app" --role contributor --scopes /subscriptions/{subscription-id}/resourceGroups/{resource-group} --sdk-auth`
1. Save the output JSON into GitHub Action Secrets as `AZURE_CREDENTIALS`. 

## Test model endpoint

Create a POST request to the following endpoint:
- Method: POST
- URL: https://diabetes-prod-ep.centralindia.inference.ml.azure.com/score
- Headers:
  - Content-Type: application/json
  - authorization: Bearer {api_key}
  - azureml-model-deployment: prod-model-deployment
- Body: 
  ```json
  {
      "input_data": [
              [9,104,51,7,24,27.36983156,1.350472047,43],
              [6,73,61,35,24,18.74367404,1.074147566,75],
              [4,115,50,29,243,34.69215364,0.741159926,59],
              [0,171,80,34,23,43.50972593,1.213191354,21]
          ]
  }
  ```
You should get the following response:
```json
[
    1,
    1,
    1,
    0
]
```