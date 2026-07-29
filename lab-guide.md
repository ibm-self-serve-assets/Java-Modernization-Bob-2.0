# IBM Bob: Java Modernization – Struts To Springboot Lab Guide

## Revision Chart
| Version | Primary Author(s) | Description of Version | Reviewer | Date Completed |
| :--- | :--- | :--- | :--- | :--- |
| 1.0 | WW Service Engineering Lab | Initial Version | | 03/27/2026 |

## Contents
1. [About this Lab](#1-about-this-lab)
2. [Pre-requisites](#2-pre-requisites)
3. [Overview](#3-overview)
4. [Lab Steps](#4-lab-steps)
    - 4.1 [Import Project into Bob Workspace](#41-import-project-into-bob-workspace)
    - 4.2 [Reverse Engineering – Understanding the Legacy Application](#42-reverse-engineering--understanding-the-legacy-application)
    - 4.3 [Java Version Upgrade](#43-java-version-upgrade)
    - 4.4 [Full Application Modernization](#44-full-application-modernization)
    - 4.5 [Create Kubernetes/OpenShift Deployment Artifacts (Optional)](#45-create-kubernetesopenshift-deployment-artifacts-optional)
5. [Key Modernization Achievements](#5-key-modernization-achievements)
6. [Troubleshooting](#6-troubleshooting)

---

## 1. About this Lab
### Objective of this lab:
* Experience IBM Bob features and capabilities.
* Understand Bob built-in modes, custom modes and MCP extensibility.
* Explore Bob’s comprehensive SDLC capabilities with a legacy Java application modernization use-case.

In this lab, we will take a **Legacy Struts 1.3** application whose documentation is not available and will perform the following activities:
* **Reverse Engineer** the legacy code to generate comprehensive documentation, architecture diagrams, ER diagrams, and UML diagrams.
* Use **‘Java Modernization’** mode to upgrade the Java version from 1.8 to 1.17.
* Use a custom **‘Modernization Architect’** mode to modernize the legacy Struts application to a cloud-native Spring Boot application.
* **Generate** required OpenShift artifacts and scripts for deployment.

> **Note:** For WebSphere modernization use cases (WebSphere/WebLogic/Tomcat to Liberty/Quarkus/EASeJ), users are recommended to use Bob premium ‘Java Modernization’ mode. This mode leverages IBM AMA (Application Modernization Accelerator), rules, and recipes with agentic capabilities.

---

## 2. Pre-requisites
### 2.1 IBM Bob IDE
Install and configure IBM Bob. Follow instructions from [https://bob.ibm.com/docs/ide/getting-started/install](https://bob.ibm.com/docs/ide/getting-started/install).

### 2.2 IBM Bob Premium Package for Java Modernization
Access to IBM Bob Premium Package for Java Modernization is required to use the **Java Modernization** workflow. Ensure your IBM Bob instance has the premium package activated before proceeding with the lab steps.

Please verify in your Bob Settings that you are able to see options in the **"Team"** dropdown and it shows access to IBM Bob Premium package upon selecting the appropriate team.

![Bob Settings – Premium Package](images/bob-settings-premium.png)

### 2.3 Bobcoins
A minimum of **100 Bobcoins** must be available in your account to complete this lab. Bobcoins are consumed as Bob performs analysis, code generation, and modernization tasks throughout the lab.

### 2.4 Optional Dependencies:
1. install `openjdk-17-jdk` package
```sh
# MacOS
brew install openjdk@17
# Ubuntu/Debian
sudo apt update
sudo apt install openjdk-17-jdk
# RHEL
sudo dnf install java-17-openjdk-devel
```
2. install `maven` package
```sh
# MacOS
brew install maven
# Ubuntu/Debian
sudo apt update
sudo apt install maven
# RHEL
sudo dnf install maven
```
3. install `sdkman` package
```sh
# MacOS
curl -s "https://get.sdkman.io" | bash
# Ubuntu/Debian
curl -sL https://get.sdkman.io | sudo bash
# RHEL
curl -sL https://get.sdkman.io | sudo bash
```

### 2.5 Install PlantUML Plugin
Install the **“PlantUML Markdown Preview”** extension on IBM Bob IDE.
1. Click on the ‘Extensions’ icon.
2. Search for “PlantUML”.
3. Install the extension shown below:

![Extension: PlantUML Markdown Preview](images/image1.png)


### 2.6 Demo Application
We are using a legacy-style netbanking application built with:
* **Java 8** (no newer language features)
* **Apache Struts 1.x** (Action, ActionForm, struts-config.xml)
* **JSPs** with scriptlets (`<% %>`)
* **Plain Servlets** (InitServlet, LogoutServlet)
* **JDBC** (no ORM/JPA)
* **SQLite** (file-based database)
* **XML Configuration** (web.xml, struts-config.xml)
* **WAR packaging**

---

## 3. Overview
This lab showcases the complete journey from legacy Struts 1.x + SQLite to modern **Spring Boot 3.x + React** including JWT Authentication and deployment to OpenShift/Kubernetes.

---

## 4. Lab Steps

### 4.1 Import Project into Bob Workspace
1. Download the ‘java-modernization-demo’ from [https://github.com/ibm-self-serve-assets/java-modernization-lab](https://github.com/ibm-self-serve-assets/java-modernization-lab).
2. Go to IBM Bob and open the folder `java-modernization-demo`.

![Importing Project](images/image2.png)
![Opening Folder](images/image3.png)

The folder structure should appear as follows:
![Folder Structure](images/image4.png)

### 4.2 Reverse Engineering – Understanding the Legacy Application
**Objective:** Analyze the undocumented early-2000s code.

1. **Switch to Ask Mode:** Click on ‘Ask’ at the bottom right corner.
![Switch to Ask Mode](images/newimage5.png)

2. **Enter Prompt:** Enter the prompt below. Hit enter to send :
   > *"Help me understand this legacy-netbanking application. Save the generated documentation and diagrams in legacy-netbanking-documentation directory for it. Make sure to generate required plantUML diagrams like sequence diagram along with mermaid architecture and ER diagram etc of current implementation"*

![Enter Prompt](images/newimage6.png)

3. **Approve Tasks:** Keep reviewing and approving the tasks as Bob works.

![Processing Tasks](images/newimage7.png)

5. **Review Artifacts:**
   - Right-click the generated `.md` file and select **‘Open Preview’**.

![Open Preview Documentation](images/newimage8.png)
![Documentation Preview](images/newimage9.png)

- Right-click a `.puml` file and select **‘Preview PlantUML File’**.

![PlantUML Preview Menu](images/newimage10.png)
![Sequence Diagram Preview](images/newimage11.png)

### 4.3 Java Version Upgrade
**Objective:** Upgrade to Java 17.

1. Open the `legacy-netbanking` directory in a different Bob window.

2. Click on workflow icon (play button). You will see 3 option
   - Java Modernization
   - Java Unit Testing
   - Java Vulnerabilities Detection.

![Worflow](images/newimage12.png)

3. Click on the Java Modernization Workflow.
![Worflow-start](images/newimage13.png)

4. Verify if the project path is correct and click **Continue**.
![Continue](images/newimage14.png)

5. Bob will Run some commands to check required tools are available. Approve those Tasks.

6. After the tasks are completed you should see the available modernization.
Select Java Upgrade and Disable Gitflow.
![Options](images/newimage15.png)

7. Approve the tasks to check prerequisites.
![Prerequisites](images/newimage16.png)

8. **Configure Upgrade:**
   - Select **Java Distribution: Oracle JDK**.
   - Select **Java Version: 17**.
   - Select **Jakarta EE Version: Do not Upgrade**.

![Java Upgrade Config](images/newimage17.png)

9. Click Continue and Approve the tasks.

![Upgrade Todo List](images/newimage18.png)

10. Bob will ask to Fix Vulnerabilites but for this lab we will select "No" since we are modernizing application.
![Fix Vulnerabilities](images/newimage19.png) 

11. Approve the To-Do list tasks.
![Todo List](images/newimage20.png)

12. Once completed, a summary flowchart will be displayed.

![Upgrade Success Flowchart](images/newimage21.png)

### 4.4 Full Application Modernization
**Objective:** Transform to Spring Boot + React using a UI Modernization Workflow.

1. Again go to workflows tab and Select Java Modernization.

![Select Workflow](images/newimage22.png)

2. Approve the tasks and Select "UI Modernization" this time.
![Modernization](images/newimage23.png)

3. Approve the To-do List. Bob will analyze the application to explore modernization options.
![To-do List](images/newimage24.png) 
![Approve](images/newimages25.png)

4. Once the analysis is completed. You should see a form. Select both "Frontend Modernization" and "Backend Modernization".
![Configure](images/newimages26.png) 

5. Create the folders to store the frontend and backend code. Open a terminal within the `legacy-netbanking` directory and run the follwing commands. 
```
mkdir frontend
mkdir backend
```

6. Configure the Modernization Options.
   - Select **Frontend Framework: React**.
   - Select **Frontend Design System: Carbon Design System**.
   - Frontend Project Path: Select the frontend directory we just created.
   - Select **Backend Framework: Spring Boot**
   - Backend Project Path: Select the backend directory we just created.
![Configure Modernization](images/newimage27.png)

7. Aprrove the Tools to start the Backend Migration.
![Start Backend Migration](images/newimage28.png)

8. Once the Backend Migration is done. Run the application using the command in the Bob chat window.
![Test Backend](images/newimage29.png)

9. You should see a Swagger UI.
![Backend](images/newimage30.png)

10. Select the "Yes, everything is working" option in bob chat window and this shoudl finish the Backend Migration.
![Finish Backend](images/newimage31.png)

11. Approve the tool to start frontend migration.
![Start Frontend](images/newimage32.png)

12. Once the Frontens Migration SubTask is done. Run the application using the command in the Bob chat window.
![Start Frontend Web](images/newimage33.png)

13. Frontend Migration runs in Multiple Subtask. First it will create the structure and integrate with Backend and validate.
![Frontend Migration SubTask 1](images/newimage34.png)
![Frontend Migration SubTask 2](images/newimage35.png)
![Frontend Migration SubTask 3](images/newimage36.png)

14. Check the application in Browser. For Login use Username: admin and Password: admin123. Perform a Fund transfer to Account : ACC1001
![UI](images/newimage37.png)

15. The modernization is complete now.
![Final](images/newimage38.png)

### 4.5 Create Kubernetes/OpenShift Deployment Artifacts (Optional)
1. In the same mode, enter: *"I need to deploy it on OpenShift. Create required artifacts and scripts"*.
2. Review the generated manifests and `deploy.sh` scripts.

![OpenShift Deployment Artifacts](images/image28.png)
![OpenShift Files Explorer](images/image29.png)

---

## 5. Key Modernization Achievements
* **Framework:** Struts 1.x → Spring Boot 3.x; JSP → React SPA.
* **Database:** SQLite → PostgreSQL with Flyway migrations.
* **Security:** Basic → JWT-based with BCrypt.
* **Cloud-Native:** Dockerized, Kubernetes/OpenShift ready, externalized config.
* **Functional Parity:** All legacy features (transfers, history) preserved.

---

## 6. Troubleshooting
### 6.1 SDKMAN not installed
Switch to **‘Advanced’** mode and enter: *"Install SDKMAN"*.

![Install SDKMAN](images/image30.png)

### 6.2 Don’t see ‘Java Modernization’ workflow option
Close open tasks or restart Bob IDE.

![Close Task Icon](images/image31.png)

### 6.3 Fix errors with Bob recommendations
When an error occurs (e.g., Build Application failure), click **“Fix it”** to let Bob analyze and remediate.

![Fix It Button](images/image32.png)
