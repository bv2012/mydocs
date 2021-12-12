# AUDITING CONTAINER IMAGES

## Building Secure Container Images

As container-based software delivery becomes the norm, it is important to ensure that what has been packaged into a container image does not introduce vulnerabilities that are open to exploitation. Container images contain not only the application, but also the environment in which the application is configured to run, including libraries, dependencies, and even operating system files.

Container Image Linting and Scanning

*   Linting images/Dockerfile

    It is always advisable to check if the container images are built using [best practices](https://docs.docker.com/develop/develop-images/dockerfile\_best-practices/). There are different approaches to do that. You can lint the Dockerfile before building the image, or you can lint an image after it is built by scanning the metadata, etc. You can also use Hadolint for linting the Dockerfile versus Dockle or a similar tool for scanning the image. Here is a [comparison between Dockle and Hadolint](https://github.com/goodwithtech/dockle).
*   Vulnerability scanning

    It is common practice to use an outdated, old, large base image from a container registry such as DockerHub without actually understanding the implications. You would be surprised to see how many vulnerabilities an unoptimized container image can introduce. How do you identify risk with your base image? Have it scanned regularly using tools such as [Trivy](https://github.com/aquasecurity/trivy).

In this chapter, you are going to begin by adding Dockle and Trivy to run automated analyses as part of your CI/CD pipelines. Once you start identifying issues with the existing images, you will also learn how to build an optimized container image.

\


