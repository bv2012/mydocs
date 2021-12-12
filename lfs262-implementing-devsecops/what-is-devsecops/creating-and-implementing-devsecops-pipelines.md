# Creating and Implementing DevSecOps Pipelines



## DevSecOps Practices and Delivery Pipeline

Let’s now learn how to put it all together and create a DevSecOps pipeline. Here we assume that:

* We are dealing with a modern, cloud native application.
* This application is being packaged as a container image.
* And it is being deployed with Kubernetes.

## DevOps Pipeline

First, take a look at a simple continuous integration pipeline that you would begin with when you get started with DevOps practices. The following diagram illustrates some common components of a CI pipeline.

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/tkrqmwyjllk1-ComponentsofaCIPipeline.png)

**Components of a CI Pipeline**

The goal of this pipeline is to provide instant, continuous feedback.

Pipeline Stages

*   Build

    This stage notifies us whether the change in the application code breaks the build. In a collaborative environment, it is of utmost importance to ensure that the change introduced by one developer does not result in a build failure, causing everyone else’s work to be impacted, as they would not be able to test their changes due to compilation failure. This is generally the very first step in the CI pipeline. In case of an application which does not need compilation (e.g., Python, Node.js, which are interpreted languages), the build stage generally checks whether dependencies are being resolved and installed correctly.
*   Test

    The next stage in the continuous integration pipeline is to run unit tests. DevOps introduces the principle of shifting tests to the left side of the application delivery process. This reduces the rework, as well as the cost of detecting and mitigating issues. The earlier the problems are detected, the easier, quicker, and cheaper they are to fix.
*   Package

    After ensuring the code has gone through essential sanity checks, it is time to package the application into an artifact and prepare it to be distributed. Artifacts are generally pushed to a common repository, from where they are distributed further for deployment.

A typical DevOps delivery pipeline would also include the deployment stages, but this is a simplistic depiction of how you start with continuous integration.\


What is clearly missing in this DevOps pipeline? The security checks! That is the whole reason why we are here, discussing DevSecOps. Let’s now look at how this pipeline can evolve into something that we can call a DevSecOps pipeline.

## Components of a DevSecOps Pipeline

Let’s say we took the same pipeline described in the previous subsection, ran some security magic, and converted it into a DevSecOps pipeline. How would it look like?

The following diagram depicts exactly such a pipeline that focuses on security at every stage of the delivery process.

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/j6rrzwephxjv-DevSecOpsPipelineVisualized.png)

**DevSecOps Pipeline Visualized**

&#x20;\
Let’s analyze the additional security-related components of this pipeline. This is going to help you understand common DevSecOps practices.

## DevSecOps Practices

Common Security Practices to Embed into a DevSecOps Pipeline

*   Secrets Scanning

    It is important to ensure that no sensitive information, such as passwords, API keys, and cloud access credentials, are accidentally checked into the repository. Secrets scanners can be either run as pre-commit hooks (checks which are automatically run when a developer tries to commit to the repository) or can be enforced to run as the first step in the continuous integration pipeline, or both.
*   Software Composition Analysis (SCA)

    A majority of API-based application software today uses open source, third-party components, including libraries and frameworks. This forms your supply chain. How secure are the components incorporated in your software? Are there any vulnerabilities that you must patch? What are the security implications of using these components? Software Composition Analysis (SCA) attempts to detect such risks and provides you with inputs on how to mitigate them.
*   Software Bill of Materials (SBOM)

    This relates to Software Component Analysis. Standards such as CycloneDX help you generate a list of components akin to a bill of materials and reports on which components are in use; with respective versions, licenses, known vulnerabilities, etc. This helps add transparency, as well as aids understanding of the security posture of your application infrastructure.
