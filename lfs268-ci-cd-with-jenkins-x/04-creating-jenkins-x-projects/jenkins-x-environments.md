# Jenkins X Environments

Jenkins X envisions the entire pipeline workflow - starting from SCM commit to build, through tests, to deployment of the application. The applications are deployed into environments commonly referred to as Testing, Staging, and Production. Jenkins X provides each team with its own environments. Staging and Production environments are created by default as part of Jenkins X installation. These environments are defined inside **jx-requirements.yml** stored inside Jenkins X cluster repository.

**environments**:

\- **key**: **dev**

\- **key**: **staging**

\- **key**: **production**

The default configuration is created for a single cluster. Jenkins X lets you create your custom environments. Please refer to official [Documentation](https://jenkins-x.io/v3/develop/environments/config/#custom-environments-per-repository) for more information.

In our labs, we will use single cluster configuration. However, in production environments multi-cluster configurations are used. Jenkins X supports multi-cluster environments too. Please refer to official [Documentation](https://jenkins-x.io/v3/admin/guides/multi-cluster/) on setting up multi-cluster environments.

Jenkins X uses GitOps principles to manage the configuration and versions of Kubernetes resources deployed into various environments.

&#x20;

![Jenkins X Workflow](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/03y7tx3t5nt6-image7.png)

**Jenkins X Workflow**

&#x20;

Each environment maps to its unique namespace in a Kubernetes cluster. Whenever a pull request is opened for an environment, a pipeline run is started and a new application gets deployed in the environment namespace. The pipeline workflow can also promote applications from one environment to another. For example, a successful pipeline run can promote application from staging to production environment.

\
