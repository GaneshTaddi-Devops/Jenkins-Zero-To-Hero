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


Docker Installation and Configuration (37:12 - 42:03):

The speaker demonstrates installing Docker on an AWS EC2 instance using command-line instructions provided in the repository's documentation.
A crucial step involves granting permissions to both the Jenkins and Ubuntu system users, ensuring they have the necessary rights to create, run, and pull containers.

Following the installation and permission updates, the Docker service must be restarted to apply changes.

After configuring Docker, it is considered a best practice to restart the Jenkins service. This is necessary because new plugins (such as SonarQube and Docker Pipeline plugins) have been installed, and a clean restart ensures these plugins function correctly within the pipeline environment.
Kubernetes and Argo CD Setup (42:03 - 48:50):

The focus shifts to the Kubernetes environment. The speaker confirms the Minikube cluster is operational.

The speaker emphasizes the use of Kubernetes Operators for installing controllers like Argo CD. Using operators simplifies the management of the controller's lifecycle, handles automatic upgrades, and provides default configurations out of the box.

The process involves installing the Operator Lifecycle Manager (OLM) first, followed by the specific Argo CD Operator. The speaker advises patience during these installations, as the system must wait for specific pods and custom resource definitions (CRDs) to deploy successfully.

Understanding the Jenkins Pipeline Architecture (48:50 - 56:05):

The speaker explains the structure of the Jenkins pipeline stages, which represent distinct blocks of work: building the application, static code analysis, creating/pushing images, updating the manifest repository, and finally, the continuous delivery stage.

CI vs. CD: A key architectural decision is separating the CI (Continuous Integration) handled by Jenkins from the CD (Continuous Delivery) managed via a GitOps model using Argo CD.

Maven and Build Logic: The discussion covers the Maven build process.
When the pipeline runs mvn clean package, it targets the pom.xml file.

The speaker clarifies the difference between mvn clean package and mvn clean install: package is sufficient when you only need to create the build artifact (e.g., a .jar or .war file) to be included in a Docker image, whereas install is typically used when you need to push artifacts to a repository like Nexus or Artifactory.

The pom.xml file is central to this process, as it defines all project dependencies. By using Maven, these dependencies are automatically fetched from the internet, removing the need to manually include large library files in the source code repository.

#######
The segment from (56:06 - 01:01:16) focuses on the static code analysis phase of the CI/CD pipeline and the subsequent Docker image management process.

Static Code Analysis with SonarQube (56:06 - 58:48)
The speaker explains that to integrate SonarQube into the Jenkins pipeline, the system must be provided with the SonarQube server's URL and an authentication token. This allows Jenkins to communicate effectively with the server and upload code quality reports.

A key step involves executing the mvn sonar:sonar command as a specific Maven goal. The speaker highlights that while the command is straightforward, it requires the developer to be in the correct directory of the project where the application code resides.

The configuration ensures that static code analysis is performed consistently, enabling the team to identify potential bugs, vulnerabilities, or code smells before proceeding with the deployment process.

Docker Image Construction and Distribution (58:48 - 01:01:16)
Following successful static analysis, the pipeline proceeds to build and push the Docker image. The speaker notes that the audience is expected to be familiar with standard Docker build and push procedures, so they focus on the operational necessity of providing Docker credentials within the Jenkins environment.

These credentials are essential for authenticating with the Docker Hub registry, allowing the pipeline to securely upload the newly created container image.

The speaker explains that once the image is pushed to the registry, it becomes available for the GitOps component (Argo CD) to pull and deploy, fulfilling the requirement for a fully automated, continuous delivery cycle. The segment concludes by mentioning the transition toward updating the Manifest repository, which serves as the final integration point before the application reaches the Kubernetes cluster.

####

The segment from (01:01:16 - 01:09:40) focuses on finalizing the CI/CD pipeline configuration, specifically handling security credentials and verifying the readiness of the environment before triggering the end-to-end execution.

Security Token Management (01:01:16 - 01:04:40)
The speaker emphasizes the critical importance of security when generating and applying API tokens (such as a personal access token for GitHub or similar services).
To prevent sensitive credentials from being exposed during the screen-sharing demonstration, the speaker temporarily pauses the display. This highlights a best practice for all DevOps engineers: never share authentication secrets or tokens in public environments or recordings.
Once the token is generated, it is securely pasted into the Jenkins configuration dashboard to facilitate communication between Jenkins and the version control or infrastructure repositories.

Pipeline Readiness and System Restart (01:04:40 - 01:06:50)
After configuring the necessary credentials and plugins, the speaker performs a system restart of Jenkins. This is emphasized as a vital step, especially after installing multiple plugins and configuring new security secrets, as it ensures that the application environment is clean and all services are loaded with the latest configurations.
The speaker notes that the browser will automatically reload once the Jenkins server becomes responsive again, indicating that the system is ready to process the pipeline.

