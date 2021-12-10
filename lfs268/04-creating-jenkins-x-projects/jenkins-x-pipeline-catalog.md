# Jenkins X Pipeline Catalog

Jenkins X comes with a [pipeline catalog](https://github.com/jenkins-x/jx3-pipeline-catalog) for different languages. This catalog includes:

* Tasks
* Build packs
* Helm

Jenkins X uses build packs to turn source code into Kubernetes applications. [Draft](https://github.com/Azure/draft/)-style build packs are used for different languages to create runtimes, as well as to build tools and required configuration files. These build packs create the following default files (if they are not yet present in the projects being created/imported):

* A **Dockerfile** to create a deployable Docker image.
* Tekton task files for defining CI/CD steps.
* A Helm chart to create deployable Kubernetes applications.
* A preview Helm chart to define any dependencies for deploying to a preview environment.

Jenkins X default build packs are stored on [GitHub](https://github.com/jenkins-x/jx3-pipeline-catalog/tree/master/packs). The **jx** command line tool clones the build packs every time you create or import a project into Jenkins X. You can also add your own build packs. We will discuss this in more detail later in the course.
