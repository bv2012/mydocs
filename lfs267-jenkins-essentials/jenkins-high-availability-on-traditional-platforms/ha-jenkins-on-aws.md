# HA Jenkins on AWS

## Virtual Private Cloud

***

A VPC is recommended as it provides a logically isolated network by allowing full control over static instance IP addresses.

The first step is to select an AWS region that is closest to your end users. Create a VPC (Virtual Private Cloud) with your CIDR block. Within your VPC, you will need to create public subnets across multiple availability zones. At minimum, you must configure two subnets. In case of a complete loss of an availability zone, you can quickly bring up a Jenkins master in another one.

Here is an illustration of creating two subnets in a VPC named example.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/8zog3cd3qucf-create-teo-public-subnets.png)

**Figure: VPC with Two Public Subnets**

&#x20;

Next, you will need to allow communication between your VPC and the Internet. To do this, you will need to create an Internet Gateway. When you create a VPC, your default (main) route table is restrictive and does not allow any routes out to the Internet. You can modify it; however, it is a good practice to leave the default route as is, and create a new custom route which will allow a route to the Internet via the Internet Gateway.

Your custom route table must include a new route as follows:

Destination - 0.0.0.0.0

Target - name of your Internet gateway

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/yw3i6g5mm9bq-route-through-IGW.png)

**Figure: New Route via Internet Gateway**

&#x20;

Associate your public subnets to the newly created route table.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/2pc5c37kmnrh-associate-public-subnets-to-route-table.png)

**Figure: Associate Public Subnets**

&#x20;

Next, let's proceed to creating the Elastic File System.

## Create Elastic File System (EFS)

***

All of the Jenkins data resides under the **JENKINS\_HOME** directory. By default, this is stored locally on the server. However, for HA, we need this to be a shared storage. EFS (Elastic File System) can be simultaneously mounted on 1000 servers in a specific region. It is worth noting that EFS is probably the most expensive storage option of all. You can use cheaper alternatives such as EBS; however, that would require some automation such as creating snapshots of the EBS volume and mounting onto another Jenkins server.

Creating an Elastic File System involves the following steps:

* Select the VPC where you would like to host your Jenkins server.
* Create mount targets in each of your VPC's Availability Zones so that your Jenkins instances across your VPC can access the file system.
* Create a security group to associate with the mount target. This is to ensure that Amazon EFS allows traffic from your Jenkins instances. The security group must allow inbound access for the TCP protocol on port 2049 for NFS from all EC2 instances on which you want to mount the file system. Each EC2 instance that mounts the file system must have a security group that allows outbound access to the mount target on TCP port 2049.
* Add tags if you need them, and choose performance mode. General Purpose performance mode is strongly recommended as it is optimized for applications where up to several thousands of instances are accessing the file system. Although, there is a small tradeoff of slightly higher latencies for file operations.

Here is an illustration of creating an EFS and mount targets in two availability zones, us-east-1b and us-east-1c. These are the availability zones where we created our public subnets under the _Create VPC_ section.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/ny24yjork86u-create-efs.png)

**Figure: Create EFS and Mount Targets**

## Create an Amazon Machine Image

***

Before you can create an image, you will first need to set up a Jenkins master. Here, we list the steps to create one.

* Launch an instance into your VPC from an existing AMI depending upon your operating system. For our illustration, we will use Ubuntu 18.04 LTS.
* Create a security group that allows SSH (Port 22) and HTTP (default Jenkins port 8080)
* Log in to the newly created instance and follow the installation steps:\
  \
  Update **apt** package index:\
  **$ sudo apt-get update**\
  \
  Install **nfs-commons**. This package provides NFS functionality:\
  **$ sudo apt-get install nfs-common**\
  \
  Create a folder for mounting your **$JENKINS\_HOME** folder:\
  **$ sudo mkdir –p /mnt/JENKINS\_HOME**\
  \
  Mount the Amazon EFS file system to this newly created directory:\
  **$ sudo mount -t nfs4 -o vers=4.1 $(curl -s \ htt‌p://169.254.169.254/latest/meta-data/placement/availability-zone).\<file-system-id>.efs.\<aws-region>.amazonaws.com:/ /mnt/JENKINS\_HOME**\
  \
  Be sure to replace **\<file-system-id>** and **\<aws-region>** placeholders with your EFS file system ID and AWS Region name.\
  \
  Java is a prerequisite for Jenkins. Install JDK 11:\
  **$ sudo apt-get install openjdk-11-jdk**\
  \
  Install Jenkins:\
  **$ sudo wget -q -O - htt‌ps://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -**\
  **$ echo "deb htt‌ps://pkg.jenkins.io/debian-stable binary/" | sudo tee -a /etc/apt/sources.list**\
  **$ sudo apt-get update**\
  **$ sudo apt-get install jenkins** \
  \
  Stop Jenkins:\
  **$ sudo service jenkins stop**\
  \
  Make sure that the newly created mount is owned by Jenkins:\
  **$ sudo chown jenkins:jenkins /mnt/JENKINS\_HOME**\
  \
  By default, **JENKINS\_HOME** is set to **/var/lib/jenkins**. Update Jenkins configuration to point to your EFS mount:\
  **$ sudo vi /etc/default/jenkins**\
  \
  Replace the value of **JENKINS\_HOME** with **/mnt/JENKINS\_HOME**.\
  \
  Launch Jenkins:\
  **$ sudo service jenkins start**\
  \
  Make sure to add fstab entry of NFS mounts as follows:\
  **$ sudo vi /etc/fstab**\
  \
  Next, add the line:\
  **\<mount-target-DNS>:/ /mnt/JENKINS\_HOME nfs defaults,vers=4.1 0 0**\
  \
  Replace **\<mount-target-DNS>** with the DNS name of your Amazon EFS server.

Login to the Jenkins UI and complete the post-installation steps. You now have a functional Jenkins. You can create an AMI (Amazon Machine Image) of your Jenkins instance. This AMI will be used to create a launch configuration which is essentially a template for the Auto Scaling group.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/h25bypai9bf8-jenkins-ami.png)

