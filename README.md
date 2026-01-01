# 🚀 End-to-End Text Summarization System (Production-Ready)

A **production-grade NLP system** for abstractive text summarization built using **Hugging Face Transformers**, **PyTorch**, and **modern MLOps practices**. 

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

