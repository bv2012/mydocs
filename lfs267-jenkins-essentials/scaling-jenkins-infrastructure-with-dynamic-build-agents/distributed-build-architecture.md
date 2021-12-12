# Distributed Build Architecture

Distributed build is a Jenkins usability best practice. It refers to the practice of executing builds on nodes other than the Jenkins master. Distributed builds allow the use of heterogeneous nodes to run builds.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/kxliksgx04h8-LFS267\_CourseGraphics-05.png)

**Figure: Jenkins Distributed Architecture**

&#x20;

The main reasons for using distributed architecture include:

* To preserve security on the Jenkins master.
* To make the Jenkins instance more scalable (by adding more agents when you need more resources).
* To define agents that run on different operating systems and CPU architectures.

Let's explore how to implement distributed build architecture in Jenkins.

## Jenkins Build Agent Types

***

Jenkins build agents can be divided into two categories:

* Static\
  Static build agents are pre-provisioned nodes attached to your Jenkins master. They are prepared and configured in advance. Whenever new builds are triggered, they are waiting to take up the build load. This approach works well if your build usage is consistent and you have access to a data center to host your own nodes. However, this means that you will have to maintain these nodes by applying patches and performing upgrades, etc. Also, if the number of builds suddenly goes up, you may not be able to provision new nodes quickly to address high demand.
* Ephemeral\
  Ephemeral build agents are provisioned on demand. In simple terms, Jenkins will spawn a build agent to run the build workload as soon as a new build job is submitted. This approach makes perfect sense for a pay-as-you-go cloud philosophy and works for many teams. Please note that sometimes it may not be possible for you to predict your build infrastructure usage. Inordinately pre-provisioned hardware means paying for unused resources. Lack of available infrastructure, on the other hand, means loss of productivity. Using ephemeral build agents is cloud infrastructure best practice.

In the rest of this chapter, we will further explore setting up ephemeral build agents with various Jenkins plugins.



## Ephemeral Build Agents with Docker Plugin

***

Jenkins [Docker plugin](https://plugins.jenkins.io/docker-plugin/) allows to dynamically provision Docker image-based build agents, run a single build and then tear down the agent. As part of the set-up, you will need a **Docker\_Host**, a node with Docker binaries installed. **Docker\_Host** needs to be configured as a _cloud_ in Jenkins. Jenkins master will use this host to launch build agents as containers. Here is a look at the high level architecture:

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/3sq6cx7ldb3s-LFS267\_CourseGraphics-07.png)

**Figure: Jenkins Docker Plugin Architecture**

&#x20;

The Jenkins Docker plugin pulls down a build agent image from the Docker registry and uses that image to create a build agent inside the **Docker\_Host**. The build agents will be created on-demand based on the needs of the system. After a build is finished, the agent will be torn down.&#x20;

Let's take a closer look at the components required for setting up a Jenkins Docker plugin. We will need the following:&#x20;

* Docker host&#x20;
* Jenkins agent Docker image&#x20;
* Jenkins Docker plugin&#x20;
* Docker cloud configuration on Jenkins master

## Ephemeral Build Agents with Docker Plugin: Docker Host

***

This is the node where build agents as Docker containers will be launched. This node should already have Docker binaries installed on it. Also, please ensure the Docker daemon is properly configured to listen to Docker engine API requests. Please refer to Docker Documentation [_“Configure and Troubleshoot the Docker Daemon”_](https://docs.docker.com/config/daemon/) for more information. Your Jenkins master can also be used as a Docker host. However, it's not recommended for production. Ideally, you should have a separate node set up as a Docker host. A Docker container can also be used as a Docket host. However, it's not a recommended practice. We recommend using a VM or a bare-metal node as Docker host.

## Ephemeral Build Agents with Docker Plugin: Jenkins Agent Docker Image

***

You need a Docker image of the build agent. This image will be used to automatically provision container based build agents. The Jenkins community has created two such images:

* [Docker ssh agent image](https://hub.docker.com/r/jenkins/ssh-agent/)
* [Docker inbound Jenkins agents](https://hub.docker.com/r/jenkins/inbound-agent/) (previously known as JNLP agents)

You can choose the one that suits your needs.





[\
](https://trainingportal.linuxfoundation.org/learn/course/jenkins-essentials-lfs267/scaling-jenkins-infrastructure-with-dynamic-build-agents/scaling-jenkins-infrastructure-with-dynamic-build-agents?page=3)