**Figure: Jenkins AMI**

****

&#x20;

At this point in time, you can terminate your Jenkins instance.

## Create a Launch Configuration

***

Creating a launch configuration is rather straightforward.

*   Pick the custom Jenkins AMI you just created.

    \


    \
    \
    **Figure: Create Launch Configuration**\
    \

* Choose an instance type as required.
*   Select the security group you created in the previous section. Your security group should allow ports 22 and 8080.\
    \
    ![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/r7joqs9u51m2-security-group-for-lc.png)\


    \
    **Figure: Security Group for Launch Configuration**
*   Select your public SSH key. If you don’t have one, create a new keypair.\




    \
    **Figure: Select Your SSH Public Key**

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/c77fng0tv95c-public-ssh-key-lc.png)

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/w4r54hqtb8yu-create-lc.png)

Now that we have a launch configuration, we are ready to create a target group.

## Create Target Group

***

As part of creating a target group, you will need to monitor port 8080 (default Jenkins port)...

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/revpymssbwgu-create-target-group.png)

**Figure: Create a Target Group**

&#x20;

...and check for response code 403 while performing health checks.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/puo7d6xpiise-health-checks.png)

**Figure: Configure Health Check for Target Group**

## Elastic Load Balancer

***

You should set up a reverse proxy so that your end users can access your Jenkins master via the reverse proxy without regard to which Jenkins master serves their request. An easiest way to set this up is by using an interfacing AWS ALB (Application Load Balancer) pointing to your auto-scaling group. Note that there will be cost implications. Alternatives for this would be HAProxy, NGINX, etc.

To create an ALB,

* Leave Scheme and IP address type at their default values.
* Configure a listener for your ALB. The listener should accept HTTP traffic on port 80.
* For Availability Zones, select your custom VPC. Select all the Availability Zones and their corresponding public subnets.\
  \
  \
  \
  **Figure: Create an Internet-Facing ALB**\
  \

* Configure a security group for your load balancer. It should allow access on port 80.&#x20;
* Configure listener rules to forward the request to your target group.\
  \
  ![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/zakn8pd7ibi8-alb-route-to-tg.png)\
  \
  **Figure: Configure Routing for Target Group**\
  \


![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/1ik50drr282p-create-ALB.png)

You are now ready to create the Auto Scaling group.

## Create an Auto Scaling Group

***

To create an Auto Scaling group, select the launch configuration that you created earlier.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/35wk6ukxwrq9-create-asg-from-lc.png)

**Figure: Create an Auto Scaling Group**

&#x20;

Under _Network_, select your VPC and subnets. You do not need to worry about any scaling policies as the group always stays at its initial size.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/52el5z44rikc-network-subnets-asg.png)

**Figure: Select Subnets**

&#x20;

Next, enable load balancing and choose your ALB target group. For health checks, be sure to select ELB.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/ivyzllupiomu-configure-alb-asg.png)

**Figure: Configuring Health Checks for ASG**

## Create an Auto Scaling Group (Cont.)

***

Set the Group Size to 1. This is due to the fact that only a single Jenkins master can be up and running at any point in time.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/63bio4zadvxn-asg-group-size.png)

**Figure: Group Size for Auto Scaling**

&#x20;

Once this is configured, auto-scaling takes effect and starts updating the capacity by creating one Jenkins master.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/r6q3e0smo0u3-asg-in-action.png)

**Figure: Auto Scaling in Action**

&#x20;

You should be able to use the ALB DNS name to get to Jenkins UI.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/uhhc278k3lj2-alb-dns-name.png)

**Figure: Retrieve ALB DNS Name**

## Additional Considerations

***

Enable Monitoring: It is always a good idea to proactively monitor your Jenkins master for CPU, disk utilization, etc. You can set thresholds and have alerts sent to you when events like low disk space or high CPU utilization cross a threshold that you define.

You can use IaC tools such as Terraform to store this as code in version control and leverage associated benefits - traceability, auditability, and reproducibility.
