# Automated CI/CD Pipeline for Node.js on Kubernetes

![Jenkins](https://img.shields.io/badge/Jenkins-Build%20Automated-red)
![Docker](https://img.shields.io/badge/Docker-Containerized-blue)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Self--Healing-green)
![Node.js](https://img.shields.io/badge/Node.js-Express-yellow)

## 🚀 Project Overview
This project demonstrates a fully automated **DevOps CI/CD pipeline**. It takes a Node.js application from code commit to production deployment with zero manual intervention.

The pipeline is designed to be **resilient** and **self-healing**, utilizing **Jenkins** for orchestration, **Docker** for consistent environments, and **Kubernetes** for high-availability deployment.

## 🏗️ Architecture
The pipeline follows a modern containerized workflow:

1.  **Source Control (GitHub):** Developer pushes code to the `main` branch.
2.  **Continuous Integration (Jenkins):**
    * **Poll SCM:** Jenkins detects changes automatically.
    * **Containerized Build:** A Docker agent builds the image dynamically.
    * **Automated Testing:** Jest unit tests run inside an ephemeral container to ensure code quality before release.
    * **Artifact Push:** Verified images are tagged and pushed to **Docker Hub**.
3.  **Continuous Deployment (Kubernetes):**
    * The cluster pulls the new image from Docker Hub.
    * **Rolling Update:** Kubernetes performs a zero-downtime deployment, replacing old pods with new ones incrementally.
    * **Self-Healing:** The cluster automatically restarts failed pods to ensure 100% uptime.

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
| **Push** | Authenticates with Docker Hub and uploads the artifact. | ✅ |
| **Deploy** | Triggers a rolling restart on the Kubernetes cluster to apply the new image. | ✅ |

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
3.  3.  **Trigger Update (Manual):**
    Use the command provided by the Jenkins Console Output to update the running image.
    ```