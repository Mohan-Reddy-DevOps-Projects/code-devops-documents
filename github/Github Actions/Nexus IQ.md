
To create Github Actions for Nexus IQ we need to have access to actions in Github. Open the actions tabs in your repository and choose "New workflow."
######
**Note:** Github Actions for Sonatype Nexus is NOT OFFICIALLY SUPPORTED by Sonatype; hence, you will not find it in the marketplace; instead, select "Simple Workflow" and customize it as needed.

"Simple Workflow" gives you some basic workflow to help you get started with Actions

The following are some basic terms:

**name:** Optional - The name of the workflow as it will appear in the "Actions" tab of the GitHub repository.

**on:** Controls when the process will be executed
for example: Initiates the workflow on push or pull request events, but only for the "main" branch (if left blank, this is triggered by a push and pull to every branch).

  **push:** (When someone pushes a change to the repository)
  **branches:** [ "main" ]
  **pull_request:** (When merges a pull request)
  **branches:** [ "main" ]
