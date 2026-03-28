JENKINS SETUP:
1. Add Credentials
Go → Jenkins → Manage Credentials
Add:
Docker Hub username/password → docker-creds
SSH key for EC2
 2. Enable GitHub Webhook (AUTO TRIGGER)
 In GitHub repo:
Settings → Webhooks
Add:
http://<jenkins-url>/github-webhook/


Sonar qube:
run:docker run -d --name sonarqube -p 9000:9000 sonarqube
Acces:http://<private IP>:9000
Login Credentials:
Username: admin
Password: admin
Generate Token:
Go to My Account → Security
Generate a token
Configure SonarQube in Jenkins
Go to:
Manage Jenkins → Configure System
Add SonarQube Server:
Name: SonarQube
URL: http://<sonarqube-ip>:9000

How to Run:

docker build -t bike-app .
docker run -p 80:80 bike-app

Terraform:
terraform init
terraform apply

