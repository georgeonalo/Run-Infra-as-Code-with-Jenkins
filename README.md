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

scroll down and select the **t2.micro** under **instance type**

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

## Step 3: Configuring Jenkins

Jenkins is now installed and running on your EC2 instance. To configure Jenkins:

Connect to http://<your_server_public_DNS>:8080 from your browser. You will be able to access Jenkins through its management interface:

![image](https://user-images.githubusercontent.com/115881685/211005628-22ea9d87-5f70-4280-a4a3-985781ab1637.png)
As prompted, enter the password found in /var/lib/jenkins/secrets/initialAdminPassword.

Use the following command to display this password:

sudo cat **/var/lib/jenkins/secrets/initialAdminPassword**

The Jenkins installation script directs you to the **Customize Jenkins page**. Click **Install suggested plugins**.

![jenkins 8](https://user-images.githubusercontent.com/115881685/211006367-90d8125a-c107-477b-a5f3-4d70c3f74dbe.JPG)

Once the installation is complete, the **Create First Admin User** will open. Enter your information, and then select **Save** and **Continue**.

![jenkins 9 create a new user](https://user-images.githubusercontent.com/115881685/211006652-e41e4410-fd1d-4795-9a79-561037b052e1.JPG)

Once this is done, you will be taken to the home page.
![welcome to jenkins 9](https://user-images.githubusercontent.com/115881685/211007211-dab827f3-66d8-47a3-a098-474d91da4355.JPG)

## Step 4: Install cloudformation plugin.

In the jenkins dashboard, click **manage jenkins** and then click **manage plugin,** click **available**, search **cloudformation**, then select it, click **install without restart**, then **restart jenkins**

## Step 5: Run Cft using using Cft Plugin

Go back to the jenkins home page and select **new item** to create a job.

In the **enter an item name** box, type **cftdemo-jenkinsplugin**

Click **freestyle project** and enter **ok**

![jenkins 10](https://user-images.githubusercontent.com/115881685/211030585-36f7142b-05d4-44c4-9542-b19b9fcac5a6.JPG)

In the new page that has opened, scroll down down to source code management, click Git, go to your github account that has a [cloudformation-demo ](https://github.com/georgeonalo/cloudformation-demo)repository that has a [jenkinsfile](https://github.com/georgeonalo/cloudformation-demo/blob/main/Jenkinsfile) and a [simplests3cft.json](https://github.com/georgeonalo/cloudformation-demo/blob/main/simplests3cft.json) script

Copy the **Cloudformatiom-demo** url and paste it in the **Repository URL** box. Under **Credentials**, click **add jenkins**, provide your Github **user ID and password**, leave the other options blank.

![jenkins 14](https://user-images.githubusercontent.com/115881685/211033697-9c18c9ae-6651-496f-8ec6-15a8ed7c8208.JPG)
![jenkins 13](https://user-images.githubusercontent.com/115881685/211033774-96bbc1b6-e3aa-471c-bce5-c309ca1e1636.JPG)

Scroll down to **Build Environment** and click Create **AWS Cloud Formation stack**, enter **""simplests3cft.json""** into the **.json box** and enter **"jenkinscftplugin-s3"** into the **Stack name** field.


Go to the aws console and create an IAM user that only has access to **Cloudformation** and **S3**, we are doing this for security purposes. When you create the user, an access key and secret key will be generated, copy both and paste it under the **AWS access key** and **AWS secrete key** field  respectively in the jenkins job. Uncheck **automatically delete stack**

Click save

![jenkins 15](https://user-images.githubusercontent.com/115881685/211038303-5c0fcbcc-49e4-4674-afac-259bc26a9941.JPG)
![16](https://user-images.githubusercontent.com/115881685/211041114-c7ed60cb-abc1-46ae-a9d7-71ee23146cd9.JPG)


Now u=in the next page that opens click **Build now** so that we can manually trigger the build job, we will automate this task later.

![jenkins 16](https://user-images.githubusercontent.com/115881685/211039138-629d75c5-bf05-4bf4-8e05-8b0a9f08e8f2.JPG)

Click **#1** beside the build job display in the bottom left hand corner of the page, click **console output** to see the system logs of the buid job.

The system logs shows that Cloudformation stack is been created

![jenkins17](https://user-images.githubusercontent.com/115881685/211040406-96da135b-f419-4abe-9ae1-f08d090e1d64.JPG)
![18](https://user-images.githubusercontent.com/115881685/211041201-36850787-232e-4c49-a142-5d610bce068c.JPG)


To comfirm the if CFT stack has been create go to cloudformation in the AWS console.

![19](https://user-images.githubusercontent.com/115881685/211040958-99208dfb-3a44-4fb9-9b39-d20d9486584b.JPG)

 ## Step 6: Install AWS CLI without keys using SSM Session Manager
 
 Go to the **AWS console** and search for **session manager**, once there, Click on **quick setup** and follow the instructions, once done, scroll down to **remote connect** under **features**, click it. Click **start session**, this reveals our **jenkins EC2 instance**, click **start session** again to open up a **shell**
 
 In the shell, run the following command:
 
 sudo yum update
 
 sudo yum install awscli
 
 Run "aws configure" to verify that awscli is installed correctly
 
 ![20](https://user-images.githubusercontent.com/115881685/211050349-a0b74d8c-5a61-4921-afad-bda7a39a1ba8.JPG)







    
    






Click this link: [part one](https://youtu.be/b6z6i3Lrp04) for a short video tutorial on how to execute the project


## Part 2


Click this link: [Part two](https://youtu.be/XnRqGMSCQyY) for instructions on how to execute the second part of the project.


