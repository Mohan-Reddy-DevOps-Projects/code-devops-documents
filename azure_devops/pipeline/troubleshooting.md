# Troubleshooting

## 400 Bad Request while uploading package to Nexus
  - `Problem`: It looks like the version you are trying to upload is already present in Nexus.
  - `Solution`: Change the version or add `-SNAPSHOT` to the version. You can push `-SNAPSHOT` versions multiple times whereas you can push non-snapshot versions only once to Nexus when it comes to Maven.

## 404 Not Found or Quarantine
  - `Problem`: The package is blocked by Nexus Firewall as it has vulnerabilities. 
  - `Solution`: Use a non-vulnerable version of the package or use a different package. 

## Pipeline fails due to Connectivity Issue or Network not found.
  - `Problem`: Firewall issue
  - `Solution`: Follow [this document](./agents.md) to raise a Firewall request. 

## Azure ARM Service Connection
- If your pipeline wants to deploy application to Azure Cloud, we need a Service Connection of type **Azure Resource Manager** for your subscription. 
  - For example, if your subscription name is `SUB_LMS_PROD`, then we need a service connection of type "Azure Resource Manager" in Azure DevOps as `SC_ARM_LMS_PROD`. 

- Only Cloud security team can do this. Open snow ticket with cloud security team for this [here](https://premierprod.service-now.com/premiernow?id=dept_cat_item&sys_id=dd18f4381bbb3050be08975e034bcb37)

  | Field | Value |
  | --- | --- |
  | Requested For | [your name] | 
  | Name | SC_ARM_LMS_PROD | 
  | Purpose | Create ARM Service Connection in Azure DevOps Project: [ADO Project Name] |
  | Subscription | SUB_LMS_PROD |

## SSH Service Connection
This is used for establishing communication with Linux VM. If you want to copy files to any VM or want to run scripts on remote VM, then you need this service connection. To create SSH Service Connection for a VM:
- Go to Project Settings
- Under pipelines, Select **Service Connection**
- Add New
- Search for `SSH` and select it.
- Fill values
- Make sure the service connection name is in format `[username]_[hostname]`. For example: **scauser_c3duabc1**

## Unauthorized to push Images to ACR
- `Problem`: The Service Connection may not have access to push Images to ACR.
- `Solution`: Please work with Cloud DevOps Team and request them to create the necessary permissions for the SC (Service Connection) to push Images to ACR.
