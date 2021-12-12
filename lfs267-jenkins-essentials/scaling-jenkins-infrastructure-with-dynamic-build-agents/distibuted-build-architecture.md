# Distibuted Build Architecture

## Ephemeral Build Agents with Docker Plugin: Jenkins Docker Plugin

***

Install Jenkins Docker Plugin via _Manage Jenkins_ > _Manage Plugins_ page.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/ufk1my8bq9cv-image10.png)

**Figure: Install Jenkins Docker Plugin**

&#x20;

Please make sure to restart your Jenkins instance after installing the plugin.

## Ephemeral Build Agents with Docker Plugin: Docker Cloud Configuration on Jenkins Master

***

You will need to configure Docker as cloud in Jenkins.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/6fq9v0s9ptea-image3.png)

**Figure: Configure Docker as Cloud**

&#x20;

Next, you have to provide “Docker Cloud details...”. The first important piece of configuration is the Docker Host URI.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/2r15kdp4i1mm-image7.png)

**Figure: Docker Cloud Details**

&#x20;

The Docker daemon typically listens on ports 2375/2376 for tcp traffic. Port 2375 is used for non-SSL data whereas port 2376 is used for SSL data. In a production system, you should use an SSL port. This may require the additional step of setting up SSL certs.

Given that you have entered the correct URI address, you can click on the “Test Connection” button and Jenkins will print the Docker version running on the Docker Host on the screen. If you see the Docker version, it would mean your Docker Jenkins plugin connectivity is working.

Next, you have to configure the “Docker Agent template”. This tells Jenkins what image to use for creating container based build agents.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/691i1xsk0axz-image11.png)

**Figure: Docker Agent Template**

&#x20;

Start with specifying “Labels”. This is how Jenkins will restrict the use of this agent. Only build jobs specified with this label will run on this agent. Next, you will have to specify Docker image for creating the build agent. You can also use a custom image here. Finally, you have to specify the “Connect method”. You will have three options to choose from. The default option of “Attach Docker container” generally works well. At this point, your configuration should be done. Just remember to save your changes and run a sample test build to test your setup. If all goes well, you should see something similar on your Jenkins dashboard.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/bqmzpdyllr14-image12.png)

**Figure: Docker Build Run**

&#x20;

Jenkins will launch a temporary build agent named something like "docker-agent-\*". Your build will be executed on this agent. As soon as your build is done, this agent will disappear.



## Ephemeral Build Agents with Jenkins Kubernetes Plugin

***

Jenkins lets you provision on-demand build agents in a Kubernetes cluster. Like the Jenkins Docker plugin, the Jenkins [Kubernetes plugin](https://plugins.jenkins.io/kubernetes/) creates a dynamic build agent and tears it down after the build is done. Here is how the high level architecture will look like.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/aj01o3bf11io-LFS267\_CourseGraphics-06.png)

**Figure: Jenkins Kubernetes Plugin Architecture**

&#x20;

The Kubernetes cluster will be added as a cloud to Jenkins master. The Jenkins Kubernetes plugin will launch builds on build agents created in the pods. Build agents will be dynamic and last as long the build is running. Let's have a detailed look at Jenkins Kubernetes plugin requirements.

* Kubernetes cluster
* Jenkins agent Docker image
* Jenkins Kubernetes plugin
* Kubernetes cloud configuration on Jenkins master

## Ephemeral Build Agents with Jenkins Kubernetes Plugin: Kubernetes Cluster

***

Jenkins Kubernetes plugin requires Kubernetes version 1.14 or later. You will need a functional cluster running on-premise or on a public cloud. It's not required to run the Jenkins master in the Kubernetes cluster. All you need is the connectivity between the Jenkins master and the Kubernetes cluster. Setting up a Kubernetes cluster can be a non-trivial task. Various public cloud vendors like AWS, Azure, and GCP provide managed environments that are fairly easy to set up and scale. However, the ease in setup comes at a cost. You will need to exercise judgement and choose what makes sense.

## Ephemeral Build Agents with Jenkins Kubernetes Plugin: Jenkins Agent Docker Image

***

Just like Jenkins Docker plugin, the Jenkins Kubernetes plugin will need a Docker image to launch a build agent. The Jenkins inbound agent (formerly known as JNLP agent) image is used for this purpose. The build agent connects with the master as soon as it is launched. The build agent container is destroyed as soon as the build is done.\


## Ephemeral Build Agents with Jenkins Kubernetes Plugin: Jenkins Kubernetes Plugin

***

The Jenkins Kubernetes plugin is not part of the core installation. You will have to install it separately. It can be installed from the _Manage Jenkins_ > _Manage Plugins_ page.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/epqkidtxla26-image14.png)

