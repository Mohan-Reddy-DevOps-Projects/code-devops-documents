# Azure DevOps Linux Pipeline
This document contains various pipeline steps that can be followed in Azure DevOps Pipeline for different use cases when you are using **Premier Self Hosted Linux Agents**.

# Table of Contents

- [Pre-requisite](#pre-requisite)
- [Available Packages](#available-packages)
- [Environment Variables](#environment-variables)
- [Pipeline Steps](#pipeline-steps)
  - [Java Setup](#java-setup)
  - [Maven Setup](#maven-setup)
  - [Gradle Setup](#gradle-setup)
  - [NodeJS Setup](#nodejs-setup)
  - [Yarn Setup](#yarn-setup)
  - [GO Setup](#go-setup)
- [Library](#library)
- [Environment](#environment)
- [Reference](#reference)

# Pre-requisite
Premier Agents are hosted within the Premier network that provides access to other applications that are hosted within Premier. For example, **[Nexus](https://nexus.premierinc.com/artifacts)** application.  In order to use Premier Agents in your pipeline, specify the Pool name in the pipeline YAML code.  

## Pipeline level
If you want your entire pipeline to be executed in Premier agents, then specify
```YAML
pool:
  name: "Premier Linux Agents"
```

## Stage level
If you want to run a specific stage in Premier agents, then specify
```YAML
stages:
- stage: Build
  pool: "Premier Linux Agents"
  jobs:
  - job: ...
```

## Job Level
If you want to run a specific job in Premier agents, then specify
```YAML
jobs:
- job: ABC
  pool: "Premier Linux Agents"
```

# Available Packages

| Name | Use | Example | 
| --- | --- | --- |
| wget | wget [URL] | `wget https://google.com` |
| Unzip | unzip [zip File] | `unzip bundle.zip` |
| Docker | docker [command] | `docker version` |
| Python3 | python3 [python file] | `python3 file.py` |
| Python3-pip | python3 -m pip install [pip package] | `python3 -m pip install requests` |

> **Note**  - Please find more packages in the next section (Environment variables). If any package required by your project is not available in the agent, please create a SNOW ticket.  Our Team will install the packages. 

# Environment Variables

We recommend teams use the below environment variables to use the packages installed in the agent. 

| Name | Value | Example |
| --- | ---  | --- |
| SYSTEM_TOOLS | /opt/app/code/automate/tools | |
| SYSTEM_JDK_8      | $SYSTEM_TOOLS/jdk-8         | `$(SYSTEM_JDK_8)/bin/java -version` |
| SYSTEM_JDK_11     | $SYSTEM_TOOLS/jdk-11        | `$(SYSTEM_JDK_11)/bin/java -version` |
| SYSTEM_JDK_17 | $SYSTEM_TOOLS/jdk-17  | `$(SYSTEM_JDK_17)/bin/java -version` |
| SYSTEM_GH_CLI | $SYSTEM_TOOLS/gh_cli/bin/gh | `$(SYSTEM_GH_CLI) --version`. Also this executable is added to PATH.  So you can also use `gh --version` |
| SYSTEM_KUBECTL | $SYSTEM_TOOLS/kubectl | `$(SYSTEM_KUBECTL) version --client`.  This executable is added to PATH. So you  can also use `kubectl version --client` |
| SYSTEM_MAVEN_3 | $SYSTEM_TOOLS/apache-maven-3/bin/mvn | `$(SYSTEM_MAVEN_3) --version` |
| SYSTEM_DOCKER_COMPOSE | $SYSTEM_TOOLS/docker-compose | `$(SYSTEM_DOCKER_COMPOSE) version`. This executable is also added to PATH. So you can use it as `docker-compose version` |
| SYSTEM_NODEJS | $SYSTEM_TOOLS/node | `$(SYSTEM_NODEJS)/bin/node --version`. This executable is also added to PATH. So you can use it as `node --version` or `npm --version` |
| SYSTEM_YARN | $SYSTEM_TOOLS/yarn | `$(SYSTEM_YARN)/bin/yarn --version`. This executable is also added to PATH. So you can use it as `yarn --version` |
| SYSTEM_LIQUIBASE | $SYSTEM_TOOLS/liquibase/liquibase | `$(SYSTEM_LIQUIBASE) --version`. This one holds the latest version of liquibase at all times. If you want a specific version, let us know. We will install and provide an ENV variable |
| SYSTEM_LIQUIBASE_3_5_3 | $SYSTEM_TOOLS/liquibase-3.5.3/liquibase | `$(SYSTEM_LIQUIBASE_3_5_3) --version`. This points to liquibase version 3.5.3 |
| SYSTEM_GO | $SYSTEM_TOOLS/go | `$(SYSTEM_GO)/bin/go version`. This executable is also added to PATH. So you can use it as `go version` directly |  

## Best Practise
In case if your pipeline is using the environment variables, you can add that to demands and hence it picks up agents with those capabilities. 
```YAML
pool:
  name: Premier Linux Agents
  demands:
  - SYSTEM_JDK_8
  - SYSTEM_MAVEN_3
```

### Reference [Microsoft Document](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/demands?view=azure-devops&tabs=yaml#manually-entered-demands)  


# Pipeline Steps

## Java Setup

### Example 1: Set & Use java in same task
```YAML
- bash: | 
    export JAVA_HOME="$SYSTEM_JDK_8"
    echo "$JAVA_HOME"
    $JAVA_HOME/bin/java -version
```

### Example 2: Set JAVA_HOME and use java in different task
```YAML
- bash: echo "##vso[task.setvariable variable=JAVA_HOME]$(SYSTEM_JDK_8)"
- bash: |
    echo $JAVA_HOME
    echo $JAVA_HOME/bin/java -version
```

## Maven Setup
Maven task depends on the JDK & Maven Path.  Please refer to environment variables section above for JDK and Maven path values. 

The below snippet uses JDK 17 and Maven 3 to run the Maven Task.

### Method 1: Using Maven Task
**Reference**:  [Maven Task Official Document](https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/build/maven?view=azure-devops)
```YAML
- task: Maven@3
  inputs:
    mavenPomFile: 'pom.xml'
    goals: 'clean deploy'
    options: '-B'
    publishJUnitResults: false
    javaHomeOption: 'path'
    jdkDirectory: $(SYSTEM_JDK_17)
    mavenVersionOption: 'path'
    mavenPath: $(SYSTEM_MAVEN_3)
```

### Method 2: Using Maven Command Line
```YAML
- bash: $MAVEN clean deploy
  displayName: Maven Deploy
  env:
    JAVA_HOME: $(SYSTEM_JDK_17)
    MAVEN: $(SYSTEM_MAVEN_3)
```

## Gradle Setup
Gradle task in ADO works only if you have Gralde Wrapper File in your project. Also it depends on the JDK path that is required to run the Gradle Daemon.  Please refer to environment variables section above for JDK values.  The below snippet uses JDK 11 to run the Gradle task

### Method 1: Using Gradle Task
**Reference**:  [Gradle Task Official Document](https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/build/gradle?view=azure-devops)
```YAML
- task: Gradle@2
  displayName: Gradle Build
  inputs:
    gradleWrapperFile: 'gradlew'
    tasks: 'clean build'
    publishJUnitResults: false
    javaHomeOption: 'Path'
    jdkDirectory: "$(SYSTEM_JDK_11)"
    gradleOptions: '-Xmx3072m'
```

### Method 2: Using Command Line
```YAML
- bash: chmod +x gradlew && ./gradlew clean build
  displayName: Gradle Build
  env:
    JAVA_HOME: $(SYSTEM_JDK_11)
```

## NodeJS Setup
### Method 1: Direct reference
```YAML
- bash: node --version
```
### Method 2: Using ENV variable
```YAML
- bash: $NODE --version
  env:
    NODE: $(SYSTEM_NODEJS)/bin/node
```

## YARN Setup
### Method 1: Direct Reference
```YAML
- bash: yarn --version
```
### Method 2: Using ENV variable
```YAML
- bash: $YARN --version
  env:
    YARN: $(SYSTEM_YARN)/bin/yarn
```

## GO Setup
### Method 1: Using ENV variable
```YAML
- bash: $GO version
  env:
    GO: $(SYSTEM_GO)/bin/go
 ```
 
### Method 2: Direct Reference
```YAML
- bash: go version
```

# Library
Go [here](https://github.com/PremierInc/code-devops-documents/blob/main/azure_devops/pipeline/library.md)

# Environment
Go [here](https://github.com/PremierInc/code-devops-documents/blob/main/azure_devops/pipeline/environments.md)

# Reference
- [Pipeline YAML schema official reference document](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema%2Cparameter-schema)

- Few sample pipeline configurations for difference use-cases are provided in the [examples](https://github.com/PremierInc/code-devops-documents/tree/main/azure_devops/pipeline/examples). 



