# Cloud-Infrastructure
This repository stores my learning about cloud infrastructure using AWS. 

## My Project Environment:
* AWS
* AWS CLI
* Docker
* Terraform
* VS Code
* WSL
* EC2 Server instances

## Steps Creating First Project:
* Create an AWS account and install AWS CLI through VS Code WSL.
* Install Docker and Terraform; these are optional but some experience is better than no experience.
* Create a new user through Identity and Access Management (IAM) in AWS Management Console.
* Set up that user with an Access Key and will be paired with a Secret Key. Save the Secret Key.
* Launch first instance. Search for EC2 in AWS Management Console.
* Click on "Launch instance". Go through the following options:
    * Server name is "My First Server"
    * Application and OS image is Amazon Linux; Amazon Linux 2023 AMI; 64-bit (x86)
    * Instance type is t2.micro
    * Create a new key pair. "MyKeyPair". I chose ED25519 and saved it as .pem and clicking create will download the private key while the public key is stored on AWS.
    * Network settings, check "Allow HTTP traffic from the internet"
    * Everything else is left as Default. Click "Launch instance" on the left and a server instance is created.

* Take note of the server instance ipv4 address to connect to it through the terminal.
* In VS Code, connect to WSL and run "aws configure". It will prompt you with Access Key, Secret Key, Region, and format. Format was left on Default.
* Now connect to the server using "ssh -i "MyKeyPair.pem" ec2-user@instance-ip" for example: ssh -i "NewKeyPair.pem" ec2-user@1.23.456.789
* Setup a simple Web Server using Apache
* sudo yum install httpd -y
* sudo systemctl start httpd
* sudo systemctl enable httpd
* echo "Hello, World!" | sudo tee /var/www/html/index.html

That's my first project getting started on learning cloud infrastructure

# Learning
Cloud-Infrastructure in today's modern use of software development allows the ability for one to control a remote instance programmatically. This project is to serve as my own learning about cloud-infrastructure.
So far, I'm able to start and stop an instance by using aws ec2 start-instances --instance-ids "instanceID" and aws ec2 stop-instances --instance-ids "instanceID" and this can be done without being connected to the instance itself
with ssh -i "MyKeyPair.pem" ec2-user@instance-ip. 

My learning will focus on a few milestones that I want to complete. These are as follows:
## Compute:
* Use EC2 instances programmatically via the AWS CLI
* Deploy and scale containers using AWS Elastic Beanstalk or with Docker

## Storage:
* S3: Storing static files
* EBS: Attach storage volumes to EC2
* Host a static website on S3

## Networking:
* Build and configure networks
* Create virtual private clouds (VPCs)
* Configure Subnets, Route Tables, and Gateways
* Secure EC2 instances with Security Groups

## Infrastructure as Code
* Optional: Write Terraform scripts to spin up resources
* Deploy an EC2 + S3 setup using Terraform



# What's Next?

## Compute
In this section, I'm going to learn how to programmatically control my EC2 instances through AWS CLI by itself and deploy containers using Docker.
