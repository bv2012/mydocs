# Software Component Analysis Overview



## Objectives

***

By the end of this chapter, you should be able to:

* Explain what Software Component Analysis (SCA) is, why it is needed, and the measures you can take to implement comprehensive SCA.
* Add SCA with dependency checker.
* Scan components for licenses and check them against an acceptable allowlist.
* Generate the Software Bill of Materials (SBOM) and send the reports to the Open Web Application Security Project (OWASP) dependency tracker.

## What Is SCA?

In the modern-day and age, what you run as an application is not entirely written by you. With the evolution of open source software development and the use of frameworks and libraries, a significant portion of the codebase is from a third-party that forms part of the supply chain. This is where Software Component Analysis (SCA) comes into play.

SCA refers to the methodology in which all components that make your software are analyzed for vulnerabilities and incompatible licenses. SCA also allows you to generate a bill of materials. This is helpful in understanding what your software consists of, as well as in providing a holistic risk profile.



## SCA Application - An Example

To better illustrate SCA, let’s take a look at the previously mentioned [Equifax data breach](https://www.csoonline.com/article/3444488/equifax-data-breach-faq-what-happened-who-was-affected-what-was-the-impact.html) that happened due to the Apache Struts vulnerability and explore possible solutions.



Equifax Data Breach Example

* What is the problem?
  * Eighty percent of your application is made of components such as open source third-party libraries and frameworks.
  * These components allow for a shorter time to reach the market, increase efficiency, provide reusability, and are relatively more secure, as the code is being used widely, providing for faster detection and patching of these issues.
  * However, many of these libraries are not updated or maintained, and have known vulnerabilities that are well-published and open to exploitation.
  * Most organizations have no policy to control incorporation of components, update those components or resources allocated for application code changes on components being upgraded.
  * Many open source applications are published under licenses which can conflict with the organization’s goals and cause serious problems.
* Example vulnerability: Struts remote code execution
  * [Apache Struts 2](https://en.wikipedia.org/wiki/Apache\_Struts\_2) is a popular web framework, a new avatar of Struts Framework.
  * Struts 2 was downloaded 1 million times by over 18,000 organizations in a year.
  * A vulnerability in the library was found to be exploited, which could allow any arbitrary code to be executed on a Struts 2 web application.
*   What is the solution?

    Ensuring your supply chain is safe requires a multi-pronged approach. Common risk factors related to the supply chain include:

    1. **Vulnerable components**\
       If components have critical security vulnerabilities, they are going to impact your environment, resulting in security breaches, possibility of important data being stolen, etc.
    2. **Outdated components**\
       Many of the well-known open source libraries, frameworks, etc., have [Common Vulnerabilities and Exposures (CVEs)](https://en.wikipedia.org/wiki/Common\_Vulnerabilities\_and\_Exposures) that are open for anyone to explore and exploit. If any vulnerabilities are exposed, they are instantly patched, keeping these libraries and frameworks up-to-date. If there is no update policy in place, the software becomes susceptible to threats and attacks.
    3. **Components with incompatible licenses**\
       Using components with incompatible licenses may have legal as well as financial consequences for an organization. Thus, it is important to have a policy that defines a list of allowed licenses.
    4. **Provenance and traceability**\
       Components with no auditable history of changes or with unknown authors can lead to various issues such as malicious code pretending to be a useful library getting incorporated into your software. That’s why it is imperative to be able to always trace and audit changes to the components.

##

\
