# What is SAST?

Static Application Security Testing (SAST) is a methodology used to scan your application source code as part of the continuous integration process. The objective is to find vulnerabilities early in the continuous delivery process. This minimizes the number and impact of issues that occur after the code is deployed and helps reduce expenses.

Since SAST scans use white-box testing, the tool involved must be aware of the programming language and scan the code with rules specific to that language. You can also set up some sort of project-gating system that would fail the build step and stop the delivery pipeline when issues that cross the pre-configured security threshold are detected.

It is important to define an acceptable baseline while using SAST applications, as it is almost always impossible to start with 100% clean code. A good strategy is to start with a baseline and iteratively work toward:

* Optimizing the code to incorporate better security practices.
* Configuring the tool to identify false positives.

The iterative and adaptive strategy works best when you are working on a project with a significant amount of code, which is difficult to update right when you are getting started with DevSecOps practices.

In the next lessons, you are going to learn how to use a simple yet powerful open source scanner called [Scan](https://slscan.io/en/latest/), which goes beyond SAST and can run SCA, SBOM and license checks. It is a very interesting and useful tool to learn about! Let’s get it set up and start scanning.

### What is SAST?

### Using SCAN (slscan.io)

### Adding the SAST Stage to the Pipeline

### Configuring SCA to Fail

### Fixing Dependency Issues

### Updating the License Approval List
