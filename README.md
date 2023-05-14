# Install Jenkins on EC2 and Run CloudFormation from Github using Jenkins

In this project, we are going to carry out the following:

* install Jenkins
* Install CloudFormation Plugin
* Run Cft using using Cft Plugin
* Install AWS CLI
  * Use SSM Sessinn Manager to Install without any keys
* Jenkins File
* Run Cft using AWS CLI Commands

## Jenkins Demo Overview/Workflow

![Untitled Diagram (9)](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/1a2d7a82-cf2b-4439-9282-b8dfac9dd14f)

## Prerequisite
- Aws account

- Github aacount

## Step 1: Create an EC2 instance

log into your **aws console** and go to ec2 service, click on **launch instance**, name the instance **jenkins webserver**.

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/348eb3f9-9d2d-40fe-b4b7-2ccaca2fbccd)

scroll down and choose the **amazon linux 2 AMI under the free tier**

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/b2f042fa-f893-4226-b45b-d39ea08d4bab)

scroll down and select the **t2.micro** under **instance type**, then create your **key pairs(for ssh)**

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/d296a096-1140-4a43-b728-4df04ea5744a)

Lastly create your **security group** and click on the orange button that says **launch instance**

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/3d4fd70c-feef-48e0-abf6-5f16fd34d771)


Go to the EC2 home page to see your instance being created, when the instance is finally up, we are going to connect to it via **browser-based SSH connection**

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/d5d16bd0-d37f-44cb-a0ea-ee32d248155d)

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/4338a9e1-0269-43e9-958d-c0aeb6145dd9)

## Step 2: install jenkins on the EC2 instance

Run the following command to install jenkins on the ec2.

