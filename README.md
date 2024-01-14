# Voting-app-
1- Create a local directory and Add the Vagrant box to it
a-mkdir LCO_linux_install_demo
b-cd LCO_linux_install_demo/
c-mkdir shared_resources
d-touch Vagrantfile
e-vagrant box add ubuntu/bionic64
f-vagrant box list | grep bionic64
ubuntu/bionic64     (virtualbox, 20210315.1.0)

2-Initialize the Vagrant environment 
vagrant.exe init ubuntu/bionic64 --force 

3-edit the vagrant file configurations then boot up the machine
vagrant.exe up
after than use vagrant.exe ssh to log in to the guest Ubuntu VM

4-Inastall K3s 
curl -sfL https://get.k3s.io | sh -

5-Use this command to check if the pods of thr k3s system is up
sudo kubectl get pods -n kube-system

6-install helm 
a- curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
b- sudo apt-get install apt-transport-https --yes
c- echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
d- sudo apt-get update
e- sudo apt-get install helm

7-Verify installation 
helm version

8- Generate first chart
helm create mychart
helm will create a new directory in your project called mychart with the structure shown below
mychart
|-- Chart.yaml
|-- charts
|-- templates
|   |-- NOTES.txt
|   |-- _helpers.tpl
|   |-- deployment.yaml
|   |-- ingress.yaml
|   `-- service.yaml
`-- values.yaml

9- create the yaml files into templates dir 

10 - cd /home/vagrant
helm install votingpp ./mychart
