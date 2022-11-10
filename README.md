# End-to-End MLOps with Azure ML

This repo is to maintain the source code for tutorial where we want to deploy the ML model from Jupyter notebook to Azure ML workspace to mimic the production env.

## Getting started

1. Activate the Azure Portal subscription
1. Create Azure ML Workspace
1. Create a compute instance in Azure ML Workspace
1. Setup Azure CLI and verify using `az version` command
1. Activate Azure ML extension in the CLI using `az extension add -n ml -y`

## Create Azure ML Data asset
1. Register the data folder into Azure ML Data Assets
   `cd src && az ml data create --file dataset.yml -g {resource-group-name} -w {workspace-name}`

## Create Azure ML job
1. Create Azure ML job using `az ml job create --file job.yml -g {resource-group-name} -w {workspace-name}`
