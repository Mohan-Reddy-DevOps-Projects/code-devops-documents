Table of contents
=================
<!--ts-->
   * [Purpose](#Purpose)
   * [Generate SSH Keys](#Generate-SSH-Keys)
      * [Test SSH Keys](#Test-SSH-Keys)
   * [Configure SSH Keys in Pipeline](#Configure-SSH-Keys-in-Pipeline)
      * [Bamboo](#Bamboo)
      * [ADO](#ADO) 
<!--te-->

## Purpose
This document covers steps for generating SSH key pairs for deploying the applications to the Target VM from the Pipeline.

## Generate SSH Keys
- First, we need server access. To get server access to raise the SNOW - [Add/Revoke Server Access](https://premierprod.service-now.com/premiernow?id=dept_cat_item&sys_id=38f6b8d6db449090765f1d89139619d7)
Also mention, that we need Privileged Access (dzdo/sudo) = dzdo permissions needed for <server username>

- Once access is granted, log in to the server using your username from the command prompt.
  `ssh <server>`

- Switch to the user using dzdo:
  `dzdo su - <server username>`

- Create a directory **.ssh** if it doesn't exist
  ```sh
  mkdir -p ~/.ssh
  chmod 700 ~/.ssh
  cd ~/.ssh
  ```

- Generate ssh keys
  `ssh-keygen -t rsa -m pem`
  
   ![ssh-keygen.png](https://github.com/PremierInc/code-devops-documents/blob/feature/sshkeys/General/images/ssh-keygen.png)
  

- You can view the content of the key pairs using **cat** command
- To view the public key:
  `cat ~/.ssh/id_rsa.pub`
- To view the private key:
  `cat ~/.ssh/id_rsa`
- Copy RSA private keys and save them locally as a text file.

- Copy public keys from id_rsa.pub to authorized_keys
  `cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys`
- Finally set the permission of authorized_keys
  `chmod 600 ~/.ssh/authorized_keys`

## Test SSH Keys
- Test whether the setup is correct by, Opening the command prompt with the path where private keys have been stored locally. 
- Connect server via SSH
`ssh -i <filename.txt> <server-username>@<servername>`


## Configure SSH Keys in Pipeline

## Bamboo
- Open deployment, edit environment, and _"Edit Tasks"_. Insert RSA private key in SSH key field and Save. 

![adding-ssh-pvt.png](https://github.com/PremierInc/code-devops-documents/blob/feature/sshkeys/General/images/adding-ssh-pvt.png)

## ADO
- Open Project settings in ADO and select service connections under _Pipeline_.
- Click "New Service Connection", search SSH and Next.
- Add required information in all fields and upload SSH private keys from local.
  
![ado-pvt.png](https://github.com/PremierInc/code-devops-documents/blob/feature/sshkeys/General/images/ado-pvt.png)
