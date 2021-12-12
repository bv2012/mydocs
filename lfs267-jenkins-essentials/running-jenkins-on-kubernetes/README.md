# Running Jenkins on Kubernetes



## Requirements

***

Jenkins installation on Kubernetes has the following minimum requirements:

* On your local computer or a bastion host:\
  \- Kubernetes client version 1.14 or greater installed and configured with kubectl
* A working Kubernetes cluster running version 1.14 or greater (select an actively supported version):\
  \- Kubernetes nodes in multi-availability zones\
  \- Network access to Docker Hub
* The NGINX Ingress Controller installed in the cluster:\
  \- A load balancer/proxy configured and pointing to the NGINX Ingress Controller\
  \- A DNS record that points to the load balancer/proxy\
  \- TLS certificates installed
* A namespace created in the cluster with Role Based Access Control (RBAC) enabled.
* Kubernetes cluster default storage class defined and ready to use.

## Jenkins on Kubernetes Architecture

***

A typical Jenkins set up on Kubernetes would require a Kubernetes master and one worker node. However, for a production server, there should be more than one Kubernetes master installed in different availability zones for redundancy purposes. The following figure depicts Jenkins on Kubernetes architecture.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/i0nbut7yvel3-LFS267\_CourseGraphics\_V2.png)

**Figure: Jenkins on Kubernetes Architecture**

&#x20;

The Kubernetes master runs the API server, controller manager, and scheduler processes. These processes are responsible for managing the state and health of the cluster. The master node also hosts Kubernetes resources created by Jenkins installation. The Jenkins ingress routes user requests to the Jenkins master running inside the cluster. Ingress is the NGINX ingress controller installation. It acts as a reverse proxy between users and the Jenkins master. The ingress controller can also be used for terminating SSL/TLS connections.

Jenkins stateful service connects user requests with the Jenkins master running inside the master pod. A **StatefulSet** is used for using stable persistent storage for Jenkins home. The persistent volume claim is defined inside the **StatefulSet**. The persistent volume claim will eventually be backed up by a vendor-specific storage service, e.g. an EBS on AWS.

The Jenkins master server runs on a separate Kubernetes node. The master pod hosts Jenkins running as a Docker container and build agents run on a different pod. This is done to create separation between the agents and the master. Build agents are managed by Jenkins Kubernetes plugin. This plugin launches build agents on demand.
