# ChatOps with Jenkins X

Jenkins X is based on the GitOps set of practices for the Kubernetes cluster management—every change must be recorded in Git and Git should initiate events that change the cluster. The developers communicate with each other through Git pull requests (PR) comments, making PRs the cornerstone of Jenkins X pipelines. ChatOps, however, takes that communication to the next level. With ChatOps, developers can type commands in chat windows resulting in automation runs. For example, typing “/approve” in the PR chat window will result in approving and auto-merging of a pull request.

Jenkins X lets you assign collaborators for a project. It automatically creates a file called **OWNERS** as soon as you import or create a new Jenkins X project.

**approvers:**\
**- hgautam**\
**reviewers:**\
**- hgautam**

As you can see, this file is divided into two sections - **approvers** and **reviewers**. You can add multiple collaborators to this file and they can use ChatOps commands to approve and run automatic workflows. The currently supported ChatOps commands are listed in the [Jenkins X Documentation](https://jenkins-x.io/v3/develop/reference/chatops/).
