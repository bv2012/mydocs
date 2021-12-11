# Jenkins X Architecture

## Architecture Overview

Now that the basic building blocks are behind us, let's explore the internal components of Jenkins X.

The diagram below describes how various Jenkins X components interact with each other to create a CI/CD flow.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/69lwjr618e8x-LFS268\_CourseGraphics-04.png)

**Jenkins X High Level Architecture**

&#x20;

In Jenkins X, CI/CD flow starts with a commit or a pull request to SCM. Next, while Prow/Lighthouse listeners are waiting for SCM events, they:

* Listen to Git webhooks
* Implement Git ChatOps commands
* Dispatch webhook requests to Jenkins X Pipeline controller
* Report the status of pipeline builds to SCM.

Alternatively, Jenkins X also provides a command line tool called JX to interact with tooling components. For example, you can use the **jx quickstart** command to create a Jenkins X CI/CD flow. You can also use JX CLI to import a pre-existing project into Jenkins X. We will further explore this functionality in the subsequent chapters.

Jenkins X pipeline controller creates Tekton tasks and pipelines based on the Jenkins X YAML definition. Tekton is the pipeline execution engine and it does the following:&#x20;

* Defines pipelines for each project/repository
* Defines steps for each of the pipelines
* Executes pipelines for each build/commit
* Stores artifacts in registries
* Deploys application into permanent/temporary environments.

Kubernetes provides the runtime environment for Jenkins X. Jenkins X components are installed on a Kubernetes cluster and are managed via GitOps principles.

&#x20;

![Jenkins X Architecture](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/vq7w201x6u57-JenkinsXArchitecture.png)

**Jenkins X Architecture**\
\


&#x20;

Jenkins X uses a loosely-coupled architecture of promising cloud-native technologies to create CI/CD for microservices. Kubernetes, Tekton and Git form the core of Jenkins X. Git generates webhooks, Tekton creates pipelines and deployment environments, and Kubernetes provides the operating environment.
