# DevOps CI/CD Pipeline with GitHub Actions, Docker, GHCR, and Azure VM 

This project demonstrates an end-to-end CI/CD pipeline for deploying a Dockerized Flask application to an Azure Virtual Machine using GitHub Actions and GitHub Container Registry (GHCR).  
The pipeline automatically builds, packages, publishes, and deploys the application on every push to the `main` branch.

---

## 🚀 Architecture Overview

1. Developer pushes code to GitHub  
2. GitHub Actions triggers CI pipeline  
3. Pipeline:
   - Checks out code  
   - Builds Docker image  
   - Pushes image to GHCR  
4. CD Stage:
   - GitHub Actions SSHes into Azure VM  
   - Pulls latest Docker image  
   - Stops old container  
   - Runs updated container  
5. Application becomes live on:


http://20.63.89.0:5000/

---

## 🛠️ Technologies Used

- GitHub Actions – CI/CD automation  
- Docker – Containerization  
- GitHub Container Registry (GHCR) – Image storage  
- Azure Virtual Machine (Ubuntu) – Deployment environment  
- Flask – Application framework  
- SSH Key Authentication – Secure deploy access  

---

## 📦 Project Structure
---

devops-ci-cd-project/
├── app.py                # Flask application
├── Dockerfile            # Docker image definition
├── requirements.txt      # Python dependencies (if used)
└── .github/
    └── workflows/
        └── deploy.yml    # CI/CD pipeline configuration

---

## 🔧 CI/CD Workflow Summary

### ✔ Continuous Integration (CI)

Triggered on every push to `main`:

- GitHub Actions checks out code  
- Builds Docker image:
  ```bash
  docker build -t ghcr.io/sncharanraj/devops-app:latest .

Pushes image to GHCR:
docker push ghcr.io/sncharanraj/devops-app:latest

✔ Continuous Deployment (CD)

GitHub Actions connects to the Azure VM via SSH and executes:
docker pull ghcr.io/sncharanraj/devops-app:latest
docker stop devops-app || true
docker rm devops-app || true
docker run -d --name devops-app -p 5000:5000 ghcr.io/sncharanraj/devops-app:latest

📡 Azure VM Setup

Create Ubuntu VM on Azure

Install Docker:
sudo apt update
sudo apt install docker.io -y
sudo usermod -aG docker azureuser

Add inbound port rule to allow 5000/TCP

🔑 GitHub Secrets
| Secret Name | Description                         |
| ----------- | ----------------------------------- |
| VM_HOST     | Azure VM public IP                  |
| VM_USER     | VM username (`azureuser`)           |
| VM_SSH_KEY  | Private SSH key for deployment      |
| CAR_PAT     | Classic PAT for GHCR authentication |


🌐 Deployment Output

After successful CI/CD execution, the Flask application becomes accessible at:
http://20.63.89.0:5000/

🧩 Key Troubleshooting Done

Fixed GHCR permission errors using a Classic PAT

Corrected SSH authentication by configuring private key properly

Replaced local WSL deployment with Azure VM due to network isolation

Opened Azure NSG port 5000 for public traffic

Ensured Flask app binds to 0.0.0.0 for external container access

📘 Conclusion

This project demonstrates end-to-end DevOps practices including CI/CD automation, Docker containerization, cloud deployment, registry management, and real debugging experience.
