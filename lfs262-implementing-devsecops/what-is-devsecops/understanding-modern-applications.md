# Understanding Modern Applications

### Microservices Architecture

We have come a long way from applications being packaged as a single software that would perform all the functionalities required—with the ability to connect to support services such as databases—which were also commonly known as monoliths. Monoliths are difficult to maintain and scale, and are less flexible.&#x20;

Modern software is typically broken down into functionalities, and each functionality is designed and deployed as an individual service (also called a microservice). For example, an e-commerce application can be broken down into functions such as the user interface, product catalog, shopping cart, payments processing, delivery tracking, etc. Each of these can operate as their own microservice, with its own databases having the ability to store its state.\


![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/w65k7m9tlw1m-13109673843\_ffb5adf371\_h.jpg)**Microservice Architecture**, by Paul Downey, retrieved from [Flickr](https://www.flickr.com/photos/psd/13109673843), licensed under [CC BY 2.0](https://creativecommons.org/licenses/by/2.0/)



Some advantages of microservices include the following:&#x20;

* **Scalability**\
  Microservices are typically stateless and are easier to scale. High availability and scalability are easier to achieve when deploying microservices.&#x20;
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
