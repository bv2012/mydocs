# Secure Deployments with GitOps



### GitOps-based Deployment Configurations

*   Immutability

    GitOps-based deployments typically lead to immutable infrastructure deployments. Immutable deployments refer to deleting the previous instances of containers and replacing those with the new ones. Immutable deployments are inherently secure and consistent, as there is no tampering of environment or manual changes.
*   Single source of truth

    With GitOps-based workflows, the only source of truth is the Git repository. There are no manual changes, and there are no situations where different team members are applying changes. This helps to streamline the process.
*   Increased developer velocity

    An important security aspect is being able to respond in case of a security breach. Mean time to recovery (MTTR) is an important metric that defines how fast one can respond. With GitOps-based deployments, you can improve MTTR and respond to issues quickly, resulting in increased developer and deployment velocity.
*   Separation of concerns

    With GitOps-based implementation, you can keep the deployment process out of sight for developers who may have control over the continuous integration pipelines. GitOps allows separation of concerns by keeping deployment code in its own repository, and also by limiting access to the deployment process only to entities or users explicitly authorized to deploy.

### In essence, GitOps improves....

* Security of code and what is inside the code by keeping deployment code...
  * Declarative and absolute
  * Easy to diff (**git diff**) and audit
* How changes are made to code...
  * By using versioning to keep commit history, which can help generate an audit trail
  * By using signed commits to Git, which can lead to accountability and traceability
* How changes are made to production...
  * By providing only one way to deploy and one place from which to audit and enforce policies
