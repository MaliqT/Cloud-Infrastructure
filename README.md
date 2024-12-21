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
* Elastic Beanstalk

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
In this section, I'm going to learn how to programmatically control my EC2 instances through AWS CLI by itself as well as create and deploy containers and environments using Docker.
New accounts after October 1st, 2024 will now require the use of launch templates in order to create an environment and deploy it. The biggest challenge here is understanding how these launch templates work.
The launch template needs to be inside a .ebextensions directory named anything. It needs to have the .config file type and in YAML or JSON format writting. Create one following this link: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environments-cfg-autoscaling-launch-templates.html
All of this needs to be inside the project application directory. Trying to create an environment without first initializing a launch template will result in a failed creation of the environment and an error will be thrown saying "Auto Scaling Group does not exist" or something along those lines.
Then using Elastic Beanstalk CLI through WSL in VS Code, run "eb init" to initialize the first instance of an application in Elastic Beanstalk, go through the steps and select those that apply and then create an environment.
Create an environment using "eb create environment-name-here" which will then process the launch template config file created. I wrote the following content inside a launch template file:

option_settings:
  aws:autoscaling:launchconfiguration:
    ImageId: "ami-myimageid"
    InstanceType: "t2.micro"
    EC2KeyName: "MyKeyPair"
    DisableIMDSv1: true
    RootVolumeType: "gp3"

This is what I wrote. In order for the creation of the environment to work, DisableIMDSv1 or RootVolumeType has to be involved in the file. At least one of those options or a few others that aren't mentioned here. The creation of the environment worked, however, with errors. Regardless, I'll work on those errors later.

### Why use Elastic Beanstalk to handle EC2 instances?
Elastic Beanstalk allows for the user to to primarily focus on the application code. Since EB provisions its own EC2 instance, this means EB handles and manages features like scaling, load balancing, health monitoring, etc.

# Learning
This section will always be updated as I progress. 

Cloud-Infrastructure in today's modern use of software development allows the ability for one to control a remote instance programmatically. This project is to serve as my own learning about cloud-infrastructure.
So far, I'm able to start and stop an instance by using aws ec2 start-instances --instance-ids "instanceID" and aws ec2 stop-instances --instance-ids "instanceID" and this can be done without being connected to the instance itself
with ssh -i "MyKeyPair.pem" ec2-user@instance-ip.

#### Using Elastic Beanstalk to manage EC2 Instances:
My biggest challenge here was getting the launch template to work. I didn't quite understand how to create one simple because reading through the documents on AWS about Launch Templates, it didn't really show or say how this is configured or setup until reading about 4-5 articles about it. Turns out, my issue was that I was initializing and creating EB inside my root directory and not in my application directory. So I created an entirely new directory and called it MyApp and inside that directory I ran "eb init" and set everything up, then I created the .ebextensions directory and the launch-template.config file and wrote the contents. And ran "eb create test-environment" and it finally worked, but there are still some errors that I need to figure out but the environment works. This means that my EC2 instance is completely handled by EB and I just need to provide the application code. 

## Notes to self:
* You can start and stop instances programmatically
* Every time an instance starts, a new ipv4 address is associated. Instead of having to connect and type this new ipv4 address every time, configure an Elastic IP address associated to the EC2 instance itself,
that way you only use the Elastic IP and it will always connect to the instance.
