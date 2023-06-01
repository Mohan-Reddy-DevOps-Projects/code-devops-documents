# Purpose
This document explains how to configure `Spring Cloud Config Server` application so that it pulls configuration from GitHub repository.  Previously teams were using Bitbucket repository to pull configurations. Going forward, they have to follow this document to switch to GitHub.

# Pre-requisite
- Create a Identity File Key Pair using below command.  This creates two files: `id_ecdsa` (private key) and `id_ecdsa.pub` (public key).

  ```bash
  ssh-keygen -t ecdsa -m pem -f id_ecdsa
  ```

- Hand over the `id_ecdsa.pub` file to GitHub Admin.
- Request them to add the public key to the GitHub repository **Settings** -> **Deploy Keys** and provide read-only access to the Keys.

> **Note**: 
> - You cannot re-use the same keys if you want to configure more than 1 repository. Use unique key-pair for each GitHub repository.
> - `ECDSA` is one type of encryption. Similary you can use `ED25519`. 
> - `RSA` format not supported


# Steps
- We need three files:
  - ~/.ssh/known_hosts
  - ~/.ssh/config
  - ~/.ssh/<private_key>

- Create the `known_hosts` file by running the below command and save it in the environment (VM or Docker container) where App runs.

  ```bash
  curl --silent https://api.github.com/meta | jq --raw-output '"github.com "+.ssh_keys[]' >> ~/.ssh/known_hosts
  ```
- Store the private key file created in the pre-requisite in the app environment (VM/Docker container) where app run in location `~/.ssh/id_ecdsa`
    ```bash
    mkdir -p ~/.ssh
    chmod 700 ~/.ssh
    cp id_ecdsa ~/.ssh/id_ecdsa
    chmod 600 ~/.ssh/id_ecdsa
    ```
- Create SSH `~/.ssh/config` file. 
  ```
  Host github.com
    Hostname ssh.github.com
    Port 22
    User git
    IdentityFile ~/.ssh/id_ecdsa
    StrictHostKeyChecking yes
  ```

  > **Note**: Port 443 is not supported for Cloning via SSH over HTTPS port for GitHub Enterprise. 

- In your `application.yml` file, specify the GitHub repository URL:
    ```yaml
    spring:
        cloud:
            config:
                server:
                    git:
                        uri: git@github.com/premierinc/lms-config.git
                        default-label: main
    ```

# Reference
- [Spring Cloud Config](https://cloud.spring.io/spring-cloud-config/multi/multi__spring_cloud_config_server.html#_spring_cloud_config_server)
- [GitHub Deploy Keys](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys#deploy-keys)
