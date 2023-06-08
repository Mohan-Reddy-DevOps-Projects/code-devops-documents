# Purpose
This example covers pipeline steps for deploying NPM webpack application to a Linux VM.

# Pre-requisite
- We are using NPM package [pm2](https://www.npmjs.com/package/pm2) for running the application on the VM
- Make sure the pm2 is installed on the VM using command: 
    ```
    npm -g pm2
    ```
- An SSH Service Connection must be created in ADO for deploying application to Linux VM. Refer the [steps to create SSH connection](../../troubleshooting.md#ssh-service-connection)

# Steps
- `pipeline/azure-pipeline.yaml` is the starting file.
- Templatized build & deployment.
- `pipeline/ci-template.yaml` has two jobs.
  1. Build
    - install NodeJS. Change it to the version as your app requires.
    - install npm dependencies
    - build project
    - create artifact
  2. Security Scan
    - Checkmarx
    - Lifecycle
- `pipeline/deploy-template.yaml` contains the deployment script.  You can see that we are using PM2 for running the app. 
