# Prerequisites

* Python3 or Python2.7.9+
* Python Pip3 / Pip

# Install awscli
## linux based system

```bash
pip install --user awscli
export PATH=$PATH:/home/$(whoami)/.local/bin
```

_--user_ is used to install the awscli under your home directory, not to interfere with any existing libraries/installations

create file _~/.aws/credentials_

```bash
[default]
aws_access_key_id=###
aws_secret_access_key=###
region=us-east-1
output=json
```

## Windows, using Anaconda
after Anaconda python distribution, goto "Start" => "Anaconda" => "open anaconda shell"

```
# potentially upgrade pip first
python -m pip install --upgrade pip
pip install --user awscli

set path=%path%;c:\users\<<username>>\appdata\roaming\python\python37\scripts
```

## TEST

```bash
aws --version
```


# Setup of eksctl

## Installation
for non-Linux OS you can find a binary download here:

https://github.com/weaveworks/eksctl/releases

on Linux, you can just execute:

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/latest_release/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp  

sudo mv /tmp/eksctl /usr/local/bin
```

This utility will use the same _credentials_ file as we explored for the AWS cli, located under '~/.aws/credentials'

## Test
```eksctl version```


# kubectl - the commandline K8s tool

## install _kubectl

* kubectl
  * on RH based Linux:  
  
  ```bash
  sudo dnf install kubernetes-clientcat <<EOF > /etc/yum.repos.d/kubernetes.repo
  [kubernetes]
  name=Kubernetes
  baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
  enabled=1
  gpgcheck=1
  repo_gpgcheck=1
  gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
  EOF
  yum install -y kubectl
  ```

  * on Debian/Ubuntu:
  
  ```bash
  sudo apt-get update && sudo apt-get install -y apt-transport-https
  curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
  echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
  
  sudo apt-get update
  sudo apt-get install -y kubectl
  ```

  * on Windows, open a terminal emulator, preferrably MobaXterm:
  
  ```bash
  curl -k -# -o kubectl.exe https://amazon-eks.s3-us-west-2.amazonaws.com/1.10.3/2018-07-26/bin/windows/amd64/kubectl.exe
  chmod +x kubectl.exe
  mkdir $HOME/bin
  mv kubectl.exe $HOME/bin
  echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc
  source .bashrc
  ```

## check kubectl

* Linux:  
```kubectl version --short --client```
* Windows:  
```kubectl.exe version --short --client```
