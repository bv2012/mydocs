# Setting up a Highly Available Jenkins Master On-Premise

A minimal high availability setup includes the following:

* **Two VM/bare-metal based Jenkins masters in an active-passive mode**\
  Jenkins does not support an active-active setup. Only one Jenkins master can be active at any given time.
* **A shared storage (NAS/SAN) for the JENKINS\_HOME directory**\
  Jenkins writes all of the data, job configurations, workspaces, build history, plugins, secrets and much more in the **JENKINS\_HOME** directory. In the event of a failure, the secondary Jenkins server, which is in standby mode, takes over as primary. However, it needs to access the **JENKINS\_HOME** directory in order to be fully functional. If you store your **JENKINS\_HOME** data on a shared storage, this can be easily mounted onto any other server. Besides, shared storage has the additional benefits of backups, and data can be restored as needed.
* **A reverse proxy such as NGINX, HAProxy, F5, Traefik, etc., which fronts the Jenkins masters**\
  The proxy will continuously monitor the Jenkins primary server. If it detects any issue with the primary Jenkins master, it starts routing all the requests to the secondary Jenkins master. For anyone accessing the Jenkins User Interface or running CLI/API tasks, the hostname of the Jenkins server remains the same as they always point to the reverse proxy server, not the individual Jenkins masters. It is worth noting that with distributed builds, your JNLP agents use a binary protocol so they cannot be reverse-proxied reliably through NGINX, HAProxy, etc. For Jenkins versions 2.2.17 and above it is recommended that you use [-webSocket](https://www.jenkins.io/blog/2020/02/02/web-socket/) mode while launching inbound agents instead of -http. This way you can avoid problems with reverse proxies.

&#x20;

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/z90s8bvojtfy-LFS267\_CourseGraphics-02.png)

**Figure: Jenkins High Availability Architecture**

&#x20;

You can leverage some monitoring tools to proactively monitor Jenkins masters and set alerts based on certain thresholds. In case of complete failure of your primary Jenkins master, you will need to failover to the secondary server. But what if your secondary master fails? You will need to replace it with another Jenkins master and so on. The question is how quickly your organization can provision a new infrastructure.

With the advent of Cloud, and quick provisioning times, setting up high availability in Cloud is a lot easier. It is a rather straightforward task if you are using a third party cloud provider.

Let’s take a look at how to set up Jenkins HA (self-healing and highly available) on public cloud, AWS (Amazon Web Services) as an example.
