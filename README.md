# Automated CI/CD Pipeline for Node.js on Kubernetes

![Jenkins](https://img.shields.io/badge/Jenkins-Build%20Automated-red)
![Docker](https://img.shields.io/badge/Docker-Containerized-blue)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Self--Healing-green)
![Node.js](https://img.shields.io/badge/Node.js-Express-yellow)

## 🚀 Project Overview
This project demonstrates a robust **DevOps CI/CD pipeline**. It takes a Node.js application from code commit to a production-ready artifact with versioning and security checks.

The pipeline is designed with **security isolation** in mind: Jenkins handles the Integration (CI) and Registry logic, while Kubernetes operations are decoupled to ensure the build server does not require direct root access to the cluster.

## 🏗️ Architecture
The pipeline follows a modern containerized workflow:

1.  **Source Control (GitHub):** Developer pushes code to the `main` branch.
2.  **Continuous Integration (Jenkins):**
    * **Poll SCM:** Jenkins detects changes automatically.
    * **Containerized Build:** A Docker agent builds the image dynamically.
    * **Automated Versioning:** Images are tagged with unique semantic versions (e.g., `v1.0.30`).
    * **Automated Testing:** Jest unit tests run inside an ephemeral container to ensure code quality.
    * **Artifact Push:** Verified images are pushed to **Docker Hub**.
3.  **Continuous Deployment (Kubernetes):**
    * Jenkins calculates the new version and generates the specific deployment command.
    * **Rolling Update:** The operator triggers the update, causing Kubernetes to perform a zero-downtime deployment.
    * **Self-Healing:** Liveness and Readiness probes ensure traffic is only sent to healthy pods.

## 🛠️ Tech Stack
* **Application:** Node.js (Express)
* **CI Server:** Jenkins (running in Docker)
* **Containerization:** Docker & Docker Hub
* **Orchestration:** Kubernetes (Minikube)
* **Testing:** Jest
* **Infrastructure:** Linux (WSL2), Shell Scripting, Groovy (Jenkinsfile)

## ⚙️ Pipeline Stages
| Stage | Description | Status |
| :--- | :--- | :--- |
| **Build** | Packages the Node.js app into a lightweight Alpine-based Docker image. | ✅ |
| **Test** | Runs `npm test` (Jest) inside the container to verify logic. | ✅ |
| **Push** | Authenticates with Docker Hub and uploads the versioned artifact. | ✅ |
| **Deploy** | Outputs the precise `kubectl` command for the operator to execute (Security Isolation). | ✅ |

## 💻 How to Run Locally
**Prerequisites:** Docker, Minikube, Jenkins.

1.  **Clone the Repo:**
    ```bash
    git clone [https://github.com/MartinS984/node-devops-project.git](https://github.com/MartinS984/node-devops-project.git)
    cd node-devops-project
    ```
2.  **Deploy to K8s:**
    ```bash
    kubectl apply -f k8s/deployment.yaml
    kubectl apply -f k8s/service.yaml
    ```
3.  **Trigger Update (Manual):**
    Use the command provided by the Jenkins Console Output to update the running image.

## 🧹 Cleanup
To stop all services and reclaim resources:

1. **Stop Kubernetes:**
   ```bash
   minikube stop