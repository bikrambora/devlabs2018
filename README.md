**[START THE LAB FROM HERE]{.underline}**

**Go to Cloud9 on the aws web console**

**On top right corner - Switch to US-EAST-1**

**[Lab Setup Steps]{.underline}**

**Launch a Cloud9 environment or Pick ECSLabCloud9**

This is your IDE in the cloud.

You can do all of these steps in a local IDE as well

**Install Docker**

\$ sudo yum update -y

\$ sudo yum install -y docker

\$ sudo service docker start

**Give Docker super user permissions**

\$ sudo usermod -a -G docker ec2-user

**\
**

**Open the Terminal**

**cd to folder**

\$ cd \~/environment

**Get Website Files downloaded into cloud9**

\$ curl -O
https://s3.amazonaws.com/dev-lab-website-bucket/DockerStaticSite-master.zip

**Unzip the downloaded Files**

\$ unzip -o DockerStaticSite-master.zip

**cd into the unzipped folder**

\$ cd DockerStaticSite-master

**Edit the index.html file and put in something Recognizable **

\<h1\>Hello World! Put something Here \</h1\>

\<p\>This is a simple static website being served from Nginx running
inside a Docker container!\</p\>

**(The next person will read what you put in so please be respectful)**

**Save your edited HTML File**

**\
**

**Build and Run your Docker Container Locally**

**Build Image from DockerFile **

\$ docker build -t staticsite:1.0 .

**Run Container from Image**

\$ docker run -itd \--name mycontainer \--publish 8080:80 staticsite:1.0

**Test if Container is Running**

\$ curl http://localhost:8080

**If u get a html page as a response, then the container is running
successfully!**

\<h1\>Hello World! Put something Here \</h1\>

\<p\>This is a simple static website being served from Nginx running
inside a Docker container!\</p\>

**\
**

**Push your container Image to the Cloud**

**Now that your containerized application is running locally, let's push
your docker image to a ECR repository in the cloud**

**\*ECR -- Elastic Container Repository**

1)  **Run this command to get your ECR login credentials**

aws ecr get-login \--no-include-email \--region us-east-1

Note: If you receive an \"Unknown options: \--no-include-email\" error
when using the AWS CLI, ensure that you have the latest version
installed. [[Learn
more]{.underline}](http://docs.aws.amazon.com/cli/latest/userguide/installing.html)

**2) Step 1 will return back a Big response**

**Copy that entire response and Paste in the terminal and run it to
login**

**3) Build your Docker image using the following command. **

docker build -t lab-image-repo .

**4) After the build completes, tag your image so you can push the image
to this repository:**

docker tag lab-image-repo:latest
894812553491.dkr.ecr.us-east-1.amazonaws.com/lab-image-repo:latest

**5) Run the following command to push this image to your newly created
AWS repository: **

docker push
894812553491.dkr.ecr.us-east-1.amazonaws.com/lab-image-repo:latest

**Once the push is completed your docker image will be pushed to an ECR
image repository on AWS**

**\
An ECS cluster is already provisioned for you. The cluster is currently
running a previous version of the same application. You will now Deploy
the your new Application Version to the Service**

**Update the ECS task definition to use the image that you just Pushed**

1.  Navigate to the **Elastic Container Service** Dashboard.

2.  In the navigation menu on the **left**, click **Task Definitions.**

3.  Select the **simplewebserver**

4.  Click **Create new revision**

5.  Scroll to the bottom of the page, then click **Create**

This will create a new version of the Task. Note the Version Number. The
new version will use the latest container image. i.e. the image that you
just pushed.

**Update the ECS Service to deploy the new version of the task**

6.  In the left navigation pane, click **Clusters**.

7.  In the **Clusters** window, click default \>.

This is the cluster that your service resides in.

8.  On the **Services** tab, check mywebservice

9.  Click **Update** then configure:

**Revision:** select the *latest*Click **Next step**

43. On **Step 2**, click **Next step**

44. On **Step 3**, click **Next step**

45. On **Step 4**, click **Update Service**

This will deploy a new version of the application.

46. On the **Launch Status** page, click **View Service**

47. On the **Service: myService** page, click the **Events** tab.

48. Monitor the process of draining connections and stopping tasks till
    > the service reaches a steady state.

You may need to click the **Refresh** button to see the new events.

49. Click the **Deployments** tab.

<!-- -->

50. Return to your web browser and refresh the Load Balancer\'s URL (DNS
    > name) to see the new version of the app

![](media/image1.png){width="6.207692475940507in"
height="2.3391305774278215in"}

Congratulations! Your Containerized web application is Now Running in
the cloud

**[Lab Setup Steps]{.underline}**

**On top right corner - Switch to US-EAST-1**

**Launch a Cloud9 instance or Pick ECSLabCloud9**

This is your IDE in the cloud.

You can do all of these steps in a local IDE as well

**Install Docker**

\$ sudo yum update -y

\$ sudo yum install -y docker

\$ sudo service docker start

**Give Docker super user permissions**

\$ sudo usermod -a -G docker ec2-user

**[Lab Reset Steps]{.underline} *Skip if first Run***

**Delete all local Docker Images **

\$ docker rmi \$(docker images -a -q) -f

**Stop all containers running locally **

\$ docker stop \$(docker ps -a -q)

**Delete all local stopped containers **

\$ docker rm \$(docker ps -a -q)

**[\
]{.underline}**

**[Lab Reset Steps]{.underline} **

**cd to correct directory**

\$ cd \~/environment

**Stop all containers running locally **

\$ docker stop \$(docker ps -a -q)

**Delete all local stopped containers **

\$ docker rm \$(docker ps -a -q)

**Delete all local Docker Images **

\$ docker rmi \$(docker images -a -q) -f
