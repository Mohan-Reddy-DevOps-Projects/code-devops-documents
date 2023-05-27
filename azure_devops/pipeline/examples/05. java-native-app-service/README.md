# Purpose
Follow this document if you use maven to build java project and deploy to Azure App service without using Docker.

# Pre-requisite
- Make sure you have access to subscription in Azure Cloud. 
- Make sure you can view the app service created by Cloud DevOps Team in Azure Cloud as we have to pull some values from the app service.
- Follow steps [here](../../troubleshooting.md#azure-arm-service-connection) to create ARM Service Connection.

- Retrieve the App service name, App Service Resource Group from Azure Cloud.

- A Library group is needed in Azure DevOps for storing these values for deploy environments. For example: java-app-service-dev must have following key/values. All these variables are used in `pipelines/deploy.yaml` file. 

    | Key | Value | 
    | --- | --- |
    | var_az_app_service_rg | ResourceGroupName (Mandatory) |
    | var_az_app_service | AppServiceName (Mandatory) | 
    | var_spring_profiles_active | Optional_Used_in_AppSettings | 
    | var_appinsights | AppInsightsName | 
    | var_az_keyvault | Az Key vault if you need | 
    | var_az_db_password | IamBatman | 

# Points to consider

- Make sure your application is running without TLS/SSL.
- If your app needs secrets like "DB_Password", then consider using Azure Key Vault. 

# Pipeline explanation
- **pipelines/azure-pipeline.yaml** is the root pipeline file.  Modify the maven goals & options as per your application.
- **pipelines/ci.yaml** contains the continuous integration part of the pipeline. It includes three jobs:
  - Build job that container Maven step to build the JAR file and create an artifact of it.
    - Modify the JAR file name in the artifact section based on your application JAR file name.
  - Nexus Lifecycle Scan
  - Checkmarx Scan
- **pipelines/deploy.yaml** is a template file that contains the Azure App service deployment steps. It will be re-used by Dev/QA/UAT/Prod stage. 
  - To define your application port, consider setting `PORT` in app settings. Check the `pipelines/deploy.yaml` file. 
  - To control the Heap Memory in Java application, you have to set the `JAVA_OPTS` in the app settings. Check the `pipelines/deploy.yaml` file. 
  - If your application needs environment variable, it needs to be passed as app settings. Check the `pipelines/deploy.yaml` file.
  - Also covered steps on how to use key vault in `pipelines/deploy.yaml` file. You can remove it if you think that's not required for your app. 
  - Application Health Check can also be configured. 