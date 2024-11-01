# Labs

## Lab 1: Setting Up GitLab CI for Continuous Integration
**Objective:** Learn to configure GitLab CI to automate the build and testing process of a Spring Boot application.

**Instructions:**
- Create a new Spring Boot project and push it to a GitLab repository.
- Define a `.gitlab-ci.yml` file in the repository to specify the CI/CD pipeline.
- Include stages for build, test, and deployment using appropriate GitLab CI runners.
- Implement unit tests in the application and configure the CI pipeline to run these tests automatically on each commit.

**Expected Outcomes:**
- Students will understand how to set up and configure GitLab CI for continuous integration.
- They will gain experience in automating builds and testing workflows.

## Lab 2: Implementing Security with GitGuardian
**Objective:** Use GitGuardian to detect sensitive data leaks in a Git repository.

**Instructions:**
- Create a sample Git repository and intentionally add sensitive information (e.g., API keys, passwords) in the codebase.
- Configure GitGuardian to monitor the repository for sensitive data leaks.
- Review the alerts generated by GitGuardian and understand the implications of leaking sensitive information.
- Remove the sensitive data from the repository and implement pre-commit hooks to prevent future leaks.

**Expected Outcomes:**
- Students will learn how to use GitGuardian to enhance security in their Git repositories.
- They will understand the importance of preventing sensitive data exposure.

## Lab 3: Static Code Analysis with SemGrep and Container Security with Trivy
**Objective:** Implement static code analysis with SemGrep and container security scanning with Trivy.

**Instructions:**
- Install SemGrep and create a configuration file with rules to identify potential security vulnerabilities in the Spring Boot application.
- Run SemGrep against the codebase to identify vulnerabilities and analyze the results.
- Create a Docker image of the Spring Boot application and use Trivy to scan the image for known vulnerabilities.
- Review the findings from Trivy and implement necessary fixes in the application and Dockerfile.

**Expected Outcomes:**
- Students will understand how to perform static code analysis using SemGrep.
- They will learn to scan container images for vulnerabilities using Trivy.

## Lab 4: Deploying Applications on Google Cloud VM
**Objective:** Deploy a Spring Boot application on a Google Cloud Virtual Machine (VM).

**Instructions:**
- Create a Google Cloud account and set up a new VM instance using the Google Cloud Console.
- Install Java and Docker on the VM.
- Build a Docker image of the Spring Boot application and push it to Google Container Registry.
- Pull the Docker image on the VM and run the application, ensuring it is accessible via the public IP address.

**Expected Outcomes:**
- Students will learn how to deploy applications on Google Cloud VMs.
- They will gain practical experience in setting up and managing cloud infrastructure for hosting applications.

# Questions
1. What are the key principles of DevOps, and how do they enhance software development and delivery?
2. Explain the role of Continuous Integration (CI) in the DevOps lifecycle.
3. What is GitLab CI, and how does it facilitate automation in software development?
4. Describe the process of defining a CI/CD pipeline in GitLab using the `.gitlab-ci.yml` file.
5. How does GitGuardian help in preventing sensitive data leaks in Git repositories?
6. Discuss the importance of pre-commit hooks in maintaining code security and quality.
7. What is SemGrep, and how does it contribute to identifying vulnerabilities in code?
8. Explain the significance of container security scanning and the role of Trivy in this context.
9. What are the benefits of deploying applications on Google Cloud VMs compared to traditional hosting solutions?
10. Discuss best practices for integrating security into the DevOps workflow (DevSecOps) and the tools that can be utilized.
