# Jenkins X Capabilities

Jenkins X is founded on capabilities of high performing teams as defined in the [_"Accelerate: The Science of Lean Software and DevOps - Building and Scaling High Performing Technology Organizations"_](https://itrevolution.com/book/accelerate/) book.



Jenkins X Capabilities

*   Use version control for all artifacts

    Jenkins X is inspired by GitOps - a term coined by Weaveworks. GitOps was originally created for Kubernetes cluster management. The main idea is to use Git as a single source of truth for all scripts and files needed to create and manage a cluster. Jenkins X advocates the same principle for CI/CD. All artifacts required to create CI/CD should reside in Git, and all changes should be made through Pull Requests.
*   Automate your Deployment process

    Jenkins X focuses on automating the entire deployment process including the process of creating deployment environments. Production and Staging environments are created out of the box. Temporary preview environments can also be automatically created from Pull Requests.
*   Use trunk-based development

    The _"Accelerate: The Science of Lean Software and DevOps - Building and Scaling High Performing Technology Organizations"_ book concluded that software teams that use trunk-based development with short lived branches perform better. Trunk-based development is an SCM (Software Configuration Management) branching model where developers collaborate in a single branch called trunk or master in Git. The focus of this approach is to resist the temptation of creating long-lasting feature branches to avoid future merging problems. Jenkins X uses the same principle to implement CI/CD workflows.
*   &#x20;Implement Continuous Integration

    DevOps best practices recommend testing and validating a proposed source code change before merging to master. Jenkins X provides out of the box support for configuring source code repositories, a Serverless CI engine running inside a Kubernetes cluster to provide Continuous Integration.
*   &#x20;Implement Continuous Delivery

    DevOps best practices recommend that every commit should be tested and validated in a live environment and should always be ready for release into Production. Jenkins X automates many parts of a release pipeline. It helps you create and configure the toolchain required for continuous delivery.
*   &#x20;Use loosely-coupled architecture

    The _"Accelerate: The Science of Lean Software and DevOps - Building and Scaling High Performing Technology Organizations"_ book has identified this as a key capability of high performing teams. Loosely-coupled architectures are lean, have less dependencies, have a single responsibility, and are easier to test and deploy. These qualities allow teams to work independently and release software quickly. Microservices are an excellent example of a loosely-coupled architecture. Jenkins X uses Kubernetes and other Kubernetes-based tools to achieve this capability. We will do a deep dive into Jenkins X architecture in the next chapter.
*   Architect for empowered teams

    Cloud-native applications have given rise to polyglot developers as Microservices components can be created in Golang, Java, NodeJS, etc. Jenkins X makes it easier to create scripted pipelines for applications written in various languages. Apart from this, Jenkins X also provides many add-ons to extend its functionality. Add-ons are similar to plugins in Jenkins. For example, Grafana and Prometheus can be implemented as add-ons to provide CI/CD metrics collection and visualization.
