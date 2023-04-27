# Purpose
Follow this document if you use maven to build java project and deploy to Azure App service without using Docker.

# Pre-requisite
- Make sure you have access to subscription in Azure Cloud. 
- Make sure you can view the app service created by Cloud DevOps Team in Azure Cloud as we have to pull some values from the app service.
- A Service Connection of type "Azure Resource Manager" for your subscription is needed in the Azure DevOps. For example, if your subscription name is `SUB_LMS_PROD`, then we need a service connection of type "Azure Resource Manager" in Azure DevOps as `SC_ARM_LMS_PROD`. 
  - Only Cloud security team can do this. Open snow ticket with cloud security team for this [here](https://premierprod.service-now.com/premiernow?id=dept_cat_item&sys_id=c64bdf091bdc2494be08975e034bcbbb)
  - Select "Azure" in Environment.

- Retrieve the App service name, App Service Resource Group from Azure Cloud.

- A Library group is needed in Azure DevOps for storing these values for deploy environments. For example: java-app-service-dev must have following key/values. All these variables are used in `pipelines/deploy.yaml` file. 

    | Key | Value | 
    | --- | --- |
    | var_az_app_service_rg | ResourceGroupName (Mandatory) |
    | var_az_app_service | AppServiceName (Mandatory) | 
    | var_APPINSIGHTS_INSTRUMENTATIONKEY | Get it from AppInsights |
    | var_APPLICATIONINSIGHTS_CONNECTION_STRING | Get it from AppInsights |
    | var_spring_profiles_active | Optional_Used_in_AppSettings | 

# Points to consider

- Make sure your application is running without TLS/SSL.

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