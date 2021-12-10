# Helm

## Helm

[Helm](https://helm.sh/docs/) is a package manager for Kubernetes. Helm charts help define, install, upgrade, and rollback even the most complex Kubernetes applications. A Helm chart includes deployment, service, config maps, etc. It is templated so the variables can be easily manipulated. Application dependencies can also be managed via Helm.

Jenkins X uses Helm to package and deploy applications inside a Kubernetes cluster.

## Helmfile

[Helmfile](https://github.com/roboll/helmfile) is a declarative specification for creating Helm charts. Jenkins X uses **helmfiles** to install, upgrade and configure Helm charts.

## Kuberhealthy

[Kuberhealthy](https://github.com/kuberhealthy/kuberhealthy) is a monitoring operator for Kubernetes. It lets you create your own tests and integrates seamlessly with tools like Prometheus and InfluxDB.

Jenkins X uses Kuberhealthy to monitor itself.

## Tekton

[Tekton](https://cloud.google.com/tekton), formerly known as Knative pipelines, is an extension built on Kubernetes for creating cloud-native CI/CD workflow. It allows you to use tools of your choice to build container images. Pipeline runs are executed in containers and can scale on-demand to meet increased demand. Tekton can deploy applications to multiple platforms including serverless, virtual machines and Kubernetes clusters.

Tekton is the pipeline execution engine of Jenkins X. At the run time, Jenkins X pipelines are converted into Tekton resources and are executed inside a Kubernetes cluster.

## Lighthouse

[Lighthouse](https://github.com/jenkins-x/lighthouse) is a lightweight ChatOps-based webhook handler to execute Jenkins X pipelines on webhooks for multiple Git providers such as GitHub, BitBucket, GitLab, etc.

Lighthouse is created by Jenkins X team to overcome Prow’s inability to work with SCM tools other than GitHub.

## Skaffold

[Skaffold](https://skaffold.dev) is a tool that makes Kubernetes deployments easy and repeatable. Skaffold will build a Docker image, push it to a registry, and deploy it to a Kubernetes cluster. It watches the local source code directory and rebuilds and redeploys automatically whenever the source code changes.

Jenkins X uses Skaffold to build Docker images. Jenkins X DevPods use Skaffold for doing incremental builds on source code changes.

## Kaniko

[Kaniko](https://github.com/GoogleContainerTools/kaniko) is a tool used to build container images from a Dockerfile. Kaniko does not use Docker daemon and can be used to build images securely.

## Kpt

[Kpt](https://googlecontainertools.github.io/kpt/) is used to create declarative workflows based on configuration files. It's also used to update, validate and apply Kubernetes configurations.

## Octant

[Octant](https://octant.dev) is used to visualize Kubernetes workloads. Jenkins X uses it as a plugin for creating a visualization console.

## Terraform

[Terraform](https://www.terraform.io) is an Infrastructure as Code tool for creating vendor-agnostic infrastructure in various public clouds. Jenkins X uses Terraform to provision Kubernetes clusters in various public clouds.

## Jenkins

[Jenkins](https://www.jenkins.io) is a very popular CI tool. Jenkins X provides interoperability with Jenkins. To learn more about Jenkins, enroll in the [Introduction to Jenkins (LFS167x)](https://training.linuxfoundation.org/training/introduction-to-jenkins-lfs167/) and [Jenkins Essentials (LFS267)](https://training.linuxfoundation.org/training/jenkins-essentials-lfs267/) courses.

## Nexus

[Nexus](https://www.sonatype.com/nexus-repository-oss) is an open source binary artifact management tool which stores and manages versioned build artifacts. An in-built Nexus repository manager is used to ship Jenkins X. Apart from Nexus, Jenkins X community is also working on a low memory footprint replacement for Nexus called [Bucketrepo](https://github.com/jenkins-x/bucketrepo).
