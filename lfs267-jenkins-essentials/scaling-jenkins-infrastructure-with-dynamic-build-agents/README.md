# SCALING JENKINS INFRASTRUCTURE WITH DYNAMIC BUILD AGENTS

As your team grows, the number of builds supported by your Jenkins master will grow as well. Although you may be able to support short-term growth by virtually scaling your Jenkins master or adding more memory and CPUs, etc. However, there is a limit to virtual scaling. Luckily, Jenkins allows us to deploy a horizontal scaling strategy. By using this approach, we can add external build agents that perform the task of running builds; hence, scaling our CI/CD infrastructure to meet our growing needs.

In this chapter, we will explore how to scale Jenkins by implementing a distributed build architecture.





By the end of this section, you should be able to:

* Describe Jenkins distributed build architecture.
* Explain the various build agent types in Jenkins.
* Create build agents with a Jenkins Docker plugin.
* Create build agents with a Jenkins Kubernetes plugin.
* Create build agents with an EC2 plugin.

