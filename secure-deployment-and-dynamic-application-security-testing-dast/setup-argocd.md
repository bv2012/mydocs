# Setup ArgoCD

### Setting Up ArgoCD

### Configuring the ArgoCD CLI

### Kubernetes Deployment Objects

### Generating Kubernetes YAML Manifests

### Deploying to Kubernetes with ArgoCD

### Authorizing Jenkins to Deploy Remotely with Argo

### Adding an Automated Deploy Stage to the Jenkins Pipeline

### What Is Dynamic Application Security Testing (DAST)?

While Static Application Security Testing (SAST) takes a white-box approach toward scanning code and analyzing it for security issues, on the other end of the spectrum lies Dynamic Application Security Testing (DAST), which is a black-box approach to security testing. DAST is a form of penetration testing.

DAST is an application-agnostic methodology that assumes an outsiders’ role to scan an application and find vulnerabilities just like an attacker would. It goes beyond the application and tries to expose issues with larger attack surfaces including reverse proxies, load balancers, caching services, databases and more.

Setting up an automated DAST along with SAST before you deploy an application can give you a holistic picture of how secure the code is, as well as its security posture after deploying it.

[\
](https://trainingportal.linuxfoundation.org/learn/course/implementing-devsecops-lfs262/secure-deployment-and-dynamic-application-security-testing-dast/secure-deployment-and-dast?page=9)

### What Is OWASP ZAP?

Zed Attack Proxy (ZAP) is an open source web application security scanner/penetration testing tool developed by OWASP. It can be invoked as part of a CI/CD pipeline to set up automated scanning of an actual environment (e.g., development or staging environment) to scan for vulnerabilities in the web application setup. ZAP is probably the most popular out of all DAST scanners, it is widely used and actively maintained.

Some of the key features of ZAP include:

* Man-in-the-middle proxy
* AJAX web crawling/spidering
* Automated scanning (e.g., Jenkins plugin)
* Passive scanning support
* Fuzzing (customized payloads for comprehensive analysis)
* Scripting/plugin support.

In the next lesson, you are going to explore how OWASP ZAP works by installing it locally and invoking it as part of a CI process to automatically run DAST scans.

[\
](https://trainingportal.linuxfoundation.org/learn/course/implementing-devsecops-lfs262/secure-deployment-and-dynamic-application-security-testing-dast/secure-deployment-and-dast?page=10)

### DAST Scan with ZAP
