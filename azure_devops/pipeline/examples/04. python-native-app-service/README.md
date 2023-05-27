# Purpose
Follow this document if you use maven to build python project and deploy to Azure App service without using Docker.

# Pre-requisite
- Follow steps [here](../../troubleshooting.md#azure-arm-service-connection) to create ARM Service Connection.

- App service name, App Service Resource Group will be provided by cloud devops team. No need Service connection for this in ADO. 

- Library group is needed for deploy environments. For example: python-app-service-dev must have following key/values.

    | Key | Value | 
    | --- | --- |
    | var_az_app_service_rg | ResourceGroupName |
    | var_az_app_service | AppServiceName | 
    | var_spring_profiles_active | Optional_Used_in_AppSettings | 