```
sudo yum update –y
sudo wget -O /etc/yum.repos.d/jenkins.repo \ https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
sudo yum upgrade
sudo amazon-linux-extras install java-openjdk11 -y
sudo yum install jenkins -y
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

Finally run this command to see if your jenkins is up and running:

- sudo systemctl status jenkins

![jenkins 7](https://user-images.githubusercontent.com/115881685/211004063-86f24a35-b929-48b4-b23a-06e9fc29ce8e.JPG)

## Step 3: Configuring Jenkins

Jenkins is now installed and running on your EC2 instance. To configure Jenkins:

Connect to http://<your_server_public_DNS>:8080 from your browser. You will be able to access Jenkins through its management interface:

![image](https://user-images.githubusercontent.com/115881685/211005628-22ea9d87-5f70-4280-a4a3-985781ab1637.png)
As prompted, enter the password found in /var/lib/jenkins/secrets/initialAdminPassword.

Use the following command to display this password:

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

The Jenkins installation script directs you to the **Customize Jenkins page**. Click **Install suggested plugins**.

![jenkins 8](https://user-images.githubusercontent.com/115881685/211006367-90d8125a-c107-477b-a5f3-4d70c3f74dbe.JPG)

Once the installation is complete, the **Create First Admin User** will open. Enter your information, and then select **Save** and **Continue**.

![jenkins 9 create a new user](https://user-images.githubusercontent.com/115881685/211006652-e41e4410-fd1d-4795-9a79-561037b052e1.JPG)

Once this is done, you will be taken to the home page.
![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/9b2be65b-1252-4635-ba11-cf5475c56463)

## Step 4: Install cloudformation plugin.

In the jenkins dashboard, click **manage jenkins** and then click **manage plugin,** click **available**, search **cloudformation**, then select it, click **install without restart**, then **restart jenkins**


![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/25960567-8c52-459b-bfa8-ab968e7a6e09)

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/5ba88232-f5cd-44e1-b016-38f3f035fb58)



## Step 5: Run Cft using using Cft Plugin

Go back to the jenkins home page and select **new item** to create a job.

In the **enter an item name** box, type **cftdemo-jenkinsplugin**

Click **freestyle project** and enter **ok**

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/6ea2c554-3f36-44b2-8318-55e51ccbc642)

In the new page that has opened, scroll down down to **source code management**, click **Git**, go to your github account that has a [cloudformation-demo ](https://github.com/georgeonalo/cloudformation-demo)repository that has a [jenkinsfile](https://github.com/georgeonalo/cloudformation-demo/blob/main/Jenkinsfile) and a [simplests3cft.json](https://github.com/georgeonalo/cloudformation-demo/blob/main/simplests3cft.json) script

Copy the **Cloudformatiom-demo** url and paste it in the **Repository URL** box. Under **Credentials**, click **add jenkins**, provide your Github **user ID and password**, leave the other options blank.

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/e463a47c-08e5-4f13-be2f-9bb00f2306d0)

![jenkins 13](https://user-images.githubusercontent.com/115881685/211033774-96bbc1b6-e3aa-471c-bce5-c309ca1e1636.JPG)

Scroll down to **Build Environment** and click Create **AWS Cloud Formation stack**, enter **""simplests3cft.json""** into the **.json box** and enter **"jenkinscftplugin-s3"** into the **Stack name** field.


Go to the aws console and create an IAM user that only has access to **Cloudformation** and **S3**, we are doing this for security purposes. When you create the user, an access key and secret key will be generated, copy both and paste it under the **AWS access key** and **AWS secrete key** field  respectively in the jenkins job. Uncheck **automatically delete stack**

Click **save**

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/a93f7b2e-5f57-4b19-ac28-bc6f68ca06c1)

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/083ad78f-b7ea-4627-b324-15f9e35d6428)


Now in the next page that opens click **Build now** so that we can manually trigger the build job, we will automate this task later.

![jenkins 16](https://user-images.githubusercontent.com/115881685/211039138-629d75c5-bf05-4bf4-8e05-8b0a9f08e8f2.JPG)

Click **#1** beside the build job display in the bottom left hand corner of the page, click **console output** to see the system logs of the build job.

The system logs shows that Cloudformation stack is been created

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/f08551ba-d635-4418-8027-bfa2c61a35c0)

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/8bdf9909-c3d0-4529-a574-e20b5af68cdb)

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/bca84564-1095-48ef-a180-6f436b49bc6f)



To comfirm the if cft stack has been created go to cloudformation in the AWS console.

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/ffa5dd0b-ba80-483b-9d6c-8fde74b9874a)

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/824f57d7-acfc-4e6b-be7a-4787c106a725)


 ## Step 6: Install AWS CLI without keys using SSM Session Manager
 
 Go to the **AWS console** and search for **session manager**, once there, Click on **quick setup** and follow the instructions, once done, scroll down to **remote connect** under **features**, click it. Click **start session**, this reveals our **jenkins EC2 instance**, click **start session** again to open up a **shell**
 
 In the shell, run the following command:

 ```
 sudo yum update
 sudo yum install awscli
 ```

 
 Run "aws configure" to verify that awscli is installed correctly
 
 ![20](https://user-images.githubusercontent.com/115881685/211050349-a0b74d8c-5a61-4921-afad-bda7a39a1ba8.JPG)
 
 
## Step 7: jenkinsfile

[jenkinsfile](https://github.com/georgeonalo/cloudformation-demo/blob/main/Jenkinsfile) is a file which defines a pipeline and its stages, e.g **build, test and deploy**.

This particular [jenkinsfile](https://github.com/georgeonalo/cloudformation-demo/blob/main/Jenkinsfile) only has one stage, and within the file there is a powerful cli command that enable us to create cloudformation stack.

![jenkinsfile](https://user-images.githubusercontent.com/115881685/211053362-d42ed433-2d64-402a-9316-a8e8dba1380a.JPG)
![jenkinsfile2](https://user-images.githubusercontent.com/115881685/211053724-4c57118f-a08d-4702-af46-c95b786c7197.JPG)

Now in other to successfully create cloudformation stack, we need to grant our **Ec2 instance Role** **permission/policy to create cloudformation and S3 bucket**


![jenkins 20](https://user-images.githubusercontent.com/115881685/211054934-eed45148-b9e3-4b3f-8079-0a4e3003db99.JPG)

## Step 8: Create a jenkins job

Go to **jenkins dashboard**, click **New item**. in the **Enter an item name** field, type **"pipeline-s3-cft"**. click **pipeline** and hit **ok**


![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/c948b8b3-931d-46c1-8545-09947d669d69)

Scroll down to **pipeline** under **Advanced Project Options**, click the **Defination** field and select **pipeline script from SCM**, under **SCM**, select **Git**, copy the cloudformation url from your repository and paste it in the job, and then add **credentials**. **click save**.

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/4ec060e1-f4c0-4524-9706-449d9769f976)


Click on **Build Now**, so that the job can be manually triggered.

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/5f50db4e-745b-4341-8d25-5ee02746221b)

We can see **submit stack** which is the name of the stage comming from **jenkinsfile.** Click **#1** and click **console output** to reveal the **system logs**

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/a7341b3c-a3e1-4419-b14b-7803402781d3)

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/f1484350-f98d-44ef-a4b2-2fb8bf084f8d)


Go to cloudformation in the AWS console to see the s3bucket stack.

![image](https://github.com/georgeonalo/Run-Infra-as-Code-with-Jenkins/assets/115881685/2c265be3-21c9-4058-8bd3-803c4a46a58b)

This brings us to the end of the project.

**Note:** take down all the resources created in the AWS consol to avoid charges
















    
    









