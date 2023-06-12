
**What are GITHUB actions?**

GitHub Actions is a CI/CD platform that allows you to automate your build, test, and deployment process. You can build and test every pull request in your repository with processes, or you may deploy merged pull requests to production.

GitHub Actions extends beyond DevOps by allowing you to conduct processes in response to other events in your repository. For example, when someone files a new issue in your repository, you may execute a process that automatically adds the required labels.

To execute your processes, GitHub provides virtual machines for Linux, Windows, and macOS, or you may host your own self-hosted runners in your own data center or cloud infrastructure.

It is an extremely powerful tool for running CI processes in a developer-friendly way. Actions enables seamless CI coordination while allowing the company to keep its secrets private and maintain control over the computing infrastructure.

**[The components of GitHub Actions](https://docs.github.com/en/actions/using-workflows)**

You may set up a GitHub Actions process to be triggered whenever an event occurs in your repository, such as the opening of a pull request or the creation of an issue. Your workflow includes one or more jobs that can execute sequentially or concurrently. Each job will operate in its own virtual machine runner or container, and will have one or more stages that either execute a script you describe or perform an action, which is a reusable addition that can simplify your workflow.

Diagram of an event triggering Runner 1 to run Job 1, which triggers Runner 2 to run Job 2. Each of the jobs is broken into multiple steps.

![image](https://github.com/PremierInc/code-devops-documents/assets/99402485/58d5c3cd-c6b1-4949-af9f-089744894a24)

  - [Workflows](https://docs.github.com/en/actions/using-workflows)
  A workflow is a programmable automated procedure that does one or more jobs. Workflows are specified by a YAML file that is checked into your repository and will execute when an event in your repository triggers them, or they may be activated manually or on a predefined timetable.
  Workflows are specified in a repository's .github/workflows directory, and a repository can have many workflows, each of which can conduct a unique set of actions. For example, you may have one process that builds and tests pull requests, another that deploys your application whenever a release is made, and still another that adds a label if someone opens a new issue.

  - [Events](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows) 
  An event is a specific behavior in a repository that initiates the execution of a process. For example, activity on GitHub can begin when someone submits a pull request, starts an issue, or commits to a repository. You may also plan a procedure to run on a regular basis by submitting to a REST API or manually.

  - [Jobs](https://docs.github.com/en/actions/using-jobs) 
  A job is a collection of process stages that are all executed on the same runner. Each step is either a shell script to be executed or an action to be performed. Steps are carried out sequentially and are interdependent. Because each step is done on the same runner, data may be shared from one step to the next. For example, you may have a step that produces your application, followed by a step that tests the constructed application.

  - [Actions](https://docs.github.com/en/actions/creating-actions) 
  An action is a custom application for the GitHub Actions platform that performs a difficult but regularly performed job. Use an action to help decrease the amount of repeated code in your workflow files. An action can get your git repository from GitHub, install the necessary toolchain for your build environment, or configure authentication with your cloud provider.

# References:
- [Understanding GitHub Actions](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions#understanding-the-workflow-file) 
- [Examples for testing the code](https://docs.github.com/en/actions/examples) 
- [Github Marketplace](https://github.com/marketplace?type=actions) 
- [How to build a CI/CD pipeline with GitHub Actions in four simple steps](https://github.blog/2022-02-02-build-ci-cd-pipeline-github-actions-four-steps/) 
