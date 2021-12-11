# Installing Jenkins X



## Installing JX and kubectl Command Line Tool

JX is a command line tool for installing and operating Jenkins X. It provides an API to interact with the underlying Kubernetes cluster, and allows you to create and export your software projects into Jenkins X. It also lets you interact with your pipelines and promote your applications to various environments. Not only that, if you don't have a working Kubernetes cluster, JX will help you create one.

You can find the latest stable JX releases on [GitHub](https://github.com/jenkins-x/jx/releases).

JX can be installed on Windows, MacOS and Linux. An installation process is very straightforward and can be accomplished with just a few commands.

Here is an example of how to install JX on Linux:

Start by downloading the latest version of the binary on your machine:

**curl -L htt‌ps://github.com/jenkins-x/jx/releases/download/v3.2.182/jx-linux-amd64.tar.gz | tar xzv**\
**chmod +x jx**

Move the JX binary to a location in your executable path:

**sudo mv jx /usr/local/bin**

You can verify your installed version by typing the following command:

**jx --version**

Please refer to [Jenkins X Documentation](https://jenkins-x.io/v3/admin/setup/jx3/) for instructions on how to install the JX tool on other operating systems.

Kubernetes command line tool kubectl is also required to run commands on your Jenkins X Kubernetes cluster. Please use the following command to install the latest version of kubectl on Linux OS:

**curl -LO "ht‌tps://dl.k8s.io/release/$(curl -L -s ht‌tps://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl**

Finally, test the version of a binary:

**kubectl version --client**

Please refer to [kubectl Documentation](https://kubernetes.io/docs/tasks/tools/#kubectl) for installation instructions on Windows and Mac OS platforms.

## Setting Up a Kubernetes Cluster

A functional Kubernetes cluster is also required for Jenkins X installation. Jenkins X supports many vendor-provided clusters including GKE, AKS, EKS, OpenShift, etc. Moreover, Jenkins X can also be installed on on-premise Kubernetes clusters and on [minikube](https://minikube.sigs.k8s.io/docs/start/). If you have a functional cluster in your private cloud, you should be able to install Jenkins X in that environment too. Setting up a private Kubernetes cluster is beyond the scope of this course.

In this chapter, we will use the Google Kubernetes Engine (GKE) platform to create a Kubernetes cluster and install Jenkins X on that cluster. If applicable, [GCP free tier](https://cloud.google.com/free) can be used for this installation. For the lab exercises, we will use minikube to create a local Kubernetes cluster and install Jenkins X in it, but you can use any vendor-based Kubernetes cluster to complete the exercises.

We will also need a gcloud binary tool to set up command line connectivity with a GCP account. This tool is installed as part of the [Google Cloud SDK](https://cloud.google.com/sdk).

In addition, we will need [Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli#install-terraform), a popular Infrastructure as Code tool. Jenkins X leverages Terraform to manage vendor-specific installation details.\


## GitOps Repositories

Jenkins X follows GitOps principles for installing and operating Jenkins X. As part of the requirements, we will start by creating a dedicated [organization](https://github.com/login?return\_to=https%3A%2F%2Fgithub.com%2Forganizations%2Fplan) in GitHub. It is highly recommended that you use a GitHub organization and not your personal repository to avoid problems with ChatOps and automated webhook creation.

You will also need a bot user with a personal access token for accessing newly created infrastructure repositories. Ideally this bot user should be different from your personal user. Once you are logged in to your GitHub account, you can click [here](https://github.com/settings/tokens/new?scopes=repo,read:user,read:org,user:email,write:repo\_hook,delete\_repo,admin:repo\_hook) to create a personal access token. Finally, invite the new bot user to be a part of the Jenkins X organization. This will grant Jenkins X access to all repositories that belong to this organization.

As part of the installation process, two repositories will be created:

1. Infrastructure repository
2. Cluster repository

All infrastructure-related artifacts will be stored inside the first repository. Terraform will use this repository to create the cluster in Google Cloud. All cluster specific artifacts, e.g. secrets and Helm charts will be stored inside cluster repository.

Jenkins X provides templates for these repositories. Templates help create server-side clones of the repositories. Below are the URLs for the template repositories:

* [htt‌ps://github.com/jx3-gitops-repositories/jx3-terraform-gke/generate](https://trainingportal.linuxfoundation.org/learn/course/ci-cd-with-jenkins-x-lfs268/installing-jenkins-x/htt%E2%80%8Cps://github.com/jx3-gitops-repositories/jx3-terraform-gke/generate)
* [https://github.com/jx3-gitops-repositories/jx3-gke-gsm/generate](https://github.com/jx3-gitops-repositories/jx3-gke-gsm/generate)

_**NOTE**: You can generate these repositories from the web browser. Please ensure that you are already logged into your GitHub account and choose the newly created organization as the user for these repositories._

As a next step, please clone these repositories into your local working directory. You will modify the **values.auto.tfvars** file located inside the **jx3-terraform-gke** directory. Next, you need to add a couple of new entries. First, you will add the URL of the cluster repository. Next, you will add the name of the GCP project to be used for this cluster. This project needs to be active before you create your cluster. Finally, you will add a flag to indicate that you are using Google secret manager to store the secrets. Jenkins X also supports the Hashicorp Vault secret manager. If you want to use it, you will need to generate an appropriate repository first. Use this [link](https://github.com/jx3-gitops-repositories/jx3-gke-vault/generate) to generate a vault secrets repository.

The modified **values.auto.tfvars** file will look similar to this:

**resource\_labels = { "provider" : "jx" }**

**jx\_git\_url = "htt‌ps://github.com/orgName/jx3-gke/gsm"**

**gcp\_project = "my-gcp-project"**

**gsm = true**

Make sure your changes are committed to Git and pushed to the master infrastructure repository.

Before we start installation, we need to make sure that the gcloud binary is configured to talk to the GCP project we have defined inside the **values.auto.tfvars** file. The following command will list all the valid configurations and a true flag will indicate the active configuration:

**gcloud config configurations list**

We also need to set GitHub user and access token as environment variables so Terraform can use them during the installation:

**export TF\_VAR\_jx\_bot\_username=my-bot-username**

**export TF\_VAR\_jx\_bot\_token=my-bot-token**

Let’s create a Kubernetes cluster and install Jenkins X with all its components inside this cluster. Following commands should be executed sequentially:

**terraform init**

**terraform plan**

**terraform apply**

Jenkins X installation can be verified by running the following commands:

**jx ns js**

**jx verify install**

Finally, you can launch the UI console by running:

**jx ui**

This will open Octant UI in a browser window and you can verify newly created Jenkins X environments.

At this point, we have successfully completed installation of Jenkins X running in GKE, and we are ready to create projects.

## Upgrading Jenkins X

There are three components that we need to upgrade:

* **jx** binary aka Jenkins CLI client
* Jenkins X cluster
* Kubernetes infrastructure

**jx** **binary upgrade**: Jenkins X CLI client should be periodically upgraded.

The following command can be used to upgrade the client:

**jx upgrade cli**

To upgrade **jx** subcommand plugins run:

**jx upgrade plugins**

For more information on CLI upgrade, please refer to [Jenkins X upgrade Documentation](https://jenkins-x.io/v3/admin/setup/upgrades/cli/).

**Jenkins X cluster upgrade**: Jenkins X cluster can be upgraded automatically or manually. Let's look at each of these options.

**Automatic upgrade**: Automatic upgrades can be enabled by updating the **jx-requirements.yml** file located in your cluster development directory:

**apiVersion: core.jenkins-x.io/v4beta1**\
**kind: Requirements**\
**spec:**\
&#x20; **autoUpdate:**\
&#x20;   **enabled: true**\
&#x20;   **schedule: “0 0 \* \* \*”**\
&#x20;   **autoMerge: true**

The above configuration will automatically upgrade at midnight. Jenkins X runs the upgrade as part of a pipeline run. If the **autoMerge** option is set to **true**, Jenkins X will automatically merge the pull request to finish the upgrade.

Automatic upgrades may not be practical for most of us. Thankfully there is also an option to perform manual upgrades.

**Manual upgrade**: The manual upgrade can be triggered by running a command in your development cluster repository. Before launching the upgrade, please ensure your local directory is in sync with the remote repository and all local changes are pushed to the remote:

**jx gitops upgrade**

The above command does the following:

* Upgrades your local [version stream](https://jenkins-x.io/about/concepts/version-stream/) to the latest.
* Applies any changes in the version stream to the [boot job and its configuration](https://jenkins-x.io/v3/about/how-it-works/#boot-job).
* Creates a pull request in your development environment repository containing all the above changes.

The pull request will run the development master pipeline in your cluster to apply the upgrade. You can view the upgrade log by running the following command:

**jx admin logs**

**Kubernetes infrastructure upgrade**: Kubernetes configuration is stored inside the infrastructure Git repository and Terraform is used to deploy these changes. Make sure you update **values.auto.tfvars** file with intended changes and run the following Terraform commands to upgrade your Kubernetes cluster:

**export TF\_VAR\_jx\_bot\_username=\[your bot username]**

**export TF\_VAR\_jx\_bot\_token=\[your bot token]**

**terraform get -update**

**terraform plan**

**terraform apply**

****

## Additional Resources

To learn more, we recommend that you take a look at the following resources:

* [_“Amazon: Setup Jenkins X on EKS on AWS”_](https://jenkins-x.io/v3/admin/platforms/eks/)
* [_“OpenShift: Setup Jenkins X on an Existing OpenShift Cluster”_](https://jenkins-x.io/v3/admin/platforms/openshift/)
* [_“Azure: Setup Jenkins X on Azure”_](https://jenkins-x.io/v3/admin/platforms/azure/)
* [_“jx: Command Line Interface Reference”_](https://jenkins-x.io/v3/develop/reference/jx/)
* [_“On-Premises: Setup Jenkins X on Vanilla Kubernetes”_](https://jenkins-x.io/v3/admin/platforms/on-premises/)
* [_”Jenkins X Capabilities Matrix”_](https://jenkins-x.io/about/capabilities/)
