# Setting up Highly Available and Self-Healing Jenkins Master on AWS

The core components for setting up a minimal HA Jenkins master in AWS are:

* **An AWS region**\
  Such as us-east-1, eu-west-, etc., where you would like to provision your Jenkins master. Ideally, a region that is closest to your end users to minimize any latency issues.
* **A VPC (Virtual Private Cloud)**\
  It is a logically isolated section of AWS Cloud. Launching inside a VPC not only allows you to keep your Jenkins resources separate from other resources you might be running, but also provides the ability to have control over static instance IP addresses.
* **EFS (Elastic File System)**\
  EFS is a scalable, elastic, cloud-native NFS file system that can be mounted on up to 1000 servers in a specific region.
* **An Auto Scaling group with a launch configuration**\
  An Auto Scaling group which will maintain Jenkins master availability based on certain checks. If the checks fail, the Jenkins master will automatically be replaced with a new one. The launch configuration is essentially a template for spinning up a new Jenkins master.
* **Elastic Load Balancer**\
  Preferably an application load balancer, so that your end users can reach your Jenkins master using the ALB DNS name without having to worry about the actual IP address of the backend Jenkins master that they are pointing to.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/id3lqfg0gkoo-LFS267\_CourseGraphics-03.png)

**Figure: Jenkins High Availability and Self-Healing Architecture**

&#x20;

Let’s take a look at what it takes to setup this architecture in AWS.
