# Microservices App (Docker + CI/CD)

A Multi-Container Microservices Application with Backend API, Frontend, and Database, deployed manually or via GitHub Actions CI/CD workflow.

---

## Table of Contents

1. [Project Structure](#project-structure)  
2. [Prerequisites](#prerequisites)  
3. [Setup Instructions](#setup-instructions)  
4. [Manual Deployment](#manual-deployment)  
5. [CI/CD Deployment](#ci-cd-deployment)  
6. [Usage](#usage)

---

## Project Structure

```text
microservices-app/
├── backend/
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
├── frontend/
│   ├── index.html
│   └── Dockerfile
├── database/
│   └── Dockerfile (optional)
├── docker-compose.yml
└── .github/
    └── workflows/
        └── deploy.yml

Prerequisites
	Docker
	Docker Compose	
	Git
	Access to AWS EC2 instance for deployment
	GitHub account for CI/CD workflow

Setup Instructions
	Clone the repository:
		git clone git@github.com:hmurafique/microservices-app.git
		cd microservices-app

	Create .env files if needed for environment variables.

Manual Deployment
	Build and run containers:
		docker-compose up -d --build

	Stop containers:
		docker-compose down

Access Frontend at http://<EC2_PUBLIC_IP>:80
Access Backend(API) at http://<EC2_PUBLIC_IP>:5000/api

CI/CD Deployment
	Add secrets in GitHub repository:
	EC2_SSH_KEY → Private key of EC2
	EC2_HOST → EC2 public IP
	EC2_USER → EC2 username (usually ubuntu)
	Workflow .github/workflows/deploy.yml triggers automatically on main branch push:
		ssh -i ~/.ssh/id_rsa ${{ secrets.EC2_USER }}@${{ secrets.EC2_HOST }} "bash /home/ubuntu/deploy.sh"

Push changes to main branch:
	git add .
	git commit -m "Trigger CI/CD workflow"
	git push origin main

GitHub Actions workflow will deploy the latest code to EC2 automatically.

Usage
	Frontend: http://<EC2_PUBLIC_IP>:80
	Backend(API): http://<EC2_PUBLIC_IP>:5000/api
	Nginx Reverse Proxy handles routing between Frontend and Backend.

Notes
	Ensure EC2 user belongs to Docker group for permission-free Docker commands:
		sudo usermod -aG docker ubuntu

Test deploy.sh manually on EC2 before relying on CI/CD workflow:
	bash /home/ubuntu/deploy.sh

		cat deploy.sh 
		#!/bin/bash
		
		echo "🚀 Deploying Microservices on $(date)"
		cd /home/ubuntu/microservices-app || exit
		git pull origin main

		docker-compose down
		docker-compose up -d --build

License
	MIT License
