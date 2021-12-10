---
cover: >-
  https://images.unsplash.com/photo-1519389950473-47ba0277781c?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=2970&q=80
coverY: 0
---

# WHAT IS DEVSECOPS?

### Overview

By the end of this chapter, you should be able to:

* Understand the need for DevSecOps.
* Understand modern applications, the risks they creates and how to address them.
* Learn the step-by-step process to start implementing DevSecOps.
* Get introduced to the concept of defense-in-depth by learning about the layers of your application infrastructure.
* Visualize a DevSecOps pipeline and learn about common DevSecOps practices.

### The Need for DevSecOps

To understand what DevSecOps is, let's begin with the question "_Why should we care about it?"_. What better way to answer this question than narrating a few stories which will help you understand the impact of what happens in its absence:

* In 2016, [attackers breached Uber’s system](https://www.cnbc.com/2018/09/26/uber-to-pay-148-million-for-2016-data-breach-and-cover-up.html) and stole the personal information of approximately 57 million users across the globe. The breached data included location history, credit card and bank account details, social security numbers, dates of birth, etc. This breach, followed by the subsequent cover-up, resulted in Uber paying $148 million in penalty. The root cause for this breach was a bug in the API.
* In 2017, a well-known and unpatched vulnerability in the underlying framework used by an [application deployed by Equifax](https://www.csoonline.com/article/3444488/equifax-data-breach-faq-what-happened-who-was-affected-what-was-the-impact.html), a credit reporting agency, resulted in attackers stealing sensitive data, including the personal and financial information of millions of people. This resulted in Equifax paying more than $575 million in settlement. The root cause of this breach was an unpatched vulnerability in a component used by the software.
* A known [vulnerability in Kubernetes](https://snyk.io/blog/a-serious-security-flaw-in-runc-can-result-in-root-privilege-escalation-in-docker-and-kubernetes/) allowed attackers to escalate privileges, break out of a container, and gain root access to the underlying host. This could have a wider impact worldwide with all the unpatched Kubernetes installations, as Kubernetes is the gold standard in the world of container-based deployments. Your systems would be vulnerable had you not patched/updated Kubernetes.

Do we need to emphasize more why it is crucial to incorporate security into the software delivery process?

Well, the good news is that DevSecOps practices can help you manage most issues similar to the ones mentioned above by helping you detect such risks early, make you aware of the impact, and push you to fix them before the issue blows out of proportion in a production environment. In some cases, these practices can even help you automatically mitigate concerns by patching/updating the application infrastructure. And this is why you should care about DevSecOps.

## The Path that Led to DevSecOps

### The Waterfall Model

Now, let’s look at the path that led to DevSecOps. This should help you better understand how software development and delivery models evolved over the years.

The most popular model used for developing and delivering software resembles a waterfall and involves moving from one step to another in the following sequence:\\

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/qbei99wmghkc-WaterfallModel.png)

**The Waterfall Model: Requirements, Design, Implementation, Verification, Maintenance**

This model commonly results in longer deployment cycles, longer feedback loops, a significant amount of rework, and a slower pace of delivery overall.\\

### Agile Methodologies

Henry Ford’s assembly line sped up the process of manufacturing, which was then iterated and perfected by Toyota with the so-called[ Toyota Production System (TPS)](https://en.wikipedia.org/wiki/Toyota\_Production\_System). This system undoubtedly transformed automobile manufacturing.

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/vdms38gzq88f-IterativeDevelopment.png)

**Iterative Development**

The principles and practices that evolved as part of this system, including "just-in-time" production, continuous feedback, and improvement ([Kaizen](https://en.wikipedia.org/wiki/Kaizen)), were then adapted by the wider manufacturing industry and, later, the software industry. This led to the development of [Lean IT](https://en.wikipedia.org/wiki/Lean\_IT) and, subsequently, to the creation of the [Agile Manifesto](https://agilemanifesto.org) in 2001, which resulted in development processes being sped up significantly.

Using agile methodologies, developers were able to build software faster, in smaller chunks and in an iterative fashion, with less reworking. They were also able to fix issues and create patches much more quickly. However, traditional operations teams that had adopted the agile principles were, in fact, left out of the development loop. While developers were more concerned about the speed of delivery, for operations teams, reliability was of foremost importance, which created their reluctance toward frequent deployments, along with reduced preparedness and a lack of tooling to support the same.

### DevOps

Using agile methodologies, developers were able to build software faster, in smaller chunks and in an iterative fashion, with less reworking. They were also able to fix issues and create patches much more quickly. However, traditional operations teams that had adopted the agile principles were, in fact, left out of the development loop. While developers were more concerned about the speed of delivery, for operations teams, reliability was of foremost importance, which created their reluctance toward frequent deployments, along with reduced preparedness and a lack of tooling to support the same.\\

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/bz2clqiku5kd-DevOpsModel.png)

**DevOps Model**\
A set of principles, practices and tools which emerged to solve these problems came to be known as the DevOps practices. These ultimately resulted in the realization of the two conflicting goals of achieving faster delivery without compromising the reliability of the software. DevOps greatly relies on a set of automation tools that echo the assembly line in manufacturing. Today, DevOps represents the de facto practices used by organizations across the globe.\\

### DevSecOps

However, even with DevOps, the aspect of security remained unresolved. While you could improve the speed of deployment without compromising the reliability of the software using DevOps, the software development ended up either being slowed down due to security practices (which are implemented toward the end of the delivery pipeline) or having vulnerabilities that often leak into the production environment. DevOps could help patch these vulnerabilities quickly, but the ideal solution would have been to make the code secure without compromising the speed of delivery. This is what led to the creation of the principles, practices, and tools referred to as DevSecOps. DevSecOps is an organic extension of DevOps and follows a similar approach for implementation.

So, let us summarize why we need DevSecOps:

* DevOps enables fast deployments with high reliability.
* Traditional security cannot keep pace with it.

Therefore, the solution is to incorporate security into the DevOps practices. This approach is what we know as DevSecOps.

## What is DevSecOps?

DevSecOps commonly refers to the set of practices that are incorporated into the software delivery pipeline to provide early feedback on security risks and help you mitigate those risks before even delivering the software. These risks could be related to vulnerabilities in the actual application code, or components which make up the software, or even the infrastructure pieces used to deploy the application with.

DevSecOps attempts to embed security into the DevOps process and makes it a shared responsibility to allow deployments to be carried out at a fast pace while ensuring that the applications deployed have high levels of reliability and security.

DevSecOps tries to fill the security gaps introduced due to either:

* **Inexperience**\
  Inexperienced teams implementing DevOps may introduce manual processes that hinder the pace of deployment.
* **Immaturity**\
  Immature DevOps implementers would completely bypass security over the speed of deployment, which risks introducing vulnerabilities at scale.

DevSecOps attempts to embed security checks as part of the DevOps pPipeline instead of security being at the end of the pipeline stifling the pace of delivery.

### Implementing DevSecOps

Below is a step-by-step recipe to implement DevSecOps in your organization:

1. Adapt to **security as a culture**. Security is not the job of a specific team which is designated to perform the InfoSec role, but it becomes a collaborative effort as developers and operations teams consider it at every phase of the delivery pipeline.
2. Analyze the **layers of your application** infrastructure for risks and come up with strategies to mitigate those risks.
3. Embed security practices into the **continuous delivery** pipelines leading to the creation of DevSecOps pipelines.
4. Create dedicated, automated **SecOps pipelines** for infrastructure and operational security practices.
5. Set up **runtime security monitoring**. Introducing preventive measures along with reactive measures, such as runtime monitoring, can help you create a holistic security plan.

## Understanding Modern Applications

### Microservices Architecture

We have come a long way from applications being packaged as a single software that would perform all the functionalities required—with the ability to connect to support services such as databases—which were also commonly known as monoliths. Monoliths are difficult to maintain and scale, and are less flexible.

Modern software is typically broken down into functionalities, and each functionality is designed and deployed as an individual service (also called a microservice). For example, an e-commerce application can be broken down into functions such as the user interface, product catalog, shopping cart, payments processing, delivery tracking, etc. Each of these can operate as their own microservice, with its own databases having the ability to store its state.\\

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/w65k7m9tlw1m-13109673843\_ffb5adf371\_h.jpg)

**Microservice Architecture**, by Paul Downey, retrieved from [Flickr](https://www.flickr.com/photos/psd/13109673843), licensed under [CC BY 2.0](https://creativecommons.org/licenses/by/2.0/)

Some advantages of microservices include the following:

* **Scalability**\
  Microservices are typically stateless and are easier to scale. High availability and scalability are easier to achieve when deploying microservices.
* **Flexibility**\
  Each service can be owned, designed, and developed by its own team, and each team would have the flexibility to use the language of their choice.

### API/Queue-Based Communication

When you break down services into different components and run them as separate microservices, you also need to consider how they would communicate with each other. Modern microservices applications communicate with each other using either of the following:

* **Asynchronous messaging**\
  Using message queues with protocols such as AMQP
* **Synchronous messaging**\
  Using protocols such as HTTP/HTTPS, typically as a Representational State Transfer (REST) style architectural plan. REST applications use HTTP methods such as GET, POST, DELETE, and PUT, although not all HTTP-based APIs are RESTful. You can read this article for further information, ["What Are REST APIs? HTTP API vs REST API"](https://www.educative.io/blog/what-are-rest-apis).

Most modern applications use REST-based API communication to talk to each other.

### Packaged and Distributed as Container Images

When it comes to packaging and running microservices applications, it is best to use container images. Containers offer a lightweight subsystem to package and run applications with very little (insignificant) overload, and are designed to run one application per container. This perfectly complements the concept of microservices.

Container images are also optimized in terms of size by stripping away all the unnecessary components of the underlying root filesystem. Moreover, with the tooling available as part of the open container ecosystem, it is easy to package a microservices-based application into a container image and distribute it using a container registry such as Docker Hub.

### Deployed and Run with Kubernetes

As an extension of container runtimes and packaging tools, such as Docker, certain systems were created to orchestrate containers on a multitude of servers. These systems are typically called container orchestration engines, a popular example of which is Kubernetes.

When you want to run containers at scale in a production-like environment, Kubernetes takes all the nodes (VMs, physical, and cloud servers) that are configured as container runtimes, forms a logical entity that appears to be a single cluster to the user, and handles the launching, connecting, and managing of containers in this environment. It offers the following features, and more, which are desirable when running your application in a production environment:

* High availability
* Scalability
* Ability to dynamically route traffic (ingress)
* Ability to network between containers that run on different hosts
* Service discovery
* Load balancing
* Ability to run different workloads: stateful, stateless, CronJobs, etc.

While microservices, API-based communications, and containerization make it easy to develop and run applications efficiently and at scale, these technologies also introduce new risks that need to be understood to enable the formulation of a holistic strategy for implementing DevSecOps. Some of these risks will be discussed next.

## Application Security Risks

### What Are the Risks with Modern Applications?

Traditionally, when applications were built as monoliths and deployed in data centers, limiting physical access and setting up firewalls would provide sufficient security. With the advent of cloud computing, security measures have been expanded to further implement policies associated with virtual private clouds (VPCs), network access control (NAC), and identity and access management (IAM).

Running modern applications on the cloud—which are exposed as APIs, designed as microservices, packaged with containers, and deployed with Kubernetes—introduces new dimensions of risk. Microservices open many perimeters (for many services), have a flexible flow and are constantly/rapidly deployed, which make it even more challenging to address security issues associated with them.

Next, we will present some tales of caution that should help you put things in perspective.

### API/Application Vulnerabilities

* A researcher found a way to take over a user account of any Uber driver ([Issue 49: Uber account takeover and the leaky Get API - API Security News](https://apisecurity.io/issue-49-uber-account-takeover-leaky-get-api/))
* Researchers detected vulnerabilities with electric vehicle (EV) charging stations, using which they could obtain users' personal information and take control of the charging, possibly causing spikes in charge or mass attacks ([Issue 145: APIs and electric car charging stations, The Nuts and Bolts of OAuth 2.0 - API Security News](https://apisecurity.io/issue-145-apis-electric-car-charging-stations-nuts-bolts-oauth-2-0/))
* A vulnerability in the iPhone’s automatic call recorder application could expose users’ call recordings and sensitive information ([Issue 125: iPhone call recorder API flaw, Burp and OpenAPI, GraphQL pentesting, FAPI - API Security News](https://apisecurity.io/issue-125/)).

You can read about many such vulnerabilities at [API Security Articles, News, Vulnerabilities & Best Practices](https://apisecurity.io).

### Platform Vulnerabilities

In the 2017 Equifax breach, private information such as credit ratings, of 147 million Americans was leaked due to a known vulnerability in a third-party software, i.e. Apache Struts. The root cause for the breach was attributed to this software not being updated for a few months, despite a fix being available ([2017 Equifax data breach - Wikipedia](https://en.wikipedia.org/wiki/2017\_Equifax\_data\_breach)).

A vulnerability was discovered with runC that would allow containers to break out and gain root access on the underlying host in some cases. This affected most of the container environments running Docker and Kubernetes, with runC being the most common low-level container runtime.

Certain Kubernetes vulnerabilities were discovered in the past that could allow attacks such as denial of service (read [Top 5 Kubernetes Vulnerabilities of 2019 — the Year in Review | StackRox Community](https://www.stackrox.io/blog/top-5-kubernetes-vulnerabilities-of-2019-the-year-in-review/). Also find out the [recent Kubernetes CVEs](https://www.cvedetails.com/vulnerability-list/vendor\_id-15867/product\_id-34016/Kubernetes-Kubernetes.html)).

For more information about API security, see [API Security? Yep, everything you needed to know about that in 6,252 words | Ping Identity](https://www.pingidentity.com/en/company/blog/posts/2020/everything-need-know-api-security-2020.html).

The point here is that modern applications need a holistic and layered approach toward security and need everyone’s involvement, including that of the application developers.

From the web application security perspective, one important resource is the [Open Web Application Security Project (OWASP)](https://owasp.org), which is an online community that produces resources such as articles, methodologies, even tools and technologies in the field of application security.

## Defense-in-Depth

### Layers of an Onion Approach to Security

While analyzing DevSecOps, it is hardly useful to look at applications or infrastructure in isolation. The threat landscape changes when you add layers such as clouds or a container orchestrator. Therefore, you should examine all the layers involved and take steps to mitigate possible risks accordingly.

Consider the example of an application running in a Kubernetes environment that is hosted on the cloud, which is a common use case with cloud native applications. In such a scenario, you need to consider implementing security policies for the following layers:

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/l30bom517hfx-TheOnionApproach.png)

**The Onion Approach**

Layers to consider when it comes to security

*   Close Application Layer

    A typical API-based application is prone to many vulnerabilities (refer to [OWASP Top 10 choices](https://owasp.org/www-project-top-ten/)). Regardless of how strong your infrastructure security is, unless application developers follow best practices while writing the code, the application will be susceptible to threats, more of which are being discovered every day. In addition, you would have to consider the vulnerabilities of the components involved, such as the libraries that your application is built with, and the way in which secret information is provided to the application. Many practices that are implemented as part of the delivery pipeline, such as static analysis, dynamic analysis, and software composition analysis, can help you build applications that have a minimum number of vulnerabilities.
*   Close Container Images Layer

    While packaging applications, it is common to use base images that are available on public registries. Some of these images have vulnerabilities and lack the latest patches, and many run as root. All of these factors could render your infrastructure vulnerable to attacks. Optimizing the container images, setting up automated scans, and using best security practices (e.g., not running applications as root) can mitigate many such risks.
*   Close Container Runtime/Kernel Layer

    Container runtime misconfigurations or assigning excess privileges could lead to threats such as [container breakouts](https://attackdefense.com/listingnoauth?labtype=container-security-container-host-security\&subtype=container-security-container-host-security-breakouts). As container runtimes essentially use kernel features such as namespaces and cgroups to implement software containers, it is crucial to look into how the kernel can be secured. Using secure container runtimes and configuring security-related parameters related to the runtime and kernel would help you secure this layer.
*   Close Container Orchestration/Kubernetes Layer

    Along with the container runtime, you must consider the container orchestration engine, which most often would be Kubernetes. Kubernetes schedules containers, maintains high availability and scalability, connects applications and provides outside users with access to the same. Considering the significant sophistication of the Kubernetes system, the scope of securing it is vast and involves numerous aspects, including network policies, ingress controllers, role-based access control (RBAC), admission controllers, and dedicated tools to scan the Kubernetes configurations, as well as runtime threat detection.
*   Close Host OS Layer

    Even though containers provide a layer of isolation, it is important to mitigate the risks that the host OS may create, especially in a shared cloud-based environment, by ensuring it is compliant with standards such as [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks/), [HIPPA](https://www.hhs.gov/hipaa/index.html), etc. Infrastructure as Code tools such as Ansible can help you not only identify, but also mitigate risks by automatically and continuously updating the system configurations to match the security policy.
*   Close Cloud/VMs Infrastructure Layer

    Since we are considering cloud native applications, it is vital to consider cloud security. The cloud layer can be secured using VPCs, network ACLs, firewalls/security groups, IAM systems, etc.

## Creating and Implementing DevSecOps Pipelines

### DevSecOps Practices and Delivery Pipeline

Let’s now learn how to put it all together and create a DevSecOps pipeline. Here we assume that:

* We are dealing with a modern, cloud native application.
* This application is being packaged as a container image.
* And it is being deployed with Kubernetes.

### DevOps Pipeline

First, take a look at a simple continuous integration pipeline that you would begin with when you get started with DevOps practices. The following diagram illustrates some common components of a CI pipeline.

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/tkrqmwyjllk1-ComponentsofaCIPipeline.png)

**Components of a CI Pipeline**

The goal of this pipeline is to provide instant, continuous feedback.

Pipeline Stages

*   Close Build

    This stage notifies us whether the change in the application code breaks the build. In a collaborative environment, it is of utmost importance to ensure that the change introduced by one developer does not result in a build failure, causing everyone else’s work to be impacted, as they would not be able to test their changes due to compilation failure. This is generally the very first step in the CI pipeline. In case of an application which does not need compilation (e.g., Python, Node.js, which are interpreted languages), the build stage generally checks whether dependencies are being resolved and installed correctly.
*   Close Test

    The next stage in the continuous integration pipeline is to run unit tests. DevOps introduces the principle of shifting tests to the left side of the application delivery process. This reduces the rework, as well as the cost of detecting and mitigating issues. The earlier the problems are detected, the easier, quicker, and cheaper they are to fix.
*   Close Package

    After ensuring the code has gone through essential sanity checks, it is time to package the application into an artifact and prepare it to be distributed. Artifacts are generally pushed to a common repository, from where they are distributed further for deployment.

A typical DevOps delivery pipeline would also include the deployment stages, but this is a simplistic depiction of how you start with continuous integration.

What is clearly missing in this DevOps pipeline? The security checks! That is the whole reason why we are here, discussing DevSecOps. Let’s now look at how this pipeline can evolve into something that we can call a DevSecOps pipeline.

### Components of a DevSecOps Pipeline

Let’s say we took the same pipeline described in the previous subsection, ran some security magic, and converted it into a DevSecOps pipeline. How would it look like?

The following diagram depicts exactly such a pipeline that focuses on security at every stage of the delivery process.

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/j6rrzwephxjv-DevSecOpsPipelineVisualized.png)

**DevSecOps Pipeline Visualized**

Let’s analyze the additional security-related components of this pipeline. This is going to help you understand common DevSecOps practices.

### DevSecOps Practices

Common Security Practices to Embed into a DevSecOps Pipeline

*   Close Secrets Scanning

    It is important to ensure that no sensitive information, such as passwords, API keys, and cloud access credentials, are accidentally checked into the repository. Secrets scanners can be either run as pre-commit hooks (checks which are automatically run when a developer tries to commit to the repository) or can be enforced to run as the first step in the continuous integration pipeline, or both.
*   Close Software Composition Analysis (SCA)

    A majority of API-based application software today uses open source, third-party components, including libraries and frameworks. This forms your supply chain. How secure are the components incorporated in your software? Are there any vulnerabilities that you must patch? What are the security implications of using these components? Software Composition Analysis (SCA) attempts to detect such risks and provides you with inputs on how to mitigate them.
*   Close Software Bill of Materials (SBOM)

    This relates to Software Component Analysis. Standards such as CycloneDX help you generate a list of components akin to a bill of materials and reports on which components are in use; with respective versions, licenses, known vulnerabilities, etc. This helps add transparency, as well as aids understanding of the security posture of your application infrastructure.
*   Close Static Application Security Testing (SAST)

    The SAST stage helps you scan your application code and compare it against best practices, as well as detect any vulnerabilities in the code. This is a form of white box testing, whereas the tool being utilized has to be aware of the programming language being used and scan the code according to that. Like running unit tests, the goal here is to shift tests right, in this case within the context of security issues. The earlier the code issues are detected, the cheaper and faster it is to mitigate them.
*   Close OSS License Checker

    This stage is run as part of the DevSecOps pipeline to automatically detect the licenses for the components in use and compare it against the list of allowed licenses. As new components are added or existing components updated, there is a possibility of license terms being updated or new licenses being incorporated. This may have legal implications and can be incompatible with the organization’s policies. Having an automated process to detect such an event as part of the delivery pipeline is extremely useful.
*   Close Container Image Linting and Scanning

    As increasingly more applications are being packaged and deployed as containers, what gets packaged along with your container as part of the artifact becomes an important security consideration. It is very common to use a base image from the registry which is outdated, unpatched and introduces many vulnerabilities. It is also a good idea to check if you are following best practices while building the container image itself. Container image linting and scanning provides the feedback on both aspects: how safe is the image along with all the packages in it, as well as how well is your Dockerfile written to build that image. Running these scans as part of an automated pipeline can be very handy.
*   Close GitOps-Based Secure Deployment

    A commonly ignored part in the delivery process is deployment security. It is common to see a continuous integration platform such as Jenkins being provided with access to deployment systems with wider authorization, as well as to deployment systems provided to a wider audience. GitOps could help separate the deployment process from development and CI processes, keep the deployment code separate, and, at the same time, provide very limited access to systems such as Jenkins to trigger deployments only to limited applications. Since GitOps is built around Git-based workflows with support for pull requests, branching models, code reviews, etc., only the approved code is deployed to an environment.
*   Close Dynamic Application Security Testing (DAST)

    While SAST is a white box testing methodology, DAST takes a black box testing approach to detect security issues. Dynamic Application Security Testing (DAST) typically refers to automated penetration testing of web applications to detect issues wider than those present just in the application code. There could be multiple components which can form a chain of connections between a user and the application, which can be possibly exploited. This includes reverse proxies, network applications, caching services, databases, etc. To provide a holistic view of your application’s security posture, it is important to take an outsider’s view and simulate external attacks in addition to taking an insider’s view with SAST. DAST is run after the application has been deployed in a Dev/QA environment.

While the practices described above form the core of the DevSecOps pipeline, it goes beyond this. DevSecOps is about taking a holistic approach and embedding security into all development and operational practices. This leads to additional processes, such as creating daily SecOps pipeline runs with compliance scans, using IDE plugins that detect security vulnerabilities, etc. True DevSecOps is about trying to follow the defense-in-depth approach. And despite following all these practices, it is important to acknowledge that you cannot guarantee 100% security, but the attempt is to get closer to it. And it is an iterative, continuous process!

### How to Choose the Right Tools to Implement DevSecOps?

So far, you learned why DevSecOps is needed and explored the risks related to modern applications. By now, you also hopefully came to an agreement that including key DevSecOps practices is an important aspect of the delivery pipeline. Now, you should consider the following two questions: _"Which tools should you use?"_ and _"How should you choose them?"_. While there are many commercial as well as open source solutions for each of the practices we discussed while visualizing the DevSecOps pipeline, there are common criteria that you can use to shortlist the tools.

Tools included in the DevSecOps pipeline should be...

*   Close Automatable

    They should have support for API-based communication or, at a minimum, a command line interface which can be used to script tasks.
*   Close Containerized

    They should support being packaged as a container application. This enables them to be integrated with the continuous integration systems which often run projects with container-based agents.
*   Close Free from licensing limitations

    Many commercial applications have limitations on the number of scans, number of parallel scans, etc.
*   Close Configurable

    This is helpful to counter false negatives/false positives.

Almost all tools recommended as part of this course are open source and available to use for free. However, you have to select the tools based on your application stack, organization goals, desired features, and availability of resources to maintain the tools.

## Additional Resources

Learn more...

* Read the ["AWS-DevSecOps\_Factory"](https://github.com/jonrau1/AWS-DevSecOps-Factory) article on GitHub
* Check out the [OWASP API Security Top 10](https://owasp.org/www-project-api-security/) list
* Read the ["Everything You Need to Know about API Security in 2021"](https://www.pingidentity.com/en/company/blog/posts/2020/everything-need-know-api-security-2020.html) article, by Bernard Harguindeguy
* Read the ["Shift Left with AWS Codepipeline - Scanning every single code change"](https://logicbomb.medium.com/shift-left-with-aws-codepipleine-scanning-every-single-code-change-273bd74f32a2) article, by Avinash Jain
* Watch the videos available in the [DevSecOps Learning playlist](https://www.youtube.com/playlist?list=PLQvhFTnS5hOIwcgRn7q2ctoqGbiE8Lhri) by Carnegie Mellon
