# devop-tech-challenge

wtw-tech-challenge
This code performs the following actions
•	Provisions Ubuntu VM in Azure Cloud using Terraform
•	Installs NGINX and configures per assignment specifications using Salt Stack
Pre-Requisites
•	Terraform Installed
•	Azure CLI Installed and authenticated ("az login")
Steps
1.	Clone Repository and change to Terraform directory
2.	git clone git@github.com:jeremyjensen3/wtw-tech-challenge.git && cd wtw-tech-challenge/terraform
3.	Update changeme.tfvars with public IP and Public SSH key that will be used for administration of VM
4.	Initialize Terraform
5.	terraform init
6.	Run Terraform Plan, should show 7 resources being created
7.	terraform plan -var-file="changeme.tfvars"
8.	Run Terraform Apply
9.	terraform apply -var-file="changeme.tfvars"
10.	Get Public IP of VM instance, we will use this for the next steps
11.	az vm show --resource-group WTW-RG --name WTW-VM -d --query [publicIps] --o tsv
12.	SCP Salt directory to remote host for NGINX configuration, substitute public IP
13.	scp -r ../salt/ azureuser@X.X.X.X:/home/azureuser
14.	SSH to Remote Server using Public IP
15.	ssh azureuser@X.X.X.X
16.	Install and Configure Salt using provided script
17.	chmod a+x salt/configure-salt.sh && sudo ./salt/configure-salt.sh
18.	Apply Salt State using Salt Masterless Command
19.	sudo salt-call --local state.apply
Verification
1.	Update hosts file on Azure VM and add the following record
2.	127.0.0.1 www.example.com
3.	Check that backend (port 3400) is responding
Should return a response with the title "Welcome to Example.com!"
4.	curl localhost:3400
5.	Check that front-end (port 3200) is responding on www.example.com
Should return a response with the title "Welcome to Example.com!"
6.	curl www.example.com:3200
7.	Check that front-end returns custom 404 without valid domain
8.	curl localhost:3200
Clean-Up
1.	Run Terraform Destroy to remove all resources
terraform destroy -var-file="changeme.tfvars"