Environment Verification (01:06:50 - 01:09:40)
With the environment back online, the speaker confirms the status of the Argo CD components. By running kubectl get pods -n operators, they verify that the Argo CD Operator is running, which is a prerequisite for the continuous delivery phase of the pipeline.
The segment concludes with the speaker ready to trigger the build process for the application. They express confidence but also readiness to debug, reiterating that CI/CD pipelines rarely succeed on the first attempt and that debugging is a core skill for any DevOps professional.


----------

The segment from (01:09:41 - 01:14:00) provides a detailed observation of the CI/CD pipeline execution, verifying that the automated stages function as intended, from build to deployment.

Execution of the Build and Analysis Stages (01:09:41 - 01:11:08)
The speaker monitors the pipeline as it initiates the build process. The system first checks for the required container images, pulling them automatically when not present in the local registry.

Once the environment is prepared, the pipeline executes the Maven build. The terminal output demonstrates the downloading of necessary project dependencies, which confirms that the pom.xml configuration is being interpreted correctly.

The successful creation of the Java archive (JAR) file is highlighted as a critical milestone; without this artifact, the subsequent stages of the pipeline cannot proceed, as the Docker image construction relies on the binary output of this build phase.

Quality Assurance and Artifact Creation (01:11:08 - 01:14:00)
SonarQube Verification: The speaker pivots to the SonarQube server interface to verify that the static code analysis report has been successfully pushed. The dashboard shows that the application has passed the quality gate with zero bugs and zero vulnerabilities, proving the integration between Jenkins and SonarQube is fully operational.

Docker Image Management: The final step involves confirming that the Docker image has been built and tagged with the current build number (build 1). The speaker runs the docker images command on the EC2 instance to confirm the existence of the image, labeled as Abhishek-F5-ultimate-cicd.
Registry Push: The speaker verifies that the image was successfully pushed to Docker Hub. This step confirms that the continuous integration loop is complete, and the artifact is now available for the GitOps phase. The segment concludes with the speaker confirming that the Manifest repository has also been updated by the shell script, readying the system for the final automated deployment via Argo CD.

---------

the final phase of this tutorial focuses on deploying the application using Argo CD onto a Kubernetes cluster (specifically Minikube). Below are the detailed steps covered from (1:14:01) to the end of the video:

1. Argo CD Installation and Configuration
Leveraging Kubernetes Operators: The host demonstrates using the Argo CD Operator for installation instead of manual Helm chart deployment. This approach is recommended for managing the lifecycle of Kubernetes controllers, handling upgrades, and managing custom resource definitions (CRDs) automatically (1:14:06).

Accessing the Argo CD UI: After applying the necessary YAML configurations to create the Argo CD controller, you must retrieve the initial password. The password is encrypted in a base64-encoded Kubernetes secret named argocd-cluster within the default namespace (1:18:21).
Deciphering Credentials: To view the password, the host uses kubectl get secret argocd-cluster followed by decoding the base64 value using the command echo [secret_value] | base64 -d. It is noted that one should ignore trailing symbols or use the -n flag to prevent issues with improper characters (1:18:58).

3. Deploying the Application via Argo CD
Syncing the Manifests: Once logged into the Argo CD dashboard, you connect your GitHub repository that contains the Kubernetes manifest files. By pointing Argo CD to the folder containing the deployment.yaml file, the tool automatically detects the desired state defined in the repository (1:21:05).
Automatic Deployment: Clicking the 'Sync' button instructs Argo CD to apply the configurations to the Kubernetes cluster. The system automatically handles the creation of deployment objects, replica sets, and pods (1:21:26).

5. Monitoring Configuration Drift
GitOps Capability: A core advantage demonstrated is Argo CD's ability to monitor for "configuration drift." If a malicious or unauthorized user manually changes an image tag on the Kubernetes cluster (e.g., changing the image tag from '1' to '2'), Argo CD detects that the live state deviates from the desired state in the Git repository (1:23:44).

Auto-Healing: Argo CD identifies this discrepancy and provides a notification of being 'out of sync.' This allows the system to auto-heal or be manually synchronized back to the state stored in the Git repository, ensuring the infrastructure remains consistent with the version-controlled manifests (1:24:35).

Final Recommendations for Learners
Resume Preparation: The host strongly advises learners to implement this entire end-to-end pipeline. Documenting this project, including the architecture and the integration of tools like Jenkins, SonarQube, Argo CD, and Kubernetes, is presented as a high-value asset for a DevOps professional's resume (1:25:55).
Professional Outreach: Building this project and sharing the implementation process on platforms like LinkedIn or GitHub can significantly increase visibility among recruiters (1:26:05).





