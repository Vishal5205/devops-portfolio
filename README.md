# DevOps Portfolio – GitOps CI/CD Pipeline

This project demonstrates a production-style CI/CD + GitOps workflow to host a portfolio website.

Jenkins performs Continuous Integration by building and pushing Docker images.  
ArgoCD handles Continuous Deployment by automatically syncing Kubernetes manifests from GitHub.

---

## Tech Stack

- GitHub – Source control
- Jenkins – CI automation
- Docker – Containerization
- Docker Hub – Image registry
- Kubernetes – Container orchestration
- ArgoCD – GitOps continuous deployment
- AWS EC2 – Infrastructure hosting

---

## Architecture

GitHub → Jenkins → Docker Hub → GitHub Manifests → ArgoCD → Kubernetes

---

## Project Structure

GitHub
 ↓
Jenkins
 ↓
Docker Hub
 ↓
GitHub (YAML update)
 ↓
ArgoCD
 ↓
Kubernetes
 ↓
NodePort Service
 ↓
http://EC2_IP:PORT


