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

### 2.2 Optional Dependencies:
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

### 2.3 Install PlantUML Plugin
Install the **“PlantUML Markdown Preview”** extension on IBM Bob IDE.
1. Click on the ‘Extensions’ icon.
2. Search for “PlantUML”.
3. Install the extension shown below:

![Extension: PlantUML Markdown Preview](images/image1.png)


### 2.4 Demo Application
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
This lab showcases the complete journey from legacy Struts 1.x + SQLite to modern **Spring Boot 3.x + React 18 + PostgreSQL 15** including JWT Authentication and deployment to OpenShift/Kubernetes.

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
![Switch to Ask Mode](images/image5.png)

2. **Enhance Prompt:** Enter the prompt below and click the **‘Enhance Prompt’** icon:
   > *"Help me understand this legacy-netbanking application. Save the generated documentation and diagrams in legacy-netbanking-documentation directory for it. Make sure to generate required plantUML diagrams like sequence diagram along with mermaid architecture and ER diagram etc of current implementation"*

![Enhance Prompt Icon](images/image6.png)

3. Bob will generate an enhanced prompt. Hit enter to send.

![Enhanced Prompt](images/image7.png)

4. **Approve Tasks:** Keep reviewing and approving the tasks as Bob works.

![Processing Tasks](images/image8.png)

5. **Review Artifacts:**
   - Right-click the generated `.md` file and select **‘Open Preview’**.

![Open Preview Documentation](images/image9.png)
![Documentation Preview](images/image10.png)

- Right-click a `.puml` file and select **‘Preview PlantUML File’**.

![PlantUML Preview Menu](images/image11.png)
![Sequence Diagram Preview](images/image12.png)

### 4.3 Java Version Upgrade
**Objective:** Upgrade to Java 17.

1. Click **“Start New Task”** and select **“Java Modernization”**.

![Java Modernization Selection](images/image14.png)

2. **Set Path:** Update the project path to point to the `legacy-netbanking` directory.

![Copy Path](images/image15.png)
![Analyze Project](images/image16.png)

3. **Configure Upgrade:**
   - Select **‘Java Upgrade’**.
   - Disable **‘Git Flow’**.
   - Select **Target Java version: 17**.
   - Disable **‘Jakarta EE migration’**.

![Java Upgrade Config](images/image17.png)
![Target Version Config](images/image18.png)

4. **Execute:** Click **‘Run Recipes’** and follow the prompts to approve the Todo list.

![Upgrade Todo List](images/image19.png)

5. Once completed, a summary flowchart will be displayed.

![Upgrade Success Flowchart](images/image20.png)

### 4.4 Full Application Modernization
**Objective:** Transform to Spring Boot + React using a Custom Mode.

1. **Explore Custom Mode:** Open **Settings** > **Modes** > **‘Modernization Architect’**.

![Open Settings](images/image21.png)
![Modernization Architect Mode](images/image22.png)
![Role Definition](images/image23.png)

2. **Review Rules:** Expand `.bob/rules-modernization-architect` in the Explorer to see the rule files.

![Rule Files](images/image24.png)

3. **Switch Mode:** Select **‘Modernization Architect’** mode and enter the modernization prompt (Backend: Spring Boot 3.x, Java 17, PostgreSQL, JWT; Frontend: React).

![Switch to Custom Mode](images/image25.png)

4. **Approve Todo List:** Bob will systematically work through the migration.

![Modernization Todo List](images/image26.png)

> **Note:** If Bob stops prematurely due to "Lost in the middle," use the prompt: *"Complete remaining tasks from the todo list"*.

5. **Review Results:** Check the new `modern-netbanking` project structure.

![Modernized Project Structure](images/image27.png)

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