*   Static Application Security Testing (SAST)

    The SAST stage helps you scan your application code and compare it against best practices, as well as detect any vulnerabilities in the code. This is a form of white box testing, whereas the tool being utilized has to be aware of the programming language being used and scan the code according to that. Like running unit tests, the goal here is to shift tests right, in this case within the context of security issues. The earlier the code issues are detected, the cheaper and faster it is to mitigate them.
*   OSS License Checker

    This stage is run as part of the DevSecOps pipeline to automatically detect the licenses for the components in use and compare it against the list of allowed licenses. As new components are added or existing components updated, there is a possibility of license terms being updated or new licenses being incorporated. This may have legal implications and can be incompatible with the organization’s policies. Having an automated process to detect such an event as part of the delivery pipeline is extremely useful.
*   Container Image Linting and Scanning

    As increasingly more applications are being packaged and deployed as containers, what gets packaged along with your container as part of the artifact becomes an important security consideration. It is very common to use a base image from the registry which is outdated, unpatched and introduces many vulnerabilities. It is also a good idea to check if you are following best practices while building the container image itself. Container image linting and scanning provides the feedback on both aspects: how safe is the image along with all the packages in it, as well as how well is your Dockerfile written to build that image. Running these scans as part of an automated pipeline can be very handy.
*   GitOps-Based Secure Deployment

    A commonly ignored part in the delivery process is deployment security. It is common to see a continuous integration platform such as Jenkins being provided with access to deployment systems with wider authorization, as well as to deployment systems provided to a wider audience. GitOps could help separate the deployment process from development and CI processes, keep the deployment code separate, and, at the same time, provide very limited access to systems such as Jenkins to trigger deployments only to limited applications. Since GitOps is built around Git-based workflows with support for pull requests, branching models, code reviews, etc., only the approved code is deployed to an environment.
*   Dynamic Application Security Testing (DAST)

    While SAST is a white box testing methodology, DAST takes a black box testing approach to detect security issues. Dynamic Application Security Testing (DAST) typically refers to automated penetration testing of web applications to detect issues wider than those present just in the application code. There could be multiple components which can form a chain of connections between a user and the application, which can be possibly exploited. This includes reverse proxies, network applications, caching services, databases, etc. To provide a holistic view of your application’s security posture, it is important to take an outsider’s view and simulate external attacks in addition to taking an insider’s view with SAST. DAST is run after the application has been deployed in a Dev/QA environment.

While the practices described above form the core of the DevSecOps pipeline, it goes beyond this. DevSecOps is about taking a holistic approach and embedding security into all development and operational practices. This leads to additional processes, such as creating daily SecOps pipeline runs with compliance scans, using IDE plugins that detect security vulnerabilities, etc. True DevSecOps is about trying to follow the defense-in-depth approach. And despite following all these practices, it is important to acknowledge that you cannot guarantee 100% security, but the attempt is to get closer to it. And it is an iterative, continuous process!

## How to Choose the Right Tools to Implement DevSecOps?

So far, you learned why DevSecOps is needed and explored the risks related to modern applications. By now, you also hopefully came to an agreement that including key DevSecOps practices is an important aspect of the delivery pipeline. Now, you should consider the following two questions: _"Which tools should you use?"_ and _"How should you choose them?"_. While there are many commercial as well as open source solutions for each of the practices we discussed while visualizing the DevSecOps pipeline, there are common criteria that you can use to shortlist the tools.

Tools included in the DevSecOps pipeline should be...

*   Automatable

    They should have support for API-based communication or, at a minimum, a command line interface which can be used to script tasks.
*   Containerized

    They should support being packaged as a container application. This enables them to be integrated with the continuous integration systems which often run projects with container-based agents.
*   Free from licensing limitations

    Many commercial applications have limitations on the number of scans, number of parallel scans, etc.
*   Configurable

    This is helpful to counter false negatives/false positives.

Almost all tools recommended as part of this course are open source and available to use for free. However, you have to select the tools based on your application stack, organization goals, desired features, and availability of resources to maintain the tools.