**Figure: Install Kubernetes Plugin**

[\
](https://trainingportal.linuxfoundation.org/learn/course/jenkins-essentials-lfs267/scaling-jenkins-infrastructure-with-dynamic-build-agents/scaling-jenkins-infrastructure-with-dynamic-build-agents?page=10)Ephemeral Build Agents with Jenkins Kubernetes Plugin: Kubernetes Cloud Configuration on Jenkins Master

***

You will have to add Kubernetes cluster as a cloud on the Jenkins master. Let's start with configuring the cloud.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/7dexdh7i6et4-image5.png)

**Figure: Configure Kubernetes Clouds**

&#x20;

If you wish, you can add multiple Kubernetes clouds. Each cloud should correspond to a separate Kubernetes cluster.

Next, let's configure “Kubernetes Cloud details...”. Below are some of the important details of this configuration.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/sc1w50190ah2-image8.png)

**Figure: Kubernetes Cloud Details**

&#x20;

**$ kubectl cluster-info**

**Kubernetes master is running at htt‌ps://192.168.64.52:8443**\
**KubeDNS is running at htt‌ps://192.168.64.52:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy**

**To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.**

Please keep in mind, your Kubernetes cluster should be ready before you run the above command. Next, we need to specify a namespace in which Jenkins can create/delete/modify pods. Jenkins will need extra privileges for this. It is recommended that rather than granting extra privileges in the default namespace, we create a separate namespace and provide the required privileges to the Jenkins Kubernetes plugin. We have covered this in greater detail in the _“Running Jenkins on Kubernetes”_ chapter.

You can test Jenkins master to Kubernetes cluster connectivity by clicking on the “Test Connection” button. You should see the Kubernetes version of the cluster printed on the screen. Finally, we need to specify the URL of Jenkins master. This will be used by build agents to connect with the master.

Next, let's configure a Pod template. This template will be used to create pods to run build agents in the cluster.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/1up7v1blviue-image6.png)

**Figure: Pod Template Configuration**

&#x20;

The Pod template name is used to identify the build agents created on this pod. Labels are used to restrict the projects built on this pod. We also need to configure the Container template that will be used to create the build containers for our builds.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/5iem9m4xd6c6-image2.png)

**Figure: Kubernetes Container Template**

&#x20;

The above template will be used to create build agents at the run time.

After this, you should be done with the configuration setup. Make sure you save your changes. Now is the time to do a test build to see if you can use the Jenkins Kubernetes plugin to provision build agents in your Kubernetes cluster. Just create a simple Freestyle job and run it. If everything is configured correctly, you will see an output similar to the following:

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/2x6xq2ug60l3-image13.png)

**Figure: Kubernetes Plugin Build**

&#x20;

Jenkins will launch a build agent named like "kube-pod-\*" to execute your builds. The temporary agent will disappear after the build is done.

## Ephemeral Build Agents with EC2 Plugin

***

The [Amazon EC2 plugin](https://plugins.jenkins.io/ec2/) lets Jenkins use AWS EC2 instances as build agents.

These agents are dynamically created inside an AWS network and terminated when they are not needed. Here is a look at the high level architecture.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/msqakjrle9v8-LFS267\_CourseGraphics-04.png)

**Figure: Dynamic Builds Agents on AWS**

&#x20;

Let’s take a closer look at the components required for setting up Jenkins EC2 plugin. We will need the following:

* Amazon Machine Image (AMI) for Jenkins build agents
* Security Group
* EC2 plugin
* EC2 cloud configuration on Jenkins master
* You can select an existing AMI for the OS of your choice, and include JDK install as part of the user data script that runs during instance launch.\
  \
  User data script for Ubuntu:\
  \
  **#! /bin/bash**\
  **sudo apt-get update -y**\
  **sudo apt-get -y install openjdk-11-jdk**
* You can create a custom AMI with a pre-installed JDK.

JDK is the only package that needs to be installed on Jenkins Agents. You have two options for AMI.

***

## Ephemeral Build Agents with EC2 Plugin: Amazon Machine Image (AMI)



JDK is the only package that needs to be installed on Jenkins Agents. You have two options for AMI.

