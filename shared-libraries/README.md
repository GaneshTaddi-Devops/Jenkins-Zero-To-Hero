# Shared Libraries

In Jenkins, a shared library is a way to store commonly used code(reusable code), such as scripts or functions, that can be used by different 
Jenkins pipelines. 

Instead of writing the same code again and again in multiple pipelines, you can create a shared library and use it in all the pipelines
that need it. This can make your code more organized and easier to maintain. 

Think of it like a library of books, Instead of buying the same book over and over again, you can borrow it from the library whenever you need it.

## Advantages

- Standarization of Pipelines
- Reduce duplication of code
- Easy onboarding of new applications, projects or teams
- One place to fix issues with the shared or common code
- Code Maintainence 
- Reduce the risk of errors

![Screenshot 2023-05-02 at 9 47 24 PM](https://user-images.githubusercontent.com/43399466/235724851-90a5cad6-ac0d-428b-9944-93fffea55180.png)

----NOTES ----


This video serves as a comprehensive introduction to **Jenkins Shared Libraries**. Abhishek Veeramalla explains the fundamental concepts, the practical advantages for DevOps engineers, and the architectural shift from manual, repetitive pipeline management to a centralized, efficient, and scalable approach (0:01 - 11:20).

### **The Core Problem: Repetitive Pipeline Management**
Abhishek illustrates a scenario (1:20) typical of large organizations or companies with many microservices, such as Amazon. In such environments:
* **Independent Microservices:** Every microservice requires its own independent CI/CD pipeline, as they are managed, deployed, and destroyed individually (1:45).
* 
* **The Replication Trap:** DevOps engineers often end up managing hundreds of pipelines. When a new service is onboarded, the standard practice is to copy an existing *Jenkinsfile* and modify it. While functional, this leads to significant long-term maintenance debt (3:30).
* 
* **The Maintenance Nightmare:** If a configuration change is required (e.g., switching from `mvn clean package` to `mvn clean install`) or if a security vulnerability is discovered, the engineer faces the gargantuan task of manually updating hundreds of pipelines. Failing to do so across all instances can lead to inconsistent deployments or security risks (4:30 - 5:15).

### **The Solution: Jenkins Shared Libraries**

A **Shared Library** is defined as a repository of common, reusable code snippets that can be extracted from individual *Jenkinsfiles* and centralized (5:20).

* **How it works:** Instead of hardcoding logic inside every pipeline, the engineer moves common tasks—such as Maven builds, SCM checkouts, or sonar analysis—into a library file in a Git repository (6:20).
* 
* **The Workflow:** The *Jenkinsfile* simply references the library name and the desired module (e.g., `Maven build`). The Jenkins controller then automatically fetches the shared code and executes it (6:50).
* 
* **Standardization:** This approach ensures that all pipelines adhere to the same organizational standards. When a change is needed, it is applied in one location (the Shared Library repository), and it propagates to every pipeline upon the next execution (8:30).

### **Key Advantages of Shared Libraries**
Abhishek outlines several strategic benefits for DevOps teams (9:45 - 11:20):

1. **Standardization:** Provides a consistent blueprint for all projects.
2. **Reduced Duplication:** Eliminates the need for copy-pasting code across multiple repositories.
3. **Easier Onboarding:** New team members can quickly set up pipelines using the standardized library templates.
4. **Efficient Maintenance:** Security patches or configuration updates are handled at a single point, drastically reducing operational overhead.
5. **Low-Code Approach:** By abstracting complex scripts into named functions, the process moves towards a 'low-code' model, minimizing the amount of raw code engineers need to write and manage.
6. **Reduced Error Risk:** Standardized code is less prone to typos or configuration errors that frequently occur when manually editing individual files.

The second half of the video (11:21–24:25) focuses on the practical implementation of Jenkins Shared Libraries, providing a step-by-step guide for developers to move from theory to application.

### **Structural Foundation of Shared Libraries**
To create a shared library, developers must understand the expected file structure within a Git repository. A shared library is typically composed of three primary directories (12:25):
*   **`vars`**: This is the most critical directory for beginners. It contains global variables that can be accessed directly in a *Jenkinsfile*. In this tutorial, the focus is exclusively on this directory to simplify the learning curve.
*   **`src`**: Reserved for higher-level Groovy source code (classes and helper functions).
*   **`resources`**: Used for non-Groovy files that might be required by the library, such as JSON or YAML configurations.

### **Step-by-Step Implementation Guide**
Abhishek demonstrates the process of creating and utilizing a shared library with a live Jenkins instance:

1.  **Writing the Library Code (13:00 - 13:45):**
    *   Create a file inside the `vars` folder using the `.groovy` extension (e.g., `helloWorld.groovy`).
    *   Use **camel casing** for file naming (e.g., `helloWorld` instead of `helloworld`).
    *   The core syntax required is the `def call` function. This method is the entry point that gets executed when the file name is invoked in a pipeline.

2.  **Configuring the Library in Jenkins (18:10 - 19:10):**
    *   Navigate to **Manage Jenkins** > **Configure System**.
    *   Locate the **Global Pipeline Libraries** section.
    *   Provide a name for the library (e.g., `my-shared-library`), specify the Git branch, and provide the repository URL. Jenkins automatically looks into the `vars` folder by default, so no manual folder mapping is required here.

3.  **Importing and Using the Library (19:15 - 20:30):**
    *   In the *Jenkinsfile*, add the annotation `@Library('my-shared-library') _` at the top. The underscore (`_`) is essential; it tells Jenkins to import everything (all methods/scripts) present within that library.
    *   To execute a script from the `vars` folder, simply write the name of the file (e.g., `helloWorld()`). Note that you call the **file name**, not the internal `def call` function name.

### **Practical Example: Standardizing Maven Builds**
Abhishek applies these concepts to a real-world scenario (21:15 - 23:15):
*   Instead of hardcoding `sh 'mvn clean install'` in every *Jenkinsfile*, the engineer creates a `mavenBuild.groovy` file in the `vars` directory.
*   This file contains the logic: `def call() { sh 'mvn clean install' }`.
*   The developer then replaces the repetitive shell step in the *Jenkinsfile* with a simple call: `mavenBuild()`.
*   **Centralized Maintenance:** If the build command needs to change (e.g., adding parameters or changing to `mvn clean package`), the engineer only updates the file in the shared library repository. Every pipeline referencing this library will automatically inherit the change upon its next run (23:25–24:25).




