# Install Jenkins on EC2 in ONE Click | Run CloudFormation from Github using Jenkins

In this project, we are going to carry out the following:

* install Jenkins
* Install CloudFormation Plugin
* Run Cft using using Cft Plugin
* Install AWS CLI
  * Use SSM Sessinn Manager to Install without any keys
* Jenkins File
* Run Cft using AWS CLI Commands

## Jenkins Demo Overview/Workflow

![image](https://user-images.githubusercontent.com/115881685/209539104-72e6b752-151f-4701-99e4-ec5b38161ba7.png)

## Prerequisite
- Aws account

- Github aacount

## Step 1: Create an EC2 instance

log into your **aws console** and go to ec2 service, click on **launch instance**, name the instance **jenkins**.

![jenkins  1](https://user-images.githubusercontent.com/115881685/210998997-336f1467-3281-41be-830a-036d66fe926b.JPG)

scroll down and choose the **amazon linux 2 AMI under the free tier

![jenkins  2](https://user-images.githubusercontent.com/115881685/211000502-994a3b18-2bb5-468a-bc5d-3eb2d716b98c.JPG)

scroll down and select the **t2.micro under instance type**

![jenkins 3](https://user-images.githubusercontent.com/115881685/211000795-bb27bdcb-1cef-479f-a559-31faff089f8b.JPG)

Lastly create your **key pairs(for ssh)**, **security group** and click on the orange button that says **launch instance**

![jenkins 4](https://user-images.githubusercontent.com/115881685/211001358-91c9b9cb-ba6f-42e4-aef9-12dd6dea5843.JPG)
![jenkins 4 J](https://user-images.githubusercontent.com/115881685/211001399-97e82284-2a18-4818-b898-febb4674be31.JPG)

Go to the EC2 home page to see your instance being created, when the instance is finally up, we are going to connect to it via **browser-based SSH connection**

![jenkins 5](https://user-images.githubusercontent.com/115881685/211002433-47357e2a-83f9-4d92-a58c-f764ea84aed4.JPG)
![jenkins 6](https://user-images.githubusercontent.com/115881685/211002497-f08175f7-d2d0-4b1d-9ed7-5f1bf0246a2e.JPG)

## Step 2: install jenkins on the EC2 instance

Run the following command to install jenkins on the ec2.

sudo yum update –y

sudo wget -O /etc/yum.repos.d/jenkins.repo \ https://pkg.jenkins.io/redhat-stable/jenkins.repo

sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
 
sudo yum upgrade

sudo amazon-linux-extras install java-openjdk11 -y

sudo yum install jenkins -y

sudo systemctl enable jenkins

sudo systemctl start jenkins

Finally run this command to see if your jenkins is up and running:

sudo systemctl status jenkins

![jenkins 7](https://user-images.githubusercontent.com/115881685/211004063-86f24a35-b929-48b4-b23a-06e9fc29ce8e.JPG)

    
    






Click this link: [part one](https://youtu.be/b6z6i3Lrp04) for a short video tutorial on how to execute the project


## Part 2


Click this link: [Part two](https://youtu.be/XnRqGMSCQyY) for instructions on how to execute the second part of the project.


