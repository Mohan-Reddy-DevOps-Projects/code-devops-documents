# Purpose
Deployment of sample NodeJS app to app service without container.  For example, we used simple express framework and pm2 for starting the application.

# Pre-requisite
- Follow steps [here](../../troubleshooting.md#azure-arm-service-connection) to create ARM Service Connection.


# Steps
- Templatized the pipeline build and deployment.
- In ci.yaml file, (build)
  - we install NPM dependencies and package the application as a zip file.
  - create artifact of the zip file.
- In deploy.yaml, (deploy)
  - we deploy to native apps.
  - You need to make changes to the `StartUpCommand` in the deploy.yaml file as it varies for each framework. 