* You can select an existing AMI for the OS of your choice, and include JDK install as part of the user data script that runs during instance launch.\
  \
  User data script for Ubuntu:\
  \
  **#! /bin/bash**\
  **sudo apt-get update -y**\
  **sudo apt-get -y install openjdk-11-jdk**
* You can create a custom AMI with a pre-installed JDK.
*

## Ephemeral Build Agents with EC2 Plugin: Security Group

***

You will need to create a security group that allows port 22 in the region where you would like to dynamically provision your Jenkins agents.

Ephemeral Build Agents with EC2 Plugin: EC2 Plugin

***

Install Jenkins EC2 Plugin via _Manage Jenkins_ > _Manage Plugins_ page.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/ulx58ttbe7bz-install-plugin.png)

**Figure: Install EC2 Plugin**

&#x20;

Please make sure to restart your Jenkins instance after installing the plugin.

## Ephemeral Build Agents with EC2 Plugin: EC2 Cloud Configuration on Jenkins Master

***

You will need to configure EC2 as cloud in Jenkins. To configure the new Amazon EC2 cloud, go to **YOUR\_JENKINS\_URL/configureClouds**. Click “Add a new cloud” > Amazon EC2.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/wnx8odwp7pjr-add-ec2-cloud.png)

**Figure: Add Amazon EC2 Cloud**

&#x20;

On the next page:

* Enter a name for your EC2 cloud.
* Create credentials to connect to your AWS account and select these credentials from the credentials dropdown.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/c2s2za80yx6m-aws-credentials.png)

**Figure: Create AWS Credentials**

&#x20;

Select the region (us-east-1, eu-west-1, etc.) where you want to provision new EC2 Jenkins agents. Enter your EC2 private key. The corresponding public key must exist in your AWS region. Click “Test Connection” to verify that you are able to authenticate to your AWS account.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/8dkbblk9geus-configure-new-aws-cloud.png)

**Figure: Configure AWS Cloud**

&#x20;

Next, we will need to configure the EC2 instance details for our dynamic agents.

* Select the Jenkins Agent AMI for AWS account (click “Check AMI” to validate that the AMI exists).
* Select instance type from the dropdown.
* Enter security group ID. This is the security group which allows port 22.
* Enter Remote FS root. This should be specified as an absolute path on the Agent.
* Enter remote user. This is specific to OS. For example, “ubuntu” for Ubuntu OS, “ec2-user” for RHEL OS, etc.
* Choose AMI type.
* Enter the default remote SSH port. By default, the SSH port is 22.
* Enter one more label separated by a space. Jenkins jobs use labels to restrict where their builds are run.
* Enter idle termination timeout in minutes. By default, instances are terminated when the idle timeout period expires. Instead, if you want instances to just be stopped, check the toggle for Stop/Disconnect on Idle Timeout in the Advanced properties of the AMI configuration.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/5u5rdivanj7l-min-config-aws-cloud.png)

**Figure: Configure AMI**

****

Next, you can configure some additional settings such as:

* Instance Cap\
  Enter total number of running instances launched from this AMI and associated configurations.
* Connection Strategy\
  Choose whether you want to use Public IP, Public DNS, Private IP, etc.
* Select Host Key Verification Strategy\
  To prevent man-in-the-middle attacks, it is strongly recommended that you select check-new-hard strategy. Click on the ![](https://img.icons8.com/flat\_round/64/000000/question-mark.png) icon adjacent to it to learn more about the various strategies.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/mafyf1xq1e4o-additional-cofniguration.png)

**Figure: Additional Configuration**

&#x20;

Click Save to save the configuration settings.

The next step is to configure a Jenkins job. For Pipeline jobs, you can set a label as shown below.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/8ied23uuyvis-pipeline-job.png)

**Figure: Set Agent Label for Pipeline Job**

&#x20;

You should be able to view the console log and confirm where your Pipeline job was built.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/i5tjyli4d6fb-console-log-pipeline-job.png)

**Figure: Console Output for the Pipeline Job**

&#x20;

Jenkins EC2 plugin will provision a VM to run your build. If you prefer, you can keep around the provisioned VM for a bit to run successive builds or you can tear it down immediately after your build is done.[\
](https://trainingportal.linuxfoundation.org/learn/course/jenkins-essentials-lfs267/scaling-jenkins-infrastructure-with-dynamic-build-agents/scaling-jenkins-infrastructure-with-dynamic-build-agents?page=13)
