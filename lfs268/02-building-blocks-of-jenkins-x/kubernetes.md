# Kubernetes

[Kubernetes](https://kubernetes.io) is an orchestration framework for running a set of containers. As a cluster manager, it strives to maintain the state of a cluster. This includes ensuring that the required number of resources is always running inside a cluster. Kubernetes state management is a declarative process achieved through configuration supplied via YAML or JSON files. These specification files are referred to as Kubernetes manifests or specs.

&#x20;

![Kubernetes logo](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/hnnkvgcthhwm-kubernetes-horizontal-color.png)

&#x20;

Kubernetes uses a declarative model for cluster management. It diligently works to ensure that the set and state of objects running inside the cluster matches with the set and state of objects defined in the manifest. Kubernetes controllers help watch the state of a cluster then make or request changes if needed. Kubernetes uses objects to abstract the state of a cluster. A full-blown discussion of Kubernetes is outside the scope of this class. Please see [Kubernetes Documentation](https://kubernetes.io/docs/home/) to learn more about it.

Kubernetes Objects

*   Pods

    In Kubernetes, a Pod is the smallest deployment unit that represents the number of processes running inside a cluster. A Pod includes an application container, storage resources, network identity, and options that govern a container runtime. A Pod can run one or many containers. Docker is the most common container runtime used in a Kubernetes Pod.
*   Deployment

    A Deployment provides a way of creating Pods via declarative manifests. You define the requirements of your application state in a YAML file and use Kubernetes API to communicate the state to the deployment controller. The controller creates Pods as defined by your configuration.
*   Services

    Applications running inside a Pod are not visible outside of a Kubernetes cluster. Services provide a way to expose these applications to the outside world. Besides, Kubernetes Pods are ephemeral. Old Pods are removed and new ones are created frequently. Services ensure that users can reach newly created Pods without making any config changes on the cluster. In this sense, Services are responsible for continuous access to applications running inside a Kubernetes cluster.
*   Namespaces

    Kubernetes supports running multiple virtual clusters on a single physical cluster. Namespaces provide this functionality and are intended for multi-user environments. They provide ways to divide cluster resources between multiple teams.
*   Volumes

    Kubernetes Pods are ephemeral but the data used by them may need to be persisted for a longer period of time. Also, containers in the Pod might need to share the data. Kubernetes Volumes help manage the data needs of the Pods. Kubernetes supports multiple volume types. To learn more about Kubernetes Volumes see [Kubernetes Documentation](https://kubernetes.io/docs/concepts/storage/volumes/#background).
*   Ingress

    Kubernetes Ingress exposes HTTP and HTTPS routes from outside the cluster to services running inside the cluster. It can also be used for load balancing, terminating SSL/TLS, and name-based virtual hosting.

Jenkins X tools run as Kubernetes pods inside a Kubernetes cluster and work together to provide a CI/CD workflow. Setting and using Jenkins X does not require extensive knowledge of Kubernetes. However, having a solid grasp of it can be very useful as you try to further extend and scale your Jenkins X cluster.
