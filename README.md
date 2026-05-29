# DevSecOps Continuous Integration (CI) Jenkins Pipeline

This repository contains a declarative Jenkins pipeline script designed to automate code validation, security scanning, static code analysis, and vulnerability checks for a **3-Tier Node.js Application**.

## 📋 Overview

The CI pipeline implements DevSecOps best practices to ensure that the code checked into the development branch is clean, syntax-error-free, secure against credential leaks, and evaluated for vulnerabilities before deployment.

### Pipeline Configuration File
* [ci-cd-pipeliine-codestage](file:///Users/majidullask/Documents/ci-cd%20for%20proof%20/CI/ci-cd-pipeliine-codestage) — The core Jenkins declarative pipeline file defining the automation stages.

---

## 🛠️ Pipeline Stages

The pipeline consists of the following automated stages:

| Stage Name | Tool/Technique | Description |
| :--- | :--- | :--- |
| **`git-checkout`** | Git | Checks out the `local-dev` branch from the target 3-tier repository (`3-Tier-DevSecOps-Mega-Project`). |
| **`frontent-compilation`** | Node.js | Navigates to the frontend directory (`client`) and runs a syntax compilation check on all JavaScript files using `node --check`. |
| **`backend-compilation`** | Node.js | Navigates to the backend API directory (`api`) and runs a syntax compilation check on all JavaScript files using `node --check`. |
| **`gitleaks`** | Gitleaks | Scans the codebase (`client` and `api` folders) for hardcoded secrets, api keys, and certificates. Fails the pipeline if leaks are detected. |
| **`sonarQube Analysis`** | SonarQube Scanner | Launches static application security testing (SAST) and code quality evaluation, sending the report to the SonarQube server. |
| **`Quality Gate check`** | SonarQube Quality Gate | Blocks the pipeline and waits (up to 1 hour) for the SonarQube server to evaluate and return the Quality Gate result. |
| **`Trivy FS Scan`** | Trivy | Scans the filesystem for OS package and library dependencies vulnerabilities, outputting a detailed HTML report named `fs-report.html`. |

---

## 🚀 Environment & Prerequisites

To run this pipeline successfully, the Jenkins environment must be configured with:

1. **Global Tool Configurations**:
   - **Node.js**: Configured with the name `nodejs26`.
   - **SonarQube Scanner**: Configured with the name `sonar-scanner`.

2. **System Configurations & Credentials**:
   - **SonarQube Server**: A server connection configured in Jenkins named `sonar`.
   - **SonarQube Token**: A credential in Jenkins with the ID `sonar-tocken`.

3. **Agent CLI Utilities**:
   - **Gitleaks CLI** installed on the Jenkins runner PATH.
   - **Trivy CLI** installed on the Jenkins runner PATH.

---

## 📊 Executions & Visual Evidence

Three screenshots are included in the repository demonstrating the setup and execution results:

### 1. Jenkins Pipeline Stage View
*File Reference:* [Screenshot 1](file:///Users/majidullask/Documents/ci-cd%20for%20proof%20/CI/Screenshot%202026-05-29%20at%205.51.26%20PM.png)

This screenshot displays the Jenkins interface:
- **Builds #9 and #10** passed successfully (all stages colored green). The average execution time was approximately ~27 seconds.
- **Builds #7 and #8** failed during the `sonarQube Analysis` stage, highlighting how pipeline checks catch static analysis failures early in the delivery lifecycle.

### 2. SonarQube Projects Analysis
*File Reference:* [Screenshot 2](file:///Users/majidullask/Documents/ci-cd%20for%20proof%20/CI/Screenshot%202026-05-29%20at%205.51.36%20PM.png)

The SonarQube analysis dashboard showing the quality metrics for the project `Nodejs-project`:
- **Status**: **Passed**
- **Bugs**: 0 (A Rating)
- **Vulnerabilities**: 0 (A Rating)
- **Hotspots Reviewed**: 0.0% (E Rating)
- **Code Smells**: 3 (A Rating)
- **Total Lines Analyzed**: 1.3k JavaScript lines

### 3. Jenkins Configuration Interface
*File Reference:* [Screenshot 3](file:///Users/majidullask/Documents/ci-cd%20for%20proof%20/CI/Screenshot%202026-05-29%20at%205.52.31%20PM.png)

Shows the pipeline definition script embedded directly in the Jenkins job configuration GUI.
