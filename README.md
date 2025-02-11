# End-to-End Text Summarization Project with Deployment using Github Actions

## Overview
This project implements an end-to-end text summarization pipeline using Natural Language Processing (NLP) techniques. The model is deployed using AWS EC2 and AWS ECR with CI/CD automation through GitHub Actions.

## Workflows
### Project Structure:
1. **Update `config.yaml`** - Define configurations for the project.
2. **Update `params.yaml`** - Set hyperparameters and other parameters.
3. **Update `entity`** - Create data entity classes.
4. **Update Configuration Manager in `src/config`** - Manage project configurations.
5. **Update Components** - Develop and refine the summarization pipeline components.
6. **Update Pipeline** - Integrate components into an end-to-end pipeline.
7. **Update `main.py`** - Build execution script.
8. **Update `app.py`** - Develop the FastAPI/Flask-based web application.

## How to Run?
### Steps to Set Up and Run Locally:
1. **Clone the Repository**
   ```sh
   git clone https://github.com/entbappy/End-to-end-Text-Summarization.git
   cd End-to-end-Text-Summarization
   ```
2. **Create a Conda Environment**
   ```sh
   conda create -n summary python=3.8 -y
   conda activate summary
   ```
3. **Install Dependencies**
   ```sh
   pip install -r requirements.txt
   ```
4. **Run the Application**
   ```sh
   python app.py
   ```
5. **Access the Application**
   - Open `http://localhost:5000` in a web browser.

---
## AWS Deployment with CI/CD
This project is deployed using AWS EC2, AWS ECR, and GitHub Actions for continuous integration and deployment.

### AWS Setup
1. **Login to AWS Console**
2. **Create IAM User for Deployment**
   - Grant required permissions:
     - `AmazonEC2ContainerRegistryFullAccess`
     - `AmazonEC2FullAccess`

### Deployment Steps:
#### 1. Build and Push Docker Image to AWS ECR
1. **Create an ECR Repository**
   ```sh
   aws ecr create-repository --repository-name text-s
   ```
2. **Authenticate Docker with AWS ECR**
   ```sh
   aws ecr get-login-password --region <AWS_REGION> | docker login --username AWS --password-stdin <AWS_ECR_LOGIN_URI>
   ```
3. **Build and Push Docker Image**
   ```sh
   docker build -t text-s .
   docker tag text-s:latest <AWS_ECR_LOGIN_URI>/text-s:latest
   docker push <AWS_ECR_LOGIN_URI>/text-s:latest
   ```

#### 2. Launch EC2 and Deploy
1. **Create an EC2 Instance** (Ubuntu 20.04 recommended)
2. **SSH into the EC2 Instance**
   ```sh
   ssh -i <your-key.pem> ubuntu@<EC2-PUBLIC-IP>
   ```
3. **Install Docker on EC2**
   ```sh
   sudo apt-get update -y
   sudo apt-get install -y docker.io
   sudo usermod -aG docker ubuntu
   newgrp docker
   ```
4. **Pull and Run Docker Container**
   ```sh
   aws ecr get-login-password --region <AWS_REGION> | docker login --username AWS --password-stdin <AWS_ECR_LOGIN_URI>
   docker pull <AWS_ECR_LOGIN_URI>/text-s:latest
   docker run -d -p 80:5000 <AWS_ECR_LOGIN_URI>/text-s:latest
   ```

#### 3. Configure GitHub Actions for CI/CD
1. **Set Up GitHub Secrets** in Repository Settings:
   - `AWS_ACCESS_KEY_ID`
   - `AWS_SECRET_ACCESS_KEY`
   - `AWS_REGION`
   - `AWS_ECR_LOGIN_URI`
   - `ECR_REPOSITORY_NAME`
2. **Create `.github/workflows/deploy.yml`**
   ```yaml
   name: Deploy to AWS EC2
   on:
     push:
       branches:
         - main
   jobs:
     deploy:
       runs-on: ubuntu-latest
       steps:
         - name: Checkout Repository
           uses: actions/checkout@v3

         - name: Login to AWS ECR
           run: |
             aws ecr get-login-password --region ${{ secrets.AWS_REGION }} | docker login --username AWS --password-stdin ${{ secrets.AWS_ECR_LOGIN_URI }}

         - name: Build and Push Docker Image
           run: |
             docker build -t ${{ secrets.ECR_REPOSITORY_NAME }} .
             docker tag ${{ secrets.ECR_REPOSITORY_NAME }}:latest ${{ secrets.AWS_ECR_LOGIN_URI }}/${{ secrets.ECR_REPOSITORY_NAME }}:latest
             docker push ${{ secrets.AWS_ECR_LOGIN_URI }}/${{ secrets.ECR_REPOSITORY_NAME }}:latest

         - name: Deploy to EC2
           uses: appleboy/ssh-action@master
           with:
             host: ${{ secrets.EC2_PUBLIC_IP }}
             username: ubuntu
             key: ${{ secrets.SSH_PRIVATE_KEY }}
             script: |
               docker pull ${{ secrets.AWS_ECR_LOGIN_URI }}/${{ secrets.ECR_REPOSITORY_NAME }}:latest
               docker stop text-s || true
               docker rm text-s || true
               docker run -d -p 80:5000 --name text-s ${{ secrets.AWS_ECR_LOGIN_URI }}/${{ secrets.ECR_REPOSITORY_NAME }}:latest
   ```

## Access the Deployed Application
- **Public EC2 IP**: `http://<EC2-PUBLIC-IP>`
- API Documentation: `http://<EC2-PUBLIC-IP>/docs`

## Author
Krish Naik  
Data Scientist  
Email: krishnaik06@gmail.com  

---
This README provides a step-by-step guide to running the project locally and deploying it to AWS with CI/CD automation.

