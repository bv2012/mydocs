# Using jx project import

What if you already have a working application and want to run CI/CD with Jenkins X? Jenkins X provides a quick way for that too—you can import an existing source code into Jenkins X. Here is how you can go about it:

**cd my-app**\
**jx project import**

**jx project import** will execute the following steps for you:

* Add your source code to a Git repository if it is not already added.
* Create a remote Git repository on a Git server like GitHub.
* Push your source code to the remote repository.
* Add any required configuration files to your project including:\
  \- A **Dockerfile**.\
  \- Tekton steps and triggers to create a pipeline.\
  \- A Helm chart.
* Register a webhook on your remote repository.
* Trigger the first run of your CI/CD pipeline.

You can also import projects that are already hosted on a remote repository. You will need to pass the **--url** argument like this:

**jx project import --url htt‌ps://github.com//gitHubRepo.git**

You can also import hosted projects under a GitHub organization. Here is how to do this:

**jx project import --github --org myname**

This command will prompt you to choose repositories from **myname** GitHub organization. Alternatively, you can import all the repositories of an organization by using the following command:

**jx project import --github --org myname --all**
