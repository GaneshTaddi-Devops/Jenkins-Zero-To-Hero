# Jenkins Pipeline for Java based application using Maven, SonarQube, Argo CD, Helm and Kubernetes

![Screenshot 2023-03-28 at 9 38 09 PM](https://user-images.githubusercontent.com/43399466/228301952-abc02ca2-9942-4a67-8293-f76647b6f9d8.png)


Here are the step-by-step details to set up an end-to-end Jenkins pipeline for a Java application using SonarQube, Argo CD, Helm, and Kubernetes:

Prerequisites:

   -  Java application code hosted on a Git repository
   -   Jenkins server
   -  Kubernetes cluster
   -  Helm package manager
   -  Argo CD

Steps:

    1. Install the necessary Jenkins plugins:
       1.1 Git plugin
       1.2 Maven Integration plugin
       1.3 Pipeline plugin
       1.4 Kubernetes Continuous Deploy plugin

    2. Create a new Jenkins pipeline:
       2.1 In Jenkins, create a new pipeline job and configure it with the Git repository URL for the Java application.
       2.2 Add a Jenkinsfile to the Git repository to define the pipeline stages.

    3. Define the pipeline stages:
        Stage 1: Checkout the source code from Git.
        Stage 2: Build the Java application using Maven.
        Stage 3: Run unit tests using JUnit and Mockito.
        Stage 4: Run SonarQube analysis to check the code quality.
        Stage 5: Package the application into a JAR file.
        Stage 6: Deploy the application to a test environment using Helm.
        Stage 7: Run user acceptance tests on the deployed application.
        Stage 8: Promote the application to a production environment using Argo CD.

    4. Configure Jenkins pipeline stages:
        Stage 1: Use the Git plugin to check out the source code from the Git repository.
        Stage 2: Use the Maven Integration plugin to build the Java application.
        Stage 3: Use the JUnit and Mockito plugins to run unit tests.
        Stage 4: Use the SonarQube plugin to analyze the code quality of the Java application.
        Stage 5: Use the Maven Integration plugin to package the application into a JAR file.
        Stage 6: Use the Kubernetes Continuous Deploy plugin to deploy the application to a test environment using Helm.
        Stage 7: Use a testing framework like Selenium to run user acceptance tests on the deployed application.
        Stage 8: Use Argo CD to promote the application to a production environment.

    5. Set up Argo CD:
        Install Argo CD on the Kubernetes cluster.
        Set up a Git repository for Argo CD to track the changes in the Helm charts and Kubernetes manifests.
        Create a Helm chart for the Java application that includes the Kubernetes manifests and Helm values.
        Add the Helm chart to the Git repository that Argo CD is tracking.

    6. Configure Jenkins pipeline to integrate with Argo CD:
       6.1 Add the Argo CD API token to Jenkins credentials.
       6.2 Update the Jenkins pipeline to include the Argo CD deployment stage.

    7. Run the Jenkins pipeline:
       7.1 Trigger the Jenkins pipeline to start the CI/CD process for the Java application.
       7.2 Monitor the pipeline stages and fix any issues that arise.

This end-to-end Jenkins pipeline will automate the entire CI/CD process for a Java application, from code checkout to production deployment, using popular tools like SonarQube, Argo CD, Helm, and Kubernetes.



#### Notes 
JENKINS END TO END CICD Implementation with Detailed Notes | BEST CICD PROJECT :
#################################################################################
This video serves as an end-to-end implementation guide for a CI/CD pipeline (0:02-0:30). The project demonstrates how to build and deploy a Java-based Spring Boot application using modern DevOps tools.

Project Overview and Workflow (0:30 - 4:45)
Tools Integrated: The pipeline utilizes Maven for building the application, SonarQube for static code analysis, Docker for containerization, a Git repository for source code and manifest management, and Argo CD for automated deployment to Kubernetes.

Design Philosophy: The instructor explains that instead of using the "Image Updater" tool, the process will use Shell scripts to update the manifest repository. This choice is made because shell scripts are more widely used and recognized in technical interviews compared to specific image update tools.

GitOps Approach: The project follows the GitOps model, emphasizing that deployments should be handled by declarative tools like Argo CD rather than imperative tools like Ansible for application delivery.

Infrastructure Setup (5:32 - 14:45)
Provisioning: The instructor creates an AWS EC2 instance to host the services. A T2 Large instance type is chosen (2 CPUs, 8GB RAM) because the combined resource requirements of Jenkins, SonarQube, and Docker are too heavy for a free-tier instance.

Security: A crucial step in the infrastructure setup involves configuring the inbound traffic rules in the AWS Security Group. By opening necessary ports (such as 8080 for Jenkins), the instance becomes accessible from the internet, though the instructor emphasizes that in real-world scenarios, these rules must be highly restrictive.

Jenkins Configuration (16:12 - 22:30)
Initial Setup: After installing Jenkins on the Ubuntu instance, the instructor performs the initial configuration, including installing default plugins and setting up an administrative account.

Pipeline Creation:
The instructor distinguishes between Freestyle projects (a legacy approach) and Pipeline projects.
The Jenkins Pipeline is the preferred method because it uses Groovy scripting, allowing the pipeline configuration to be stored as a Jenkinsfile within the source code repository. This promotes team collaboration and version control.

Managing the Jenkinsfile:
A critical takeaway is that the Jenkinsfile does not necessarily have to be named "Jenkinsfile" or reside at the root of the entire repository. It can be placed inside specific folders (like the Spring Boot application directory) and named differently to handle multiple applications within a single codebase.
The instructor demonstrates how to point the Jenkins job configuration to this specific file, ensuring the pipeline executes the correct stages automatically.

####
Generate detailed long notes from 22:31 to 37:12 without any short cut notes please.

Following the setup of the Jenkins environment, the instructor focuses on preparing the infrastructure for the continuous integration tasks defined in the pipeline.

Docker as a Pipeline Agent (23:31 - 28:50)

Strategy: The instructor emphasizes using Docker containers as agents for Jenkins pipelines. This approach is highly effective because it minimizes the need for manual configuration on the Jenkins host server.

Dynamic Provisioning: When a pipeline is triggered, the system automatically creates a Docker container, executes the necessary stages (such as Maven builds or code scans) within that isolated environment, and then deletes the container upon completion.

Resource Management: This ephemeral nature of containers ensures that system resources are freed up after every job, allowing multiple Jenkins jobs to run concurrently without performance degradation or resource conflicts.

Implementation Requirements: To utilize this, one must install the Docker Pipeline plugin in Jenkins and carefully select a base image that contains the necessary runtime environment for the application being built.

SonarQube Integration (28:50 - 37:12)
Purpose: SonarQube is integrated into the workflow to handle static code analysis, ensuring code quality and security by identifying bugs and vulnerabilities.
Plugin Configuration: The instructor demonstrates installing the Sonar Scanner plugin within the Jenkins interface.

Server Setup:
The SonarQube server is hosted on the same AWS EC2 instance to simplify network connectivity between Jenkins and the scanner.
The setup process involves creating a dedicated sonarqube user to manage the service, downloading the SonarQube binaries, and installing the unzip utility required to extract the files.

Permissions and Execution: The instructor outlines specific steps for granting permissions to the SonarQube directory and highlights the importance of correctly mapping the SonarQube project within the Jenkins job configuration. This allows the pipeline to push code execution reports directly to the Sonar dashboard for real-time monitoring.


