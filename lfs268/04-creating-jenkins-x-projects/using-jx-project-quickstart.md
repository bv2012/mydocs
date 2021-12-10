# Using jx project quickstart



Quickstart is an easy way to create Jenkins X CI/CD workflow for an application. It provides a way of creating an application based on pre-created samples. The Jenkins X team has created a curated list of sample applications that can be quickly imported into Git. Every time you run a Quickstart, Jenkins X matches the correct build pack for the language of your application and this build pack does the rest - compiles, tests and deploys the application. Jenkins X Quickstarts are stored in this [GitHub organization.](https://github.com/jenkins-x-quickstarts)

If you are just starting out with Jenkins X, Quickstart is the best option to see Jenkins X’s capabilities. You can start with the following command:

**jx project quickstart**

Please note that Quickstarts can be filtered based on the programming languages. For example, if you want to list Golang Quickstarts, you can run the following command:

**jx project quickstart -l go**

Alternatively, you can use text-based filter like this:

**jx project quickstart -f http**

After executing one of the above commands, you will see the list of available Quickstarts. It will be similar to the following:

**jx project quickstart**

The following steps will be executed as part of the **jx project quickstart** command:\


* A new application will be created in a subdirectory on your computer.
* An application source code will be added to a Git repository.
* A remote Git repository will be created on a Git server, such as GitHub.
* Your application code will be pushed to a remote GitHub repository.
* The following default config files will be added:\
  \- A **Dockerfile** to build your application as a Docker image.\
  \- Tekton steps and triggers to create a CI/CD pipeline.\
  \- A Helm chart to deploy your application in the Kubernetes cluster.
* A webhook will be registered on the remote Git repository that triggers Tekton pipelines.
* The first pipeline run will be triggered.
