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
   git clone https://github.com/shaadclt/End-to-End-Text-Summarization-Project.git
   cd End-to-End-Text-Summarization-Project
   ```
2. **Create a Conda Environment**
   ```sh
   conda create -n environment_name python=3.8 -y
   conda activate environment_name
   ```
3. **Install Dependencies**
   ```sh
   pip install -r requirements.txt
   ```
4. **Run the Application**
   ```sh
   uvicorn app:app --host 0.0.0.0 --port 8080
   ```
5. **Access the Application**
   - Open `http://localhost:8080` in a web browser.

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
2. **Create `.github/workflows/main.yaml`**
   ```yaml
   name: workflow

   on:
     push:
       branches:
         - main
       paths-ignore:
         - 'README.md'
   
   permissions:
     id-token: write
     contents: read
   
   jobs:
     integration:
       name: Continuous Integration
       runs-on: ubuntu-latest
       steps:
         - name: Checkout Code
           uses: actions/checkout@v3

      - name: Lint code
        run: echo "Linting repository"

      - name: Run unit tests
        run: echo "Running unit tests"

     build-and-push-ecr-image:
       name: Continuous Delivery
       needs: integration
       runs-on: ubuntu-latest
       steps:
         - name: Checkout Code
           uses: actions/checkout@v3

      - name: Install Utilities
        run: |
          sudo apt-get update
          sudo apt-get install -y jq unzip
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ secrets.AWS_REGION }}

      - name: Login to Amazon ECR
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@v1

      - name: Build, tag, and push image to Amazon ECR
        id: build-image
        env:
          ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
          ECR_REPOSITORY: ${{ secrets.ECR_REPOSITORY_NAME }}
          IMAGE_TAG: latest
        run: |
          # Build a docker container and
          # push it to ECR so that it can
          # be deployed to ECS.
          docker build -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
          docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG
          echo "::set-output name=image::$ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG"
          
          
     Continuous-Deployment:
       needs: build-and-push-ecr-image
       runs-on: self-hosted
       steps:
         - name: Checkout
           uses: actions/checkout@v3

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ secrets.AWS_REGION }}

      - name: Login to Amazon ECR
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@v1
      
      
      - name: Pull latest images
        run: |
         docker pull ${{secrets.AWS_ECR_LOGIN_URI}}/${{ secrets.ECR_REPOSITORY_NAME }}:latest
         
      # - name: Stop and remove container if running
      #   run: |
      #    docker ps -q --filter "name=texts" | grep -q . && docker stop texts && docker rm -fv texts
       
      - name: Run Docker Image to serve users
        run: |
         docker run -d -p 8080:8080 --name=texts -e 'AWS_ACCESS_KEY_ID=${{ secrets.AWS_ACCESS_KEY_ID }}' -e 'AWS_SECRET_ACCESS_KEY=${{ secrets.AWS_SECRET_ACCESS_KEY }}' -e 'AWS_REGION=${{ secrets.AWS_REGION }}'  ${{secrets.AWS_ECR_LOGIN_URI}}/${{ secrets.ECR_REPOSITORY_NAME }}:latest
      - name: Clean previous images and containers
        run: |
         docker system prune -f
   ```

## Access the Deployed Application
- **Public EC2 IP**: `http://<EC2-PUBLIC-IP/8080>`
- API Documentation: `http://<EC2-PUBLIC-IP>/docs`

## Author
Mohamed Shaad  
Data Scientist  
Email: shaadclt@gmail.com  


