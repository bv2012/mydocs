# JENKINS HIGH AVAILABILITY ON TRADITIONAL PLATFORMS

Jenkins is a critical piece of CI/CD infrastructure. It is important to ensure that it is highly available and fault-tolerant. In the previous chapter, we discussed setting Jenkins in Kubernetes. We saw how Kubernetes lets you achieve high availability. However, if you are using a traditional platform, VM-based Jenkins installation, then there are some additional steps that you have to take to achieve high availability.&#x20;

In this chapter, we will go over how to set up a highly available and self-healing Jenkins architecture on a traditional VM-based platform.

## Need for Jenkins HA

***

Jenkins is a critical piece of your CI/CD infrastructure. It seamlessly lets you orchestrate all the stages of your CI/CD pipelines. It retains information about all your build runs. It also provides you with a single dashboard that gives a complete overview of your software delivery process. Any potential downtime with your Jenkins server can impact your software delivery, essentially adversely impacting your release milestones.

Your Jenkins server needs to be resilient to failures and should be highly available. Your RTO (Recovery Time Objective) should be minimal as you want to quickly get back up and running. Your target RPO (Recovery Point Objective) needs to be very high as you do not want to lose any data.

Next, let’s take a look at setting up a highly available Jenkins master on-premise.

