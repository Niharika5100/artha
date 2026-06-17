# Artha — Regulatory Filing Intelligence Pipeline

Artha is a serverless cloud pipeline that ingests bank regulatory filings, classifies
their risk level using a trained model, indexes them for retrieval, and exposes a
searchable interface for risk analysts.

The name is Sanskrit for wealth, finance, and meaning — a direct fit for a system
built around financial filings and the meaning buried inside them.

---
# Project Overview

Regulatory teams at banks deal with a constant stream of filings and circulars. Artha
automates the ingestion and triage of these documents — fetching new filings on a
schedule, tagging risk level, and making the entire corpus queryable in natural language.

---
# Core Capabilities

Scheduled Ingestion → Document Storage → Chunking & Embedding → Risk Classification →
Indexed Storage → RAG Query Interface → Dashboard

---
# System Architecture

EventBridge (scheduled trigger)

       ↓
       
Lambda — Fetch Filings (regulatory API)

       ↓
       
S3 (raw documents)

       ↓
       
Lambda — Process & Embed

       ↓
       
RDS / pgvector (embeddings)  +  DynamoDB (risk tags)

       ↓
       
ML Classifier (risk level: Low / Medium / High / Action Required)

       ↓
       
FastAPI — RAG Query Endpoint

       ↓
       
Dashboard (Streamlit / React)


---
# Technology Stack

Cloud:
- AWS Lambda, S3, EventBridge, RDS (pgvector), DynamoDB, ECR
RAG:
- LlamaIndex, pgvector, hybrid retrieval
ML:
- scikit-learn / HuggingFace DistilBERT (document risk classification)
Experiment Tracking:
- MLflow
Backend:
- FastAPI
Dashboard:
- Streamlit
Infrastructure:
- Terraform

---
# Key Modules

Ingestion
- Scheduled Lambda fetch from regulatory filing APIs
- Raw document storage in S3

Processing
- Chunking and embedding generation
- ML-based risk classification with experiment tracking

Query Layer
- RAG-based natural language search across all ingested filings
- Risk-tagged dashboard view

---
# API Endpoints

Health check
GET /health

Query filings
POST /query
Body: { "question": "What did the RBI say about crypto exposure in Q1?" }

List recent filings by risk level
GET /filings?risk=HIGH

---
# Installation

git clone https://github.com/yourusername/artha.git
cd artha
cp .env.example .env
docker-compose up -d
pip install -r requirements.txt
uvicorn app.main:app --reload

For the cloud pipeline:
cd infra
terraform init
terraform apply

---
# Project Status

In development — local ingestion and classification pipeline working. AWS Lambda
deployment and Terraform infra in progress.

---
# Author

Built to learn cloud-native MLOps architecture — serverless pipelines, scheduled
ingestion, and combining a classification model with RAG over the same document set.
