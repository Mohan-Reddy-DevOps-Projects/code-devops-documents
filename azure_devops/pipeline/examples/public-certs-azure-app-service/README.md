# Purpose
Find steps on how to add Public Certificates to Azure App Service.  

In App Service, your application runs on HTTP (non-TLS) mode. By default, Azure has default trust certificates like Digicert. So if your app wants to talk to another HTTPS service which has Digicert certificate, it will work without any issue. 

But, if your application wants to communicate with another service that is running on HTTPS and is using custom certificate provided by PremierInc CA or custom CA, then you need to add the PremierInc CA certificate or custom CA certificate to Azure app service. 

>Note: This is useful for non-Container based deployments. For Container based, add the truststore as part of Docker Image. 

# Steps to add Public Certificates to App Service
- Upload the Public Certificate to Azure Library -> Secure files.       
    **Note:** Provide name without spaces. For example: member_ldap_cert

- Copy the `update-ca.yaml` template.  This job runs only on windows agents.

- Add the template job in your deploy.yaml file.

- If you want to upload multiple files, then call the template files multiple times in your deploy.yaml file like below example.

```YAML
- template: update-ca.yaml
  parameters: 
    job_name: update_rootca
    ado_environment: "${{ parameters.ado_environment }}"
    cert_secure_file: 'premierRootCA'  # ado secure file name 
    variable_group: "${{ parameters.deploy_variable_group }}"
    app_service_slot: $(APP_SERVICE_SLOT)
    az_subscription_sc: ${{ parameters.az_subscription_sc }} 

- template: update-ca.yaml
  parameters: 
    job_name: update_root_ca1
    ado_environment: "${{ parameters.ado_environment }}"
    cert_secure_file: 'premierRootCA1' # ado secure file name
    variable_group: "${{ parameters.deploy_variable_group }}"
    app_service_slot: $(APP_SERVICE_SLOT)
    az_subscription_sc: ${{ parameters.az_subscription_sc }} 
```
