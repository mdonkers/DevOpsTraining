# Exercise 001 - Setting up the environment

This first exercise is for setting up the environment for devops experience workshop.

## Prerequisites

The following prerequisites are required for participating in this workshop.

- VirtualBox + extensions
- Git client (SSH enabled)
- Favorite editor
- 20GB of disk space
- 4GB of RAM
- Virtual machine image (provided), or can be build from the machine directory.
- Internet connection

## Setting up the virtual machine

After the installation of VirtualBox, double-click on the provided image. The image will be imported and will be 
available in the VirtualBox interface. After importing the machine should be visible and can be started by 
right-clicking on the machine and selecting **start -> normal start**

![VM Setup](images/vm-setup.png)

## After starting

After starting the DevOps platform becomes available. This platform is divided into three segments.

- Development (GIT server)
- Build/Deployment (Concourse)
- Run (blue/green deployment and ngnix)

![Training environment](images/training-environment.png)


## Fork the GIT repository

By forking the GIT repository on GitHub will allow for changes to be individual

- Create a GitHub account (skip this step if a account already exists)
- Supply a SSH public <a href="https://github.com/settings/keys" target="_blank">key</a> (skip this step if SSH public key is already provided)
- Go to the training git <a href="https://github.com/codecentric/DevOpsTraining" target="_blank">repository</a>
- Select to fork option in the top right corner 
![Fork the repo](images/fork-repo.png)
- Select the account to fork to
- The repo will be available under the https://github.com/**username**/DevOpsTraining (replace **username** with 
selected username)
- Now clone the repo ```$ git clone https://github.com/username/DevOpsTraining``` (note: replace username with 
own username)

## Install Fly

Fly is the command line tooling for controlling behavior in concourse. The tooling can be downloaded from the <a href="http://localhost:23234/" target="_blank">concourse 
start page</a>. After installing the tooling can be used to login and create a alias for the 
concourse instance. <a href="https://concourse.ci/fly-cli.html" target="_blank">Fly documentation</a>

Logging in:
```bash
$ fly -t lite login -c http://localhost:23234
```
Use **concourse** as username and **changeme** as password.

## Create a secrets file

To keep secrets outside the repository a **secrets.yml** must be created. This can be done on the root of the repository. 
The **.gitignore** file does not allow for this file to be part of the repository. Create the file with the following contents. Make _real_ sure the key data has the same indentation as the dashes before and after, otherwise it won't parse as valid Yaml.

```yaml
private-repo-key: |
  -----BEGIN RSA PRIVATE KEY-----
  <your private key data>
  -----END RSA PRIVATE KEY-----
```

Just to be sure, instead of using your existing key-pair, it's possible to create a separate key-pair and use this from Concourse. First generate a new pair, and when asked where to save, choose a different location such as `~/Downloads/id_rsa`:

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

Paste the contents of the private key to the `secrets.yml` file mentioned above. Then upload the public key to GitHub here: <a href="https://github.com/settings/keys" target="_blank">https://github.com/settings/keys</a>

Now it is time to build the part of the continuous delivery pipeline.

