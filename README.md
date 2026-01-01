# 🚀 End-to-End Text Summarization System (Production-Ready)

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue.svg)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-Deep%20Learning-red.svg)](https://pytorch.org/)
[![HuggingFace](https://img.shields.io/badge/HuggingFace-Transformers-yellow.svg)](https://huggingface.co/)
[![FastAPI](https://img.shields.io/badge/FastAPI-Production-green.svg)](https://fastapi.tiangolo.com/)
[![Docker](https://img.shields.io/badge/Docker-Containerized-blue.svg)](https://www.docker.com/)
[![AWS](https://img.shields.io/badge/AWS-EC2%20%7C%20ECR-orange.svg)](https://aws.amazon.com/)
[![CI/CD](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-success.svg)](https://github.com/features/actions)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A **production-grade NLP system** for abstractive text summarization built using **Hugging Face Transformers**, **PyTorch**, and **modern MLOps practices**.

This project implements an end-to-end text summarization pipeline using Natural Language Processing (NLP) techniques. The model is deployed using AWS EC2 and AWS ECR with CI/CD automation through GitHub Actions.

This project demonstrates how to design, train, deploy, and scale a real-world machine learning system with CI/CD automation and cloud deployment.


## 🔍 Project Overview

This repository implements an **end-to-end text summarization pipeline** that covers:

- Data ingestion and validation  
- Text preprocessing and transformation  
- Model training and evaluation  
- API-based inference  
- Containerization and cloud deployment  

The system is designed with **modularity, scalability, and production-readiness** in mind.


## 🧠 Key Features

- 🔹 End-to-end ML pipeline (ingestion → inference)
- 🔹 Modular, clean architecture
- 🔹 Hugging Face PEGASUS-based abstractive summarization
- 🔹 Configuration-driven design (YAML-based)
- 🔹 CI/CD with GitHub Actions
- 🔹 Dockerized & deployed on AWS EC2
- 🔹 Logging, validation, and monitoring support


## 🏗️ Project Structure
```text
End-to-End-Text-Summarization-Project/
│
├── .github/
│   └── workflows/
│       └── main.yaml                     # CI/CD pipeline (GitHub Actions)
│
├── config/
│   └── config.yaml                      # Project configuration file
│
├── research/
│   ├── 01_data_ingestion.ipynb               
│   ├── 02_data_validation.ipynb              
│   ├── 03_data_transformation.ipynb          
│   ├── 04_model_trainer.ipynb
│   ├── 05_model_evaluation.ipynb
│   ├── text_summarization.ipynb             
│   └── trials.ipynb
│
├── src/
    └── textSummarizer
        ├── __init__.py
        ├── components/
        │   ├── __init__.py
        │   ├── data_ingestion.py               # Dataset download & extraction
        │   ├── data_validation.py              # Dataset validation logic
        │   ├── data_transformation.py          # Tokenization & preprocessing
        │   ├── model_trainer.py                # Model training logic
        │   └── model_evaluation.py             # Evaluation and metrics
        │
        ├── config/
        │   ├── __init__.py                      
        │   └── configuration.py                      
        │
        ├── constants/
        │   └── __init__.py                      # Global constants
        │
        ├── entity/
        │   └── __init__.py                      # Dataclass-based configuration schemas
        │
        ├── logging/
        │   └── __init__.py                      
        │
        ├── pipeline/
        │   ├── __init__.py      
        │   ├── prediction.py     
        │   ├── stage_01_data_ingestion.py      # Data ingestion pipeline
        │   ├── stage_02_data_validation.py     # Data validation pipeline
        │   ├── stage_03_data_transformation.py # Data transformation pipeline
        │   ├── stage_04_model_trainer.py       # Model training pipeline
        │   └── stage_05_model_evaluation.py    # Model evaluation pipeline
        │
        └── utils/
            ├── __init__.py
            └── common.py                        # Utility functions (I/O, helpers)
│
├── .gitignore                          # Git ignore rules
├── app.py                              # FastAPI inference service
├── Dockerfile                          # Container configuration
├── LICENSE                             # license
├── main.py                             # Orchestrates full pipeline execution
├── params.yaml                         # Training & hyperparameter config
├── README.md                           # Project documentation
├── requirements.txt                    # Python dependencies
├── setup.py                            # Package setup
└── template.py
```


## ⚙️ Tech Stack

| Category | Tools |
|------|------|
| Language | Python |
| NLP | Hugging Face Transformers (PEGASUS) |
| Training | PyTorch |
| Evaluation | ROUGE |
| API | FastAPI |
| MLOps | Docker, GitHub Actions |
| Cloud | AWS EC2 & ECR |
| Configuration | YAML |
| Logging | Python Logging |


## 🔄 Pipeline Workflow

1. **Data Ingestion** – Downloads and extracts dataset  
2. **Data Validation** – Ensures data integrity  
3. **Data Transformation** – Tokenization & preprocessing  
4. **Model Training** – Fine-tuning PEGASUS  
5. **Model Evaluation** – ROUGE metrics  
6. **Inference** – Real-time summarization via REST API  


## 🚀 Running Locally

### 1️⃣ Clone the Repository
```bash
git clone https://github.com/shaadclt/End-to-End-Text-Summarization-Project.git
cd End-to-End-Text-Summarization-Project
```

### 2️⃣ Create Virtual Environment
```bash
conda create -n summarizer python=3.9 -y
conda activate summarizer
```

### 3️⃣ Install Dependencies
```bash
pip install -r requirements.txt
```

### 4️⃣ Run the Application
```bash
uvicorn app:app --host 0.0.0.0 --port 8080
```

Access the API at:
👉 `http://localhost:8080`

## 🐳 Docker Deployment
```bash
docker build -t text-summarizer .
docker run -p 8080:8080 text-summarizer
```

## ☁️ Cloud Deployment (AWS)

- Dockerized and pushed to Amazon ECR
- Deployed on EC2
- CI/CD via GitHub Actions
- Automated build, test, and deploy pipeline

## 👤 Author

**Mohamed Shaad**

Machine Learning Engineer


