# What is ArgoCD

## What is ArgoCD?

ArgoCD is a tool used to implement principles of GitOps in a Kubernetes environment. It extends Kubernetes by providing custom resource definitions (CRDs) and controllers to take actions (deploy) based on those resources.

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/ktcbuua3bass-ArgoCDDeploymentinKubernetes.png)

**ArgoCD Deployment in Kubernetes**

## How Does Jenkins Interface with ArgoCD?

The following diagram depicts the workflow between ArgoCD and Jenkins, both running natively on Kubernetes.\
\


![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/fwqpvqdv0egk-ArgoCDandJenkinsWorkflow.png)

**ArgoCD and Jenkins Workflow**

\
\
1\. Jenkins is deployed on Kubernetes as a native application. It uses the Kubernetes pods-based agent setup.

2\. ArgoCD is deployed natively inside the same Kubernetes cluster as part of this course setup. Users write YAML manifests with Argo-specific custom resources definitions. Argo, in turn, reads changes to any CRDs it exposes and takes action based on them, which typically involves deploying to Kubernetes. This is how users communicate with ArgoCD.

3\. Jenkins needs the capability to only synchronize a specific ArgoCD application deployment. This can be achieved by:

* Creating an API user with Argo
* Storing API users’ tokens as Kubernetes secrets
* Defining an ArgoCD policy with relevant authorization (only run a sync for a specific application) provided to an API user
* Configuring a Jenkins job (project) that reads a token from the Kubernetes secret (the secret assumes the role of the API user and triggers syncing), which ultimately results in a deployment to Kubernetes.

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/4890wz9fowp5-DeploytoDevStagewithArgoCD.png)

**Deploy to Dev Stage with ArgoCD**

\


In the above diagram, the _Deploy to Dev_ stage is where Jenkins assumes the role of the API user created by Argo and triggers the deployment of a specific application on Argo.
