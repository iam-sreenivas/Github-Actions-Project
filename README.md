# Intro
# Agenda
# GitHub Actions
# Setup Project Repo

# Configure VM as a Private Runner
  - Create one instance in azure/aws
  - enable inbound and outbound ports for http and https
  - use public ip and connect to terminal
  - go to github and create one runner for self hosted
  - run the commands
    
# Set Up SonarQube Server
# SonarQube Job in Pipeline
  - create one ec2 instance for sonarqube server and connect to terminal
  - sudo apt update
  - docker # by default docke ris not present we need to install docker
  - sudo apt install docker .io -y # Once docker installed we need to provide ubuntu use permission to docker
  - sudo usermod - aG docker $USER
  - newgrp docker
  - we need to setup docker container
  - docker run -d --name sonar -p 9000:9000 sonarqube:lts community
  - Copy Public ip and port 9000 # connected to sonarqube server
  - go to administartion tab genarate token in sonarqube
  - create one file in github named as sonar-project-properties
     sonar.projectKey=GC-Bank
     sonar.projectName=GC-Bank
     sonar.java.binaries=.
 - Copy host token and sonar url and add it in secrets in github #b https://github.com/marketplace/actions/official-sonarqube-scan
 - add sonarqube quality gate # https://github.com/marketplace/actions/sonarqube-quality-gate-check
 - sonarqube stage failed bcaz unzip command not found
 - we need to install unzip command in server
 - sudo apt install unzip
 - Once done... go to sonarqube server and check
   

# Setup Docker Job in Pipeline
- need to install docker in runner
- # Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
- sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
- sudo usermod -aG docker ubuntu
- newgrp docker
- close the runner and again start
  
# Setup EKS Kubernetes Cluster
- create one ec2 instance to setting up EKS cluster
- sudo apt update
## Install AWS CLI
- curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
- aws configure # to connect Aws account
- aws account and secret access key need to provide
- region

## Install terraform 
- sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
- wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
- terraform -version

# EKS-Terraform
- Go to inside EKS Terraform folder
- terraform init
- terraform plan
- terraform apply --auto approve

## Install kubectl
- curl -LO https://dl.k8s.io/release/v1.34.0/bin/linux/amd64/kubectl
- curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
- udo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
- kubectl version --clinet


chmod +x kubectl
mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl
# and then append (or prepend) ~/.local/bin to $PATH

## Kubectl get nodes - by default it failed not yet connected to the cluster for that setup kubeconfig file
- aws eks region ap south -1 update-kubeconfig --name


## Now try to run kubectl get nodes

## ls command
eks terraform aws cli

- cat ~/.kube/config
  Copy the kubeconfig file and add it in secrets

# Kubernetes Job in Pipeline

step by step process
