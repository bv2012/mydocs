# Key Practices to Mitigate Risks and Ensure Safety in the Supply Chain

Next, we will discuss some measures (key practices) that can be introduced to mitigate these risks and ensure the safety of the supply chain.

*   Close Solution 1: SCA

    It is a good idea to run static scans not only on your own code, but also on all the components that your software uses. You may want to scan your code to perform:

    1. **Vulnerability analysis**\
       This would help detect common vulnerabilities, which can then be followed by patching/upgrading of such components.
    2. **Outdated component analysis**\
       This is helpful to ensure that the libraries that you are using are well maintained and kept up-to-date.
    3. **Pedigree/Provenance analysis**\
       It is important to know who wrote the software and its ancestry. Changes made to your components should be traceable and auditable.

    _**NOTE:**_\
    _Static analysis of libraries is best thought of as providing hints regarding where security vulnerabilities might be located in the code. It is **not** a replacement for human expertise._
*   Close Solution 2: SBOM

    Generating a list of components and their details, such as versions, licenses, etc., helps build transparency. An SBOM is similar to the ingredients in a recipe. Some of the common standards for generating SBOM include:

    * [OWASP CycloneDX](https://cyclonedx.org)
    * [International Open Standard (ISO/IEC 5962:2021) - The Software Package Data Exchange (SPDX)](https://spdx.dev)

    One tool that could help you generate an SBOM is the [OWASP Dependency-Track](https://owasp.org/www-project-dependency-track/).

    To learn more about CycloneDX, see the following two articles: [CycloneDX Use Cases](https://cyclonedx.org/use-cases/) and [Guiding Principles](https://cyclonedx.org/about/guiding-principles/).
*   Close Solution 3: Analyze Component Licenses

    It is important to know the licenses under which the components on which your application is dependent are published. Ignoring this could lead to legal implications for your organization and may conflict with the organizational goals and strategies. Moreover, the component licenses may get updated over time, making it important to define an allowed list of licenses acceptable to your organization and scan all the components regularly to ensure that no component violates the organization’s policy.
*   Close Solution 4: Define an Open Source Policy

    You must define and follow a clear policy on the inclusion and usage of open source software components. Some rules that you should consider include:

    * Running regular vulnerability scans (no critical CVEs).
    * Limiting the age of components (e.g., no older than three years).
    * Removing unnecessary dependencies.
    * Ensuring the use of high quality components.
    * Automating patching and updates.
    * Dedicating developer time for component management and updates.

Key Practices
