DevOps Task - Voting App

#This repository contains the necessary files and configurations to automate the deployment of a voting application on a Kubernetes cluster using Helm.#

Prerequisites:

- Vagrant installed
- Helm installed
- Kubernetes cluster up and running

##Getting Started##

1. Create a local directory and Add the Vagrant box:

#Commands#

mkdir LCO_linux_install_demo

cd LCO_linux_install_demo/

mkdir shared_resources

touch Vagrantfile

vagrant box add ubuntu/bionic64

vagrant box list | grep bionic64

vagrant.exe init ubuntu/bionic64 --force

-Initialize the Vagrant environment, use Vagrant to provision the VM and Boot up the machine using:

#Command#

vagrant.exe up

-After that,log in to the guest Ubuntu VM using:

#Command#

vagrant.exe ssh

------------------------------------------------------------

2. Install Kubernetes Locally:

Install a local Kubernetes cluster using a K3s tool.

Install K3s:

#Commands#

curl -sfL https://get.k3s.io | sh -

Check K3s Pods:

#Command#

sudo kubectl get pods -n kube-system

---------------------------------------------------
3. Install Helm:


#Command#

curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null

sudo apt-get install apt-transport-https --yes

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list

sudo apt-get update

sudo apt-get install helm

Verify Helm Installation:

#Command#

helm version

----------------------------

4. Deploments and services for Voting Application:
 
 -Deployments:

a- voting-app-deploy.yaml : front-end web app in Python which lets you vote between two options.

b- redis-deploy.yaml: Redis which collects new votes.

c- worker-app-deploy.yaml: .NET worker which consumes votes and stores them in the DB.

d- postgres-deploy.yaml: Postgres database backed by a persist volume.

e- result-app-deploy.yaml: Node.js web app which shows the results of the voting in real time.

  -Services type:

a- postgres-service.yaml: cluster IP type (for internal communication).

b- redis-service.yaml: cluster IP type (for internal communication).

c- worker-service.yaml: cluster IP type (for internal communication).

d- result-app-service.yaml: Node port (to be able to use external access).

e- voting-app-service.yaml: Node port (to be able to use external access).

--------------------------------
5. Encrypt Database Credentials:

Use Kubernetes Secrets for encrypting sensitive information.

#Use this command to encode the values of username and password#

echo -n "postgres" |  base64

output was "cG9zdGdyZXM=" for the username and password which were used in the secret.yaml file.

-------------------------------
6. Database Persistence:

Ensure database persistence by configuring persistent volume and persistent volume claim.

can be found in pv.yaml and pvc.yaml.

--------------------------------
7. Auto-Scaling:

Configure Horizontal Pod Autoscaling for the voting application.

can be found voting-hpa.yaml

----------------------------------

8. Generate First Helm Chart:

#Command#

helm create mychart

Helm will create a new directory in the project called mychart with the specified structure.

-----------------------------------

9. Create YAML Files in Templates Directory:

Manually create YAML files in the templates directory.

----------------------------------------

10. Install Helm Chart:

#Commands#

cd /home/vagrant

helm install votingpp ./mychart

---------------------------------------------
11. to verify the application is up 

kubectl get pods 

kubectl get deployments

kubectl get services

kubectl get secrets

kubectl get pv

kubectl get pvc

Curl http://192.168.56.104:30004 to vote

Curl http://192.168.56.104:30005 to see the result
