# Jenkins Installation Using Helm

## Jenkins Installation Using the Helm Package Manager

***

Helm is a Kubernetes package manager. You can use Helm to install Jenkins. However, there is a learning curve as you first need to know how to customize deployment parameters. As learning Helm is outside the scope of this course, you can go through [Helm Documentation](https://helm.sh/docs/) to gain basic understanding of Helm. We also recommend LF's ["Managing Kubernetes Applications with Helm"](https://training.linuxfoundation.org/training/managing-kubernetes-applications-with-helm-lfs244/) (LFS244) course.

We need to install Helm binaries on our client machine. We will use Helm version 3.x. Helm releases can be downloaded from [here](https://github.com/helm/helm/releases). The Helm community maintains a bunch of stable release packages. There is one such stable package available for Jenkins as well. Let's start by adding reference to all stable Helm packages.

**`$ helm repo add jenkins htt‌ps://charts.jenkins.io`**

**`"jenkins" has been added to your repositories`**

Now let’s see if we can find a Jenkins chart in this repo.

**`$ helm search repo jenkins`**

**`NAME             CHART VERSION   APP VERSION      DESCRIPTION`**\
``**`jenkins/jenkins  2.7.1           lts              Open source continuous integration server. It s.`..**

The details of this chart can be found [here](https://hub.helm.sh/charts/stable/jenkins). You can also check the default values of this chart by executing the following command:

**`$ helm inspect values jenkins/jenkins`**

The output of the above command will be fairly large and will list the default values of your Jenkins installation. If you like, you can change the defaults. There are two options available.

1. use **–set** flag and pass **key=value**
2. create a values YML file and pass it on as argument to Helm like **--values=jenkins-values.yml**

Let's create our own **jenkins-values.yml** file. We will modify some values to customize our installation, starting with the Docker image tag.

&#x20;**  `tag: "lts-alpine"`**\
```  `**`resources:`**\
```    `**`requests:`**\
```      `**`cpu: "1000m"`**\
```      `**`memory: "1000`Mi"**

Rather than using the default **lts** image, we will use **lts-alpine**. We will also bump up CPU and RAM resources. We will need to ensure that our Jenkins server is guaranteed to get 1 CPU core and 1 GB RAM. In your case, you should adjust these values according to your needs. The Helm chart includes some plugins as part of the installation. We have added some extra plugins per our requirement.

**`installPlugins:`**\
```  `**`- durable-task:latest`**\
```  `**`- workflow-durable-task-step:latest`**\
```  `**`- blueocean:latest`**\
```  `**`- configuration-as-code:latest`**\
```  `**`- credentials:latest`**\
```  `**`- ec2:latest`**\
```  `**`- git:latest`**\
```  `**`- git-client:latest`**\
```  `**`- github:latest`**\
```  `**`- kubernetes:latest`**\
```  `**`- pipeline-utility-steps:latest`**\
```  `**`- pipeline-model-definition:latest`**\
```  `**`- slack:latest`**\
```  `**`- thinBackup:latest`**\
```  `**`- workflow-aggregator:latest`**\
```  `**`- ssh-slaves:latest`**\
```  `**`- ssh-agent:latest`**\
```  `**`- jdk-tool:latest`**\
```  `**`- command-launcher:latest`**\
```  `**`- github-oauth:latest`**\
```  `**`- google-compute-engine:latest`**\
```  `**`- pegdown-formatter:latest`**

Notice that we are installing the latest versions of all these plugins. If you want, you can install specific versions by replacing the latest version with the actual one. For example, directive **kubernetes:1.22.3** will specifically install version 1.22.3 of the Jenkins Kubernetes plugin.

The Ingress Controller can be enabled via the YAML. You just need to make sure you have installed the Ingress Controller. You can visit [“NGINX Ingress Controller: Installation Guide”](https://kubernetes.github.io/ingress-nginx/deploy/) to learn how to install it on your platform.

**ingress:**\
&#x20;   **enabled: true**\
&#x20;   **paths:**\
&#x20;     **- backend:**\
&#x20;         **serviceName: jenkins**\
&#x20;         **servicePort: 8080**\
&#x20;   **apiVersion: "extensions/v1beta1"**\
&#x20;   **labels: {}**\
&#x20;   **annotations:**\
&#x20;     **kubernetes.io/ingress.class: "nginx"**\
&#x20;     **nginx.ingress.kubernetes.io/ssl-redirect: "false"**\
&#x20;     **nginx.ingress.kubernetes.io/proxy-body-size: 50m**\
&#x20;     **nginx.ingress.kubernetes.io/proxy-request-buffering: "off"**\
&#x20;     **ingress.kubernetes.io/ssl-redirect: "false"**\
&#x20;     **ingress.kubernetes.io/proxy-body-size: 50m**\
&#x20;     **ingress.kubernetes.io/proxy-request-buffering: "off"**\
&#x20;   **hostName: 34.83.44.19.nip.io**

Helm install makes enabling Kubernetes Role Based Access Control (RBAC) very easy. There is no need to create a **ServiceAccount**, **Role** and **RoleBindings** resources. All you need is the following entry in values YML:

**rbac:**\
&#x20; **create: true**

The Helm chart also defines persistent storage volume. The default setting is 8GB. You can adjust it to your needs.

&#x20; storageClass:\
&#x20; annotations: {}\
&#x20; accessMode: "ReadWriteOnce"\
&#x20; size: "8Gi"

As you can see, Helm makes Kubernetes package installation fairly easy. Our newly created values YAML file will look like [this](https://gist.github.com/hgautam/7a00f688882e4b09ddf7b99ba2d3834c).

Let’s finish the installation now. We are going to execute the **helm install** and pass our updated values YML file as an extra argument.

**$ helm install jenkins jenkins/jenkins --namespace jenkins \\**\
**--values=jenkins-values.yml**

The above command will install the Jenkins server in the Jenkins namespace. Please note that Jenkins namespace needs to pre-exist before you execute this command. A successful install command will produce an output similar to following:

**NAME: jenkins**\
**LAST DEPLOYED: Tue Aug 18 09:48:45 2020**\
**NAMESPACE: jenkins**\
**STATUS: deployed**\
**REVISION: 1**\
**NOTES:**\
**1. Get your 'admin' user password by running:**\
&#x20; **printf $(kubectl get secret --namespace jenkins jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo**\
\
**2. Visit ht‌tp://35.233.228.104.nip.io**\
\
**3. Login with the password from step 1 and the username: admin**

You might want to save the command to get the admin password as you will need it to access your instance.

Let’s check the status of this deployment.

$ kubectl get pods -n jenkins

NAME                      READY   STATUS    RESTARTS   AGE\
jenkins-f4b845cf8-mz77q   2/2     Running   0          16m

$ kubectl get pvc -n jenkins

NAME      STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE\
jenkins   Bound    pvc-f94439f8-e0a1-44c6-b6a1-d05ab0bbc6a7   8Gi        RWO            standard       51s

$ kubectl -n jenkins rollout status deploy jenkins

deployment "jenkins" successfully rolled out



Let’s get the ingress entry point to our Jenkins application.

**$ kubectl get ingress -n jenkins**

**NAME      HOSTS                   ADDRESS          PORTS   AGE**\
**jenkins   35.233.228.104.nip.io   35.233.228.104   80      15m**

Please notice the entry under “Hosts” is the **hostName** field declared in **values.yaml**. In a production environment this should be the DNS name of your load balancer. Your Jenkins instance can be accessed by ht‌tp://35.233.228.104.nip.io.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/igfjc2m9hbkl-JenkinsLandingPage.png)

**Figure: Jenkins Landing Page**

&#x20;

Before you access the instance, you need to retrieve the admin password. This password is required to access our instance.

**printf $(kubectl get secret --namespace jenkins jenkins -o \\**\
**jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo**

You should be able to access your Jenkins application with the above password.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/izvvr7iu5f3d-JenkinsWelcomePage.png)

**Figure: Jenkins Welcome Page**

&#x20;

Please notice that this Jenkins instance has no build executors configured. This means that user build jobs will not run on the Jenkins master. It is a best practice. We will show how to run builds on external build agents as part of the “Scaling Jenkins Infrastructure with Dynamic Build Agents” chapter. Also, this instance is configured with a legacy security realm called _Delegate to servlet container_. You may want to change this to LDAP or Active Directory.

Please note that as part of these installation examples, we did not look at setting up TLS. Part of the reason for this is that we don’t have a valid domain that can be used to create TLS certificates. In a real-world production scenario, you should be able to create certificates for your actual domain. We strongly recommend using certificates created by third party issuing authorities. There are a couple of ways of applying the certificates to your Kubernetes cluster.

* Creating a Kubernetes TLS secret based on **.crt** and **.key** files
* Using a tool like [cert-manager](https://github.com/jetstack/cert-manager) to install and manage certificates in your Kubernetes cluster

In this chapter, we explored two ways of installing Jenkins on a Kubernetes cluster. Helm based installation is relatively quicker as most of the routine steps are pre-configured. However, this introduces the additional overhead of knowing Helm. Our example installation is still incomplete as we have not configured build agents yet. We will discuss that in a greater detail in the “Scaling Jenkins Infrastructure with Dynamic Build Agents” chapter.

\
