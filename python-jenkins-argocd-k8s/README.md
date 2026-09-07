# django-todo
A simple todo app built with django

![todo App](https://raw.githubusercontent.com/shreys7/django-todo/develop/staticfiles/todoApp.png)

## CICD Architecture [GitHub -> Jenkins -> k8s Manifests -> Argo CD -> k8s cluster]

![Screenshot 2023-02-01 at 2 48 06 PM](https://user-images.githubusercontent.com/43399466/216001659-74024e94-2c3c-4f1a-8e2e-3ef69b3a88ad.png)



You can find the complete details of the setup and configuration in the below video

https://www.youtube.com/watch?v=ogrx8G8pClQ


#### Notes #############################################################################################

This video provides a practical, end-to-end demonstration of a CI/CD pipeline (Continuous Integration/Continuous Delivery) using Jenkins and Argo CD for Kubernetes deployments.

Project Overview & Tools (0:01 - 2:12)
Jenkins is utilized for Continuous Integration, while Argo CD serves as the tool for Continuous Delivery.
The approach emphasizes using minimal code and avoiding complex Groovy scripting, making it accessible even for those with limited development experience.
The project uses Minikube as the local Kubernetes cluster, deploying a to-do application used in previous demonstrations.

Initial Deployment Check (2:13 - 4:48)
The speaker verifies that the to-do application is currently running by using kubectl get svc and minikube service to generate a public URL for browser access.
The goal is to demonstrate a modification to the application and observe the automated deployment process.

Demonstrating the CI/CD Pipeline (4:49 - 7:02)
The speaker modifies the index.html file of the to-do application to update the header text.
While normally triggered by GitHub Webhooks, the speaker manually clicks the "Build Now" button in Jenkins due to local firewall restrictions.

Jenkins Pipeline Breakdown (7:03 - 10:45)
Jenkinsfile: The pipeline is defined by a Jenkinsfile stored directly in the GitHub repository. It uses a declarative pipeline syntax for consistency and ease of maintenance.

Stages:

Checkout: Pulls the latest code changes from the repository.
Build Docker: Builds a new image using the Jenkins build number.
Push Artifacts: Pushes the new Docker image to Docker Hub.
The speaker notes that for beginners, the Pipeline Syntax Generator in the Jenkins UI is a helpful tool for constructing necessary scripts.

Implementing GitOps with Argo CD (10:46 - 15:05)
Artifact Repository: The Kubernetes manifests (e.g., deploy.yaml) are kept in a separate dedicated repository. This is considered a best practice for GitOps principles.

Automation: The Jenkins pipeline updates the image tag in the Kubernetes manifest repository after a successful build.

Syncing: Argo CD continuously monitors the manifest repository. Once the tag is updated (e.g., to build #33), Argo CD detects the drift, syncs the configuration, and performs the rolling update on the Kubernetes cluster automatically.

Technical Rationale (15:33 - 19:00)
The speaker reiterates the importance of storing the Jenkinsfile in the source code repository to enable version control and auditability.
By offloading the deployment phase to Argo CD, the system gains Auto-healing capabilities, meaning Argo CD will revert any manual, unauthorized changes made directly to the Kubernetes cluster that deviate from the state defined in Git.

The final section of the video (19:01 - 23:24) focuses on the configuration process for Argo CD and the overall value of the CI/CD pipeline established in the demonstration.

Argo CD Application Configuration (19:01 - 21:05)
User Interface Configuration: The speaker explains that Argo CD can be configured entirely through its web console, making it accessible even for those without deep technical expertise.

Key Settings:
Automation: Users can select "Automatic" for synchronization, ensuring that any changes pushed to the GitHub repository are immediately reflected in the Kubernetes cluster.

Auto-healing: The speaker highlights the importance of the Auto-heal feature, which forces the Kubernetes cluster state to match the configuration defined in Git. If a user manually alters a deployment on the cluster, Argo CD will automatically revert those changes to match the repository.

Required Inputs: To set up an application, users simply need to provide the GitHub repository URL, the specific directory path, the target Kubernetes cluster, and the namespace for deployment.

Pipeline Summary & Next Steps (21:06 - 23:24)
Recap: The speaker summarizes the workflow: Jenkins manages the Continuous Integration (building the code and updating the manifest repository), while Argo CD manages the Continuous Delivery (pulling from Git and deploying to Kubernetes).

Documentation: The speaker mentions that the README file in the project repository will be updated to include these end-to-end CI/CD steps for easier reference by viewers.

Call to Action: The speaker invites viewers to explore his other videos on Argo CD for deeper dives into its features. He also offers to host a future live session to walk through the actual installation and setup process if requested by the community in the comment section.

#########################################################################################################################################################
