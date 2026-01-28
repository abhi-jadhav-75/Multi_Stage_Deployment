# Multi-Stage Branch-Based Deployment (Staging & Production)

## Project Overview
This project demonstrates a **branch-based deployment strategy** using **GitHub Actions**, **Docker**, and **AWS EC2**.  
The same GitHub repository is deployed to **two separate EC2 instances** based on the Git branch:

- `staging` branch → Staging EC2 instance
- `main` branch → Production EC2 instance

The deployment is **fully automated** with CI/CD and does not require any manual steps after initial setup.

---

## Architecture Overview
GitHub Repository
├── staging branch ──▶ Staging EC2
└── main branch ──▶ Production EC2

GitHub Actions
├── Detect branch
├── Connect to EC2 via SSH
├── Pull correct branch
└── Deploy using Docker Compose


---

## Branch-to-Environment Mapping

| Git Branch | Environment  | EC2 Instance |
|-----------|-------------|--------------|
| `staging` | Staging     | Staging EC2 |
| `main`    | Production  | Production EC2 |

Each branch is deployed to a **separate EC2 instance**, ensuring complete environment isolation.

---

## CI/CD Workflow Explanation

The project uses **GitHub Actions** for Continuous Integration and Deployment.

### Workflow behavior:
1. A push is made to either `staging` or `main` branch
2. GitHub Actions workflow is triggered automatically
3. The workflow identifies the branch name
4. Based on the branch:
   - `staging` → deploys to Staging EC2
   - `main` → deploys to Production EC2
5. The pipeline:
   - Connects to the EC2 instance using SSH
   - Pulls the correct branch
   - Builds the Docker image
   - Runs the application using Docker Compose

No manual deployment is performed after the pipeline is set up.

---

## Docker-Based Deployment

- The application is containerized using **Docker**
- **Docker Compose** is used to manage the application container
- Environment variables are used to pass the branch name to the application

The same Docker setup is used for both staging and production.

---

## Application Endpoints

The application exposes the following endpoints:

### Health Check
/health
Returns application health status.

### Version Check
/version
Returns the deployed branch name (`staging` or `main`).

---

## Steps to Verify Deployment

### Verify Staging and Production Deployments
Open in browser:
http://<STAGING_EC2_PUBLIC_IP>:3000/version
http://<PRODUCTION_EC2_PUBLIC_IP>:3000/version






