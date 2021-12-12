# Defence-in-Depth

## Layers of an Onion Approach to Security

While analyzing DevSecOps, it is hardly useful to look at applications or infrastructure in isolation. The threat landscape changes when you add layers such as clouds or a container orchestrator. Therefore, you should examine all the layers involved and take steps to mitigate possible risks accordingly.

Consider the example of an application running in a Kubernetes environment that is hosted on the cloud, which is a common use case with cloud native applications. In such a scenario, you need to consider implementing security policies for the following layers:

![](<../../.gitbook/assets/image (2).png>)**The Onion Approach**

****

Layers to consider when it comes to security

*   Application Layer

    A typical API-based application is prone to many vulnerabilities (refer to [OWASP Top 10 choices](https://owasp.org/www-project-top-ten/)). Regardless of how strong your infrastructure security is, unless application developers follow best practices while writing the code, the application will be susceptible to threats, more of which are being discovered every day. In addition, you would have to consider the vulnerabilities of the components involved, such as the libraries that your application is built with, and the way in which secret information is provided to the application. Many practices that are implemented as part of the delivery pipeline, such as  static analysis, dynamic analysis, and software composition analysis, can help you build applications that have a minimum number of vulnerabilities.
*   Container Images Layer

    While packaging applications, it is common to use base images that are available on public registries. Some of these images have vulnerabilities and lack the latest patches, and many run as root. All of these factors could render your infrastructure vulnerable to attacks. Optimizing the container images, setting up automated scans, and using best security practices (e.g., not running applications as root) can mitigate many such risks.
*   Container Runtime/Kernel Layer

    Container runtime misconfigurations or assigning excess privileges could lead to threats such as [container breakouts](https://attackdefense.com/listingnoauth?labtype=container-security-container-host-security\&subtype=container-security-container-host-security-breakouts). As container runtimes essentially use kernel features such as namespaces and cgroups to implement software containers, it is crucial to look into how the kernel can be secured. Using secure container runtimes and configuring security-related parameters related to the runtime and kernel would help you secure this layer.
*   Container Orchestration/Kubernetes Layer

    Along with the container runtime, you must consider the container orchestration engine, which most often would be Kubernetes. Kubernetes schedules containers, maintains high availability and scalability, connects applications and provides outside users with access to the same. Considering the significant sophistication of the Kubernetes system, the scope of securing it is vast and involves numerous aspects, including network policies, ingress controllers, role-based access control (RBAC), admission controllers, and dedicated tools to scan the Kubernetes configurations, as well as runtime threat detection.
*   Host OS Layer

    Even though containers provide a layer of isolation, it is important to mitigate the risks that the host OS may create, especially in a shared cloud-based environment, by ensuring it is compliant with standards such as [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks/), [HIPPA](https://www.hhs.gov/hipaa/index.html), etc. Infrastructure as Code tools such as Ansible can help you not only identify, but also mitigate risks by automatically and continuously updating the system configurations to match the security policy.
*   Cloud/VMs Infrastructure Layer

    Since we are considering cloud native applications, it is vital to consider cloud security. The cloud layer can be secured using VPCs, network ACLs, firewalls/security groups, IAM systems, etc.
