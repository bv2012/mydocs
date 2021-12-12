# Secure Deployments with GitOps

## rinciples of GitOps

GitOps is commonly referred to as a set of principles and related practices that define how to set up an automated continuous delivery using Git-based well-established workflows.

\


![The Four Principles of GitOps](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/g5meuhjblxi5-The4principlesofGitOps.png)

GitOps Principles

*   Define everything as code

    You start implementing GitOps by first writing everything as code. Defining everything as code goes beyond just the application code and includes writing configurations, infrastructure provisioning, and even application deployment strategies. With a platform such as Kubernetes, which exposes everything as Application Programming Interface (API) objects, it is possible to define everything as code, including deployments, configurations, network policies, user access controls, etc.
*   &#x20;Store desired state in Git

    Once you start defining everything as code, the next logical step is to store that code in a Git repository and start tracking changes to it via its revision control features. This needs no further elaboration.
*   Apply approved changes automatically

    Developers have already established well-defined practices and workflows based on Git. This includes implementing branching models (e.g., trunk-based development) and defining branch protection rules. This helps define a workflow that involves locking down the main line of code, using feature branches, running automated continuous integration (CI) on every change, bringing changes in by raising pull requests, etc. With pull requests, there is a way to set up review and approval processes as well. What this principle intends to convey is to leverage Git-based workflows and enforce that each change goes through set-up checks and approval process. As a result, what gets merged into the main/release branch is considered stable enough. Set up an automated process to apply changes to such a branch automatically.
*   Check and correct with a software agent

    This is a continuation of principle #3. Approved changes need to be applied automatically. How does one do so? Start by setting up an agent; in the context of Kubernetes, this agent typically sits inside Kubernetes (deployed as a pod in Kubernetes), and watches for any changes to the specific branches of the Git repository that stores the deployment code. Upon detecting the change, the agent automatically syncs those with the cluster. When it does, it compares the desired state in Git with the actual state of infrastructure and takes pre-configured corrective actions. This is where tools such as FluxCD or ArgoCD fit in.

\
