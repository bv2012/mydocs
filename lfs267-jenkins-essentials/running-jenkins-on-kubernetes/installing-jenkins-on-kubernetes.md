# Installing Jenkins on Kubernetes

Jenkins can be installed on any Kubernetes cluster as long as you meet the Kubernetes version requirements mentioned in the requirements section. Kubernetes clusters can be created inside a vendor-provided public cloud or inside your company-provided infrastructure. Here is a [gist](https://gist.github.com/hgautam/aca87f3d5662859eb96fa214e9196a45) that shows how to create a single node Kubernetes cluster in GKE.

You can even use Minikube to create a cluster on your work computer and install Jenkins inside that cluster. Such a Jenkins installation would be fine for learning how to install Jenkins on Kubernetes, but is not recommended for production workflows.

Jenkins on Kubernetes can be installed via kubectl commands or Helm. A user is required to create YAML files for defining various resources and must use kubectl binary to deploy those resources inside a functional Kubernetes cluster. Helm, on the other hand, is a package manager for deploying Kubernetes applications. Helm packages are called charts and can be easily deployed via a Helm command line binary. In the rest of this chapter, we will cover the manual and Helm-based Jenkins installation.

## Manual Jenkins Installation Using kubectl and YAML Files

***

We will start with a manual installation that would require the following:

* A working Kubernetes cluster
* Installed NGINX Ingress Controller
* kubectl binary
* YAML files defining Kubernetes objects

As a first step, we need a namespace for installing Jenkins. Kubernetes namespaces are used to divide a cluster into virtual clusters and provide a way for distributing resources between multiple users. We will define namespace creation inside a YAML file like this:

**`apiVersion: v1`**\
``**`kind: Namespace`**\
``**`metadata:`**\
``` `**`name: jenkins`**

A Jenkins namespace will be used to install Jenkins applications. That's all good but we also need build agents - the nodes that will do the actual work. If you like, you can connect traditional build agents to a Jenkins server running in a Kubernetes cluster. However, what if you want the agents to run in the Kubernetes cluster too? The Jenkins Kubernetes plugin lets you do exactly that. We are going to explore this plugin in greater detail later on in the “Scaling Jenkins Infrastructure with Dynamic Build Agents” chapter. For now, we will show you how to create a separate build namespace so application and builds run on separate namespaces. This will help you manage cluster resources more efficiently. Let’s create a new namespace for Jenkins build agents.

**`apiVersion: v1`**\
``**`kind: Namespace`**\
``**`metadata:`**\
```  `**`name: build`**

Next comes the creation of a **ServiceAccount**. A **ServiceAccount** provides identity for all the processes running inside containers. If you don’t specify any **ServiceAccount** for your Jenkins pod, Kubernetes will assign one for you. As a result, your pod will be associated with the default **ServiceAccount**. Since the default account is quite restrictive, Jenkins running inside the pod will not be able to perform many important tasks. This will become a major hindrance when we will use Jenkins Kubernetes plugin to run distributed builds. To overcome this, we need to associate the Jenkins process with a **ServiceAccount** that has more permissions. Let’s create a **ServiceAccount**. The following YML section will do this for us.

**`apiVersion: v1`**\
**`kind: ServiceAccount`**\
**`metadata:`**\
&#x20; **`name: jenkins`**\
&#x20; **`namespace: jenkins`**

The Jenkins Kubernetes plugin requires extensive permissions on pods. It should be able to create pods, retrieve logs from them, execute processes on pods, delete pods, and so on. In addition to these, we will also grant read-only access to the secrets token associated with this ServiceAccount. The following YAML will define the required role:

**`kind: Role`**\
``**`apiVersion: rbac.authorization.k8s.io/v1`**\
``**`metadata:`**\
```  `**`name: jenkins`**

&#x20; **`namespace: build`**

**`rules:`**\
``**`- apiGroups: [""]`**\
```  `**`resources: ["pods"]`**\
```  `**`verbs: ["create","delete","get","list","patch","update","watch"]`**\
``**`- apiGroups: [""]`**\
```  `**`resources: ["pods/exec"]`**\
```  `**`verbs: ["create","delete","get","list","patch","update","watch"]`**\
``**`- apiGroups: [""]`**\
```  `**`resources: ["pods/log"]`**\
```  `**`verbs: ["get","list","watch"]`**\
``**`- apiGroups: [""]`**\
```  `**`resources: ["events"]`**\
```  `**`verbs: ["watch"]`**\
``**`- apiGroups: [""]`**\
```  `**`resources: ["secrets"]`**\
```  `**`verbs: ["get"]`**

As a final step, we need to tie the newly created **ServiceAccount** with the newly defined Role. The **RoleBindings** help us achieve exactly that. Here is the section of YML that will create such a **RoleBinding**:

**`apiVersion: rbac.authorization.k8s.io/v1beta1`**\
``**`kind: RoleBinding`**\
``**`metadata:`**\
```  `**`name: jenkins`**\
```  `**`namespace: build`**\
``**`roleRef:`**\
```  `**`apiGroup: rbac.authorization.k8s.io`**\
```  `**`kind: Role`**\
```  `**`name: jenkins`**\
``**`subjects:`**\
``**`- kind: ServiceAccount`**\
```  `**`name: jenkins`**\
```  `**`namespace: jenkins`**

Next, let's define a service resource to access the Jenkins application. Here is how we can define our service:

**`apiVersion: v1`**\
``**`kind: Service`**\
``**`metadata:`**\
```  `**`name: jenkins`**\
```  `**`namespace: jenkins`**\
``**`spec:`**\
```  `**`selector:`**\
```    `**`app: jenkins`**\
```  `**`ports:`**\
```  `**`- name: http`**\
```    `**`port: 8080`**\
```    `**`targetPort: 8080`**\
```    `**`protocol: TCP`**\
```  `**`- name: agent`**\
```    `**`port: 50000`**\
```    `**`protocol: TCP`**

We are defining two ports: 8080 and 50000. Port 8080 is the default Jenkins application port. Port 50000 is used by inbound (formerly known as “JNLP”) agents. Jenkins build agents running as part of the Kubernetes plugin will use port 50000 to connect to the Jenkins master.

At this point, Jenkins is not exposed to any traffic coming from the outside of the current Kubernetes cluster. The Ingress Controller will provide us with a way of routing external user requests to the Jenkins master. Let’s define an ingress resource.

**a`piVersion: extensions/v1beta1`**\
``**`kind: Ingress`**\
``**`metadata:`**\
```  `**`name: jenkins`**\
```  `**`namespace: jenkins`**\
```  `**`annotations:`**\
```    `**`kubernetes.io/ingress.class: "nginx"`**\
```    `**`ingress.kubernetes.io/ssl-redirect: "false"`**\
```    `**`ingress.kubernetes.io/proxy-body-size: 50m`**\
```    `**`ingress.kubernetes.io/proxy-request-buffering: "off"`**\
```    `**`nginx.ingress.kubernetes.io/ssl-redirect: "false"`**\
```    `**`nginx.ingress.kubernetes.io/proxy-body-size: 50m`**\
```    `**`nginx.ingress.kubernetes.io/proxy-request-buffering: "off"`**\
``**`spec:`**\
```  `**`rules:`**\
```  `**`- http:`**\
```      `**`paths:`**\
```      `**`- path: /jenkins`**\
```        `**`backend:`**\
```          `**`serviceName: jenkins`**\
```          `**`servicePort: 8080`**

As you can see, we are defining a backend service named Jenkins listening on port 8080. Based on this config, Kubernetes Ingress will route an incoming request to the backend.

Let’s now move on to installing Jenkins. We will create a **StatefulSet** with **1** replica. The idea is that a single instance of Jenkins is always running in the cluster. If for some reason Jenkins container goes down, Kubernetes cluster manager will restart it. This is the default high availability support provided by Kubernetes.

**`apiVersion: apps/v1`**\
``**`kind: StatefulSet`**\
``**`metadata:`**\
``` `**`name: jenkins`**\
``` `**`namespace: jenkins`**\
``**`spec:`**\
``` `**`selector:`**\
```   `**`matchLabels:`**\
```     `**`app: jenkins`**\
``` `**`serviceName: jenkins`**\
``` `**`replicas: 1`**\
\
**Jenkins container image is also defined in the YAML.**\
\
&#x20;   **      `containers:`**\
```      `**`- name: jenkins`**\
```        `**`image: jenkins/jenkins:lts-alpine`**\
```        `**`imagePullPolicy: Alway`s**

This Jenkins install will be based on the official Alpine-based Jenkins image. If you want to use a different image, you will have to update this entry.

We have chosen a stateful set as the Jenkins master needs stable persistent storage. Persistent storage volume will be created by the following snippet:

**`volumeClaimTemplates:`**\
``` `**`- metadata:`**\
```     `**`name: jenkins-home`**\
```   `**`spec:`**\
```     `**`accessModes: [ "ReadWriteOnce" ]`**\
```     `**`resources:`**\
```       `**`requests:`**\
```         `**`storage: 2Gi`**

In our example, we are creating a 2GB volume. However, this volume size may not be enough for a production Jenkins master. You should specify a value large enough to meet your needs. We will also specify resource constraints in YAML. For example, our application is guaranteed to get 1 cpu and 500 MB memory but cannot exceed 4 cpus and 8 GB of memory.

**`resources:`**\
```         `**`limits:`**\
```           `**`cpu: 4`**\
```           `**`memory: 8Gi`**\
```         `**`requests:`**\
```           `**`cpu: 1`**\
```           `**`memory: 500Mi`**

Now, these settings may or may not be sufficient for your production system. Hence you should choose appropriate numbers. Using limits and requests on resources is a good idea as it protects the Kubernetes cluster from being hijacked by renegade applications. The rest of the YAML configurations are pretty straightforward. You can see the entire YAML [here](https://gist.github.com/hgautam/061983d8301d5fbf69fd3532c75a5168).

At this point, all the prerequisites are ready. Let’s proceed with Jenkins installation by running the following kubectl command:

**$ kubectl apply -f jenkins.yaml**

**namespace/jenkins created**\
**namespace/build created**\
**serviceaccount/jenkins created**\
**role.rbac.authorization.k8s.io/jenkins created**\
**rolebinding.rbac.authorization.k8s.io/jenkins created**\
**service/jenkins created**\
**ingress.extensions/jenkins created**\
**statefulset.apps/jenkins created**

This command will create the following Kubernetes resources - namespaces, service account, service, ingress and a **StatefulSet** of Jenkins application server. Let’s validate the resources created as part of this installation. We should have two namespaces.

**$ kubectl get ns build jenkins**

**NAME      STATUS   AGE**\
**build     Active   28s**\
**NAME      STATUS   AGE**\
**jenkins   Active   28s**

Validate service resource.

**$ kubectl get svc -n jenkins**

**NAME      TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)              AGE**\
**jenkins   ClusterIP   10.101.77.56   \<none>        8080/TCP,50000/TCP   8m59s**

Validate rollout status.

**$ kubectl -n jenkins rollout status sts jenkins**

**statefulset rolling update complete 1 pods at revision jenkins-5d6c46d5d...**

Validate the ingress.

**$ kubectl get ingress -n jenkins**

**NAME      CLASS    HOSTS   ADDRESS         PORTS   AGE**\
**jenkins   \<none>   \*       192.168.64.46   80      11m**

You should be able to access your Jenkins server by accessing the ip listed under the “ADDRESS” field. For example, our instance can be accessed by: htt‌p://192.168.64.46/jenkins and you should see a screen similar to this:

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/avdo0by0iajh-image1.png)

**Figure: Unlock Jenkins**

&#x20;

Let’s retrieve the admin password by typing the following command:

**$ kubectl -n jenkins exec -it jenkins-0 -- cat /var/jenkins\_home/secrets/initialAdminPassword**

This password will help you unlock your Jenkins instance and you will be prompted to install plugins. After installing the plugins, you will see a Jenkins welcome page similar to the following:

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/nlqedhkotucd-image2.png)

**Figure: Jenkins Welcome Page**

&#x20;

You are now ready to use your Jenkins instance.

With manual installation behind us, let’s explore installing Jenkins with a Helm package.
