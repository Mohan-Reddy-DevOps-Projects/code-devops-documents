
To create Github Actions for Nexus IQ we need to have access to actions in Github. Open the actions tabs in your repository and choose "New workflow."

---

**Note:** Github Actions for Sonatype Nexus is NOT OFFICIALLY SUPPORTED by Sonatype; hence, you will not find it in the marketplace; instead, select "Simple Workflow" and customize it as needed.

---

"Simple Workflow" gives you some basic workflow to help you get started with Actions

The following are some basic terms:

**name:**  
Optional - The name of the workflow as it will appear in the "Actions" tab of the GitHub repository.

**on:** Controls when the process will be executed  
for example: Initiates the workflow on push or pull request events, but only for the "main" branch (if left blank, this is triggered by a push and pull to every branch).

  **push:** (When someone pushes a change to the repository)  
  **branches:** [ "main" ]  
  **pull_request:** (When merges a pull request)  
  **branches:** [ "main" ]  

**workflow_dispatch:**  
Allows you to perform this workflow manually from the Actions tab.

**jobs:**  
A workflow run is made up of one or more tasks that can execute sequentially or in parallel.

**build:**  
This is the name of a job; there is only one task in this workflow named "build."

**runs-on:**  
This should be set to "premierinc" The kind of agent on which the task will run.

**Steps:**  
It illustrates a series of tasks that will be completed as part of the work. Each item in this section is a distinct action or shell script.

**uses: actions/checkout@v3**  
The uses keyword specifies that this step will run v3 of the actions/checkout action. This is an action that checks out your repository onto the $GITHUB_WORKSPACE, allowing you to run scripts or other actions against your code.

**run:**  
The run keyword tells the job to execute a command on the runner.

-----

Following the completion of the preceding procedures, the following are some extra actions for configuring Nexus IQ:  

 -1. We need to incorporate Java setup into our process. This configuration was obtained from the Github marketplace. - Setup Java JDK  

 -2. Install Maven and configure it to run the POM file  

 -3. Add the Nexus task as indicated in the screenshot below  
