# 📋 Cahier de Charge — Smart Log Analyzer

## 🎯 Project: Real-Time Log Anomaly Detection with AWS & MLOps

**Version:** 2.0 (Revised - Realistic Timeline)  
**Author:** Houssam Ibnchakroune  
**Date:** January 2026  
**Target Role:** Data Engineer / ML Engineer  
**Timeline:** 8 weeks (part-time student schedule)  

---

## 1. Executive Summary

### 1.1 Project Vision
Build a **production-ready log analytics platform** that demonstrates:
1. Real-time data pipeline (Kafka → PostgreSQL)
2. ML-based anomaly detection with model versioning (MLflow)
3. Cloud integration (AWS S3, Lambda, DynamoDB)
4. Infrastructure as Code (Terraform basics)
5. Production API (FastAPI) + Monitoring (Grafana + Prometheus)

### 1.2 Why This Project?
| Your Current Gap | How This Project Fills It |
|------------------|---------------------------|
| No Cloud Experience | AWS S3, Lambda, DynamoDB |
| No Infrastructure as Code | Terraform for AWS resources |
| No MLOps | MLflow for model versioning |
| No Production API | FastAPI with async endpoints |
| No Observability | Prometheus metrics + Grafana |

### 1.3 Realistic Expectations
| Aspect | Reality |
|--------|---------|
| Timeline | 8 weeks part-time (10-15 hours/week) |
| Complexity | Medium - challenging but achievable |
| AWS Costs | $0 (Free Tier only) |
| Bugs & Issues | Expect 20-30% of time on debugging |
| Learning Curve | 40% building, 60% learning |

---

## 2. Phased Feature Breakdown

### 📍 Phase 1: Foundation (Weeks 1-2)
**Goal:** Working local pipeline - logs flowing through Kafka to PostgreSQL

| ID | Feature | Description | Difficulty |
|----|---------|-------------|------------|
| F1 | Docker Compose Setup | Kafka, Zookeeper, PostgreSQL, Redis | 🟢 Easy |
| F2 | Log Generator | Python script producing realistic JSON logs | 🟢 Easy |
| F3 | Kafka Consumer | Python consumer writing to PostgreSQL | 🟡 Medium |
| F4 | Database Schema | logs, anomalies, metrics tables | 🟢 Easy |

**Deliverable:** `docker-compose up` shows logs flowing into PostgreSQL

---

### 📍 Phase 2: ML & API (Weeks 3-4)
**Goal:** Anomaly detection working + REST API exposing data

| ID | Feature | Description | Difficulty |
|----|---------|-------------|------------|
| F5 | Feature Engineering | Extract log_rate, error_rate, patterns | 🟡 Medium |
| F6 | Isolation Forest Model | Train on historical data | 🟡 Medium |
| F7 | MLflow Integration | Track experiments, version models | 🟡 Medium |
| F8 | FastAPI Endpoints | `/health`, `/anomalies`, `/metrics` | 🟡 Medium |
| F9 | Real-time Detection | Integrate ML into Kafka consumer | 🟡 Medium |

**Deliverable:** API returns detected anomalies, model tracked in MLflow

---

### 📍 Phase 3: AWS Cloud Integration (Weeks 5-6)
**Goal:** Data flowing to AWS, Lambda processing it

| ID | Feature | Description | Difficulty |
|----|---------|-------------|------------|
| F10 | AWS Account Setup | IAM user, credentials, billing alerts | 🟡 Medium |
| F11 | S3 Export | Daily batch export of logs (Parquet) | 🟡 Medium |
| F12 | Lambda Function | Triggered by S3, computes daily stats | 🔴 Hard |
| F13 | DynamoDB | Store daily summaries | 🟡 Medium |
| F14 | Terraform Basics | IaC for S3 bucket + DynamoDB table | 🔴 Hard |

**Deliverable:** Logs in S3, Lambda processing them, summaries in DynamoDB

---

### 📍 Phase 4: Observability & Dashboard (Week 7)
**Goal:** Full visibility into system health and metrics

| ID | Feature | Description | Difficulty |
|----|---------|-------------|------------|
| F15 | Prometheus Metrics | Instrument FastAPI + Consumer | 🟡 Medium |
| F16 | Grafana Dashboard | Log volume, errors, anomalies, latency | 🟢 Easy |
| F17 | Cost Estimation API | `/costs/estimate` endpoint | 🟡 Medium |

**Deliverable:** Grafana dashboard showing real-time system metrics

---

### 📍 Phase 5: Polish & Deploy (Week 8)
**Goal:** Production-ready, documented, deployed

| ID | Feature | Description | Difficulty |
|----|---------|-------------|------------|
| F18 | GitHub Actions CI | Lint, test, build on push | 🟡 Medium |
| F19 | API Deployment | Deploy FastAPI to Railway/Render | 🟡 Medium |
| F20 | Documentation | README, architecture diagram, demo GIF | 🟢 Easy |
| F21 | Unit Tests | Test ML model, API endpoints | 🟡 Medium |

**Deliverable:** Live API, professional README, demo video

---

## 3. Technology Stack

### 3.1 Core Technologies (You Know These)

| Layer | Technology | Your Experience |
|-------|------------|-----------------|
| Streaming | Apache Kafka | ✅ Familiar |
| Database | PostgreSQL | ✅ Familiar |
| Processing | Python 3.11+ | ✅ Familiar |
| Containers | Docker + Compose | ✅ Familiar |
| Dashboard | Grafana | ✅ Familiar |

### 3.2 New Technologies (Learning Opportunities)

| Layer | Technology | Why Learn It | Difficulty |
|-------|------------|--------------|------------|
| **API** | FastAPI | Industry standard, async, auto-docs | 🟡 Medium |
| **ML** | Scikit-learn | Simple anomaly detection | 🟡 Medium |
| **MLOps** | MLflow | Model versioning, experiment tracking | 🟡 Medium |
| **Cloud** | AWS S3 | Object storage, data lake pattern | 🟡 Medium |
| **Serverless** | AWS Lambda | Event-driven, free tier | 🔴 Hard |
| **NoSQL** | AWS DynamoDB | Fast key-value lookups | 🟡 Medium |
| **IaC** | Terraform | Infrastructure as Code (basics) | 🔴 Hard |
| **Metrics** | Prometheus | Application metrics collection | 🟡 Medium |

### 3.3 Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         SMART LOG ANALYZER v2.0                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │                      LOCAL (Docker Compose)                           │  │
│  │                                                                       │  │
│  │   ┌─────────────┐    ┌─────────────┐    ┌─────────────────────────┐  │  │
│  │   │    Log      │    │   Apache    │    │    Python Consumer      │  │  │
│  │   │  Generator  │───▶│   Kafka     │───▶│  + Isolation Forest     │  │  │
│  │   │  (Python)   │    │ (Redpanda)  │    │  + MLflow Tracking      │  │  │
│  │   └─────────────┘    └─────────────┘    └───────────┬─────────────┘  │  │
│  │                                                     │                │  │
│  │                      ┌──────────────────────────────┼────────┐       │  │
│  │                      │                              │        │       │  │
│  │                      ▼                              ▼        ▼       │  │
│  │               ┌─────────────┐              ┌──────────┐ ┌────────┐  │  │
│  │               │ PostgreSQL  │              │  MLflow  │ │  S3    │  │  │
│  │               │   (Logs)    │              │ (Models) │ │ Export │  │  │
│  │               └──────┬──────┘              └──────────┘ └────┬───┘  │  │
│  │                      │                                       │      │  │
│  │   ┌──────────────────┼───────────────────────────────────────┘      │  │
│  │   │                  │                                              │  │
│  │   ▼                  ▼                                              │  │
│  │ ┌─────────────┐  ┌─────────────┐  ┌─────────────┐                  │  │
│  │ │  FastAPI    │  │  Grafana    │  │ Prometheus  │                  │  │
│  │ │  (REST)     │  │ (Dashboard) │◀─│  (Metrics)  │                  │  │
│  │ └─────────────┘  └─────────────┘  └─────────────┘                  │  │
│  │                                                                     │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │                      AWS CLOUD (Free Tier)                            │  │
│  │                      Provisioned via Terraform                        │  │
│  │                                                                       │  │
│  │   ┌─────────────┐    ┌─────────────┐    ┌─────────────┐              │  │
│  │   │     S3      │───▶│   Lambda    │───▶│  DynamoDB   │              │  │
│  │   │   (Logs)    │    │ (Processor) │    │ (Summaries) │              │  │
│  │   │   5 GB Free │    │  1M Free    │    │  25 GB Free │              │  │
│  │   └─────────────┘    └─────────────┘    └─────────────┘              │  │
│  │                                                                       │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │                      CI/CD (GitHub Actions)                           │  │
│  │   Push → Lint → Test → Build Docker → Deploy to Railway              │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 4. Data Models

### 4.1 Log Event Schema (Kafka Message)

```json
{
  "timestamp": "2026-01-15T10:30:00Z",
  "level": "ERROR",
  "service": "payment-service",
  "host": "server-01",
  "message": "Database connection timeout",
  "metadata": {
    "request_id": "abc-123",
    "user_id": "user-456",
    "duration_ms": 5000
  }
}
```

### 4.2 PostgreSQL Schema

```sql
-- Raw logs table
CREATE TABLE logs (
    id BIGSERIAL PRIMARY KEY,
    timestamp TIMESTAMPTZ NOT NULL,
    level VARCHAR(10) NOT NULL,
    service VARCHAR(100) NOT NULL,
    host VARCHAR(100),
    message TEXT,
    metadata JSONB,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Detected anomalies
CREATE TABLE anomalies (
    id BIGSERIAL PRIMARY KEY,
    detected_at TIMESTAMPTZ NOT NULL,
    anomaly_type VARCHAR(50) NOT NULL,
    service VARCHAR(100),
    severity VARCHAR(20),
    description TEXT,
    metric_value FLOAT,
    threshold FLOAT,
    model_version VARCHAR(50),  -- MLflow model version
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Hourly metrics (for dashboards)
CREATE TABLE hourly_metrics (
    id BIGSERIAL PRIMARY KEY,
    hour TIMESTAMPTZ NOT NULL,
    service VARCHAR(100),
    log_count INTEGER,
    error_count INTEGER,
    anomaly_count INTEGER,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Indexes
CREATE INDEX idx_logs_timestamp ON logs(timestamp);
CREATE INDEX idx_logs_service ON logs(service);
CREATE INDEX idx_anomalies_detected_at ON anomalies(detected_at);
```

### 4.3 DynamoDB Schema

```
Table: daily_summaries
Partition Key: date (String) — "2026-01-15"
Sort Key: service (String)

Attributes:
- date: "2026-01-15"
- service: "payment-service"
- total_logs: 50000
- error_count: 150
- anomaly_count: 3
- processed_at: "2026-01-15T00:05:00Z"
```

---

## 5. API Specification

### 5.1 Endpoints

| Method | Endpoint | Description | Phase |
|--------|----------|-------------|-------|
| GET | `/health` | Health check + dependencies | Phase 2 |
| GET | `/api/v1/anomalies` | List recent anomalies | Phase 2 |
| GET | `/api/v1/anomalies/{id}` | Get anomaly details | Phase 2 |
| GET | `/api/v1/metrics/hourly` | Hourly log metrics | Phase 2 |
| GET | `/api/v1/costs/estimate` | Current month cost estimate | Phase 4 |
| GET | `/metrics` | Prometheus metrics endpoint | Phase 4 |

### 5.2 Example Responses

**GET /api/v1/anomalies**
```json
{
  "anomalies": [
    {
      "id": 1,
      "detected_at": "2026-01-15T10:30:00Z",
      "type": "error_rate_spike",
      "service": "payment-service",
      "severity": "high",
      "description": "Error rate increased 300% in last 5 minutes",
      "metric_value": 0.15,
      "threshold": 0.05,
      "model_version": "1.2.0"
    }
  ],
  "total": 1,
  "page": 1
}
```

**GET /api/v1/costs/estimate**
```json
{
  "period": "2026-01",
  "estimated_costs_usd": {
    "s3": 0.00,
    "lambda": 0.00,
    "dynamodb": 0.00,
    "total": 0.00
  },
  "free_tier_usage": {
    "s3_storage_gb": {"used": 2.5, "limit": 5},
    "lambda_invocations": {"used": 45000, "limit": 1000000}
  },
  "status": "within_free_tier"
}
```

---

## 6. Machine Learning Component

### 6.1 Algorithm: Isolation Forest

**Why Isolation Forest?**
- ✅ Works with streaming data
- ✅ Unsupervised (no labeled data needed)
- ✅ Fast inference (< 10ms)
- ✅ Easy to explain in interviews
- ✅ Scikit-learn has stable implementation

### 6.2 Features

| Feature | Description | Window |
|---------|-------------|--------|
| `log_rate` | Logs per minute | 5 min rolling |
| `error_rate` | Errors / Total logs | 5 min rolling |
| `avg_message_length` | Average message size | 5 min rolling |
| `unique_services` | Number of unique services | 5 min rolling |
| `hour_of_day` | Current hour (0-23) | Point-in-time |

### 6.3 MLflow Integration

```python
import mlflow
from sklearn.ensemble import IsolationForest

# Start experiment
mlflow.set_experiment("log-anomaly-detection")

with mlflow.start_run():
    # Log parameters
    mlflow.log_param("contamination", 0.05)
    mlflow.log_param("n_estimators", 100)
    
    # Train model
    model = IsolationForest(contamination=0.05, n_estimators=100)
    model.fit(X_train)
    
    # Log metrics
    mlflow.log_metric("training_samples", len(X_train))
    
    # Save model
    mlflow.sklearn.log_model(model, "anomaly_model")
```

### 6.4 Anomaly Types

| Type | Trigger | Severity |
|------|---------|----------|
| `error_rate_spike` | Error rate > 3x baseline | 🔴 High |
| `log_volume_spike` | Volume > 5x baseline | 🟡 Medium |
| `log_volume_drop` | Volume < 0.1x baseline | 🟡 Medium |

---

## 7. Terraform (Infrastructure as Code)

### 7.1 What We'll Provision

```hcl
# terraform/main.tf

provider "aws" {
  region = "eu-west-1"
}

# S3 Bucket for logs
resource "aws_s3_bucket" "logs" {
  bucket = "smart-log-analyzer-logs-houssam"
}

# DynamoDB Table for summaries
resource "aws_dynamodb_table" "summaries" {
  name           = "daily_summaries"
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "date"
  range_key      = "service"

  attribute {
    name = "date"
    type = "S"
  }

  attribute {
    name = "service"
    type = "S"
  }
}

# Lambda function (uploaded separately)
resource "aws_lambda_function" "processor" {
  function_name = "log-processor"
  runtime       = "python3.11"
  handler       = "handler.main"
  # ... more config
}
```

### 7.2 Commands to Learn

```bash
terraform init      # Initialize providers
terraform plan      # Preview changes
terraform apply     # Create resources
terraform destroy   # Clean up (important for free tier!)
```

---

## 8. Detailed Timeline

### Week 1: Environment Setup
| Day | Task | Hours | Deliverable |
|-----|------|-------|-------------|
| 1 | Install Docker, VS Code, Python 3.11 | 2h | Dev environment ready |
| 2-3 | Create docker-compose.yml (Kafka, PostgreSQL) | 4h | Containers running |
| 4-5 | Build log generator script | 3h | Fake logs producing |
| 6-7 | Debug, test, document | 3h | Phase 1a complete |

**Buffer:** 2-3 hours for troubleshooting Kafka on Windows

### Week 2: Kafka Consumer + Database
| Day | Task | Hours | Deliverable |
|-----|------|-------|-------------|
| 1-2 | Create PostgreSQL schema | 2h | Tables created |
| 3-5 | Build Kafka consumer | 5h | Logs in PostgreSQL |
| 6-7 | Test end-to-end pipeline | 3h | Phase 1 complete ✅ |

**Milestone:** `docker-compose up` → logs visible in database

### Week 3: ML Model
| Day | Task | Hours | Deliverable |
|-----|------|-------|-------------|
| 1-2 | Feature engineering | 3h | Features extracted |
| 3-4 | Train Isolation Forest | 3h | Model trained |
| 5-6 | Setup MLflow locally | 3h | Model versioned |
| 7 | Integrate into consumer | 3h | Anomalies detected |

**Buffer:** MLflow can be tricky to set up, allocate extra time

### Week 4: FastAPI
| Day | Task | Hours | Deliverable |
|-----|------|-------|-------------|
| 1-2 | FastAPI project setup | 2h | `/health` working |
| 3-4 | `/anomalies` endpoint | 3h | Returns anomalies |
| 5-6 | `/metrics` endpoint | 2h | Returns hourly data |
| 7 | Add tests | 3h | Phase 2 complete ✅ |

**Milestone:** API accessible at `localhost:8000/docs`

### Week 5: AWS Account + S3
| Day | Task | Hours | Deliverable |
|-----|------|-------|-------------|
| 1-2 | Create AWS account, IAM user | 2h | Credentials ready |
| 3 | Set billing alert at $1 | 1h | No surprise bills |
| 4-5 | Create S3 bucket manually first | 2h | Bucket exists |
| 6-7 | Python script to export to S3 | 4h | Logs in S3 |

**Important:** ALWAYS set billing alerts before doing anything!

### Week 6: Lambda + DynamoDB + Terraform
| Day | Task | Hours | Deliverable |
|-----|------|-------|-------------|
| 1-2 | Create Lambda manually (console) | 3h | Lambda working |
| 3-4 | Setup DynamoDB table | 2h | Table created |
| 5-6 | Install Terraform, write config | 4h | IaC for S3 + DynamoDB |
| 7 | Test terraform apply/destroy | 2h | Phase 3 complete ✅ |

**Buffer:** Lambda + S3 triggers can be frustrating, allocate extra time

### Week 7: Observability
| Day | Task | Hours | Deliverable |
|-----|------|-------|-------------|
| 1-2 | Add Prometheus to docker-compose | 2h | Prometheus running |
| 3-4 | Instrument FastAPI with metrics | 3h | Metrics exposed |
| 5-6 | Create Grafana dashboard | 3h | Dashboard live |
| 7 | Add cost estimation endpoint | 2h | Phase 4 complete ✅ |

### Week 8: Polish & Deploy
| Day | Task | Hours | Deliverable |
|-----|------|-------|-------------|
| 1-2 | GitHub Actions CI pipeline | 3h | Auto-tests on push |
| 3-4 | Deploy to Railway/Render | 3h | Public URL |
| 5-6 | Write README, record demo | 3h | Professional docs |
| 7 | Final review, cleanup | 2h | PROJECT COMPLETE ✅ |

---

## 9. Deliverables Checklist

### 9.1 Code Structure

```
smart-log-analyzer/
├── docker-compose.yml          # One-command local setup
├── .env.example                 # Environment variables template
├── README.md                    # Professional documentation
│
├── log_generator/
│   ├── __init__.py
│   └── generator.py            # Realistic log simulator
│
├── kafka_consumer/
│   ├── __init__.py
│   ├── consumer.py             # Kafka → PostgreSQL
│   └── ml_detector.py          # Anomaly detection
│
├── api/
│   ├── __init__.py
│   ├── main.py                 # FastAPI application
│   ├── routers/
│   │   ├── anomalies.py
│   │   ├── metrics.py
│   │   └── costs.py
│   └── models/
│       └── schemas.py          # Pydantic models
│
├── ml/
│   ├── train.py                # Model training script
│   ├── features.py             # Feature engineering
│   └── models/                 # Saved models
│
├── aws/
│   ├── lambda_handler.py       # Lambda function code
│   └── s3_exporter.py          # S3 upload script
│
├── terraform/
│   ├── main.tf                 # AWS resources
│   ├── variables.tf
│   └── outputs.tf
│
├── grafana/
│   └── dashboard.json          # Dashboard config
│
├── .github/
│   └── workflows/
│       └── ci.yml              # GitHub Actions
│
├── tests/
│   ├── test_api.py
│   ├── test_consumer.py
│   └── test_ml.py
│
└── docs/
    ├── Technical requirements.md
    ├── ARCHITECTURE.md
    └── API.md
```

### 9.2 Deliverables by Phase

| Phase | Deliverable | Status |
|-------|-------------|--------|
| 1 | Docker Compose + Log Generator | ⬜ |
| 1 | Kafka Consumer + PostgreSQL | ⬜ |
| 2 | ML Model + MLflow | ⬜ |
| 2 | FastAPI Endpoints | ⬜ |
| 3 | AWS S3 Export | ⬜ |
| 3 | Lambda + DynamoDB | ⬜ |
| 3 | Terraform Config | ⬜ |
| 4 | Prometheus + Grafana | ⬜ |
| 5 | GitHub Actions CI | ⬜ |
| 5 | Deployed API | ⬜ |
| 5 | README + Demo | ⬜ |

---

## 10. Success Criteria

### 10.1 Technical Metrics

| Metric | Target | How to Verify |
|--------|--------|---------------|
| Local setup time | < 3 minutes | Time `docker-compose up` |
| Log ingestion | > 50 logs/second | Test with generator |
| Anomaly detection | < 500ms latency | Measure in consumer |
| API response | < 300ms | Test with curl |
| S3 export | Daily, automatic | Check S3 bucket |

### 10.2 Portfolio Metrics

| Metric | Target |
|--------|--------|
| README quality | Professional, with architecture diagram + GIF |
| Code quality | Linted (black/ruff), typed (mypy), documented |
| Test coverage | > 50% for API and ML modules |
| Demo video | 60-90 seconds showing full flow |

### 10.3 Interview Talking Points

After completing this project, you can confidently discuss:

1. **"Why Kafka over direct writes?"** → Decoupling, buffering, replay, scalability
2. **"Why Isolation Forest?"** → Unsupervised, fast, interpretable
3. **"Why MLflow?"** → Model versioning, experiment tracking, reproducibility
4. **"Why Terraform?"** → Infrastructure as Code, reproducible, team collaboration
5. **"Why S3 + Lambda + DynamoDB?"** → Serverless, cost-effective, event-driven
6. **"How would you scale this?"** → Kafka partitions, Lambda concurrency, read replicas

---

## 11. Risk Mitigation

| Risk | Probability | Mitigation |
|------|-------------|------------|
| Kafka issues on Windows | High | Use Redpanda instead (lighter) |
| AWS costs exceed $0 | Medium | Billing alerts at $1, use free tier calc |
| MLflow setup fails | Medium | Fallback: save models with joblib only |
| Lambda deployment fails | Medium | Test in console first, then automate |
| Time overrun | High | Prioritize Phases 1-2, defer Phase 3-5 features |
| Burnout | Medium | Max 15 hours/week, take breaks |

---

## 12. Learning Resources

### Phase 1-2 (Local Development)
- [Kafka Python Tutorial](https://kafka-python.readthedocs.io/)
- [FastAPI Official Docs](https://fastapi.tiangolo.com/)
- [Docker Compose for Data Engineering](https://docs.docker.com/compose/)

### Phase 3 (AWS)
- [AWS Free Tier Calculator](https://calculator.aws/)
- [S3 + Lambda Tutorial](https://docs.aws.amazon.com/lambda/latest/dg/with-s3.html)
- [Terraform AWS Provider Docs](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)

### Phase 4 (Observability)
- [Prometheus + Python](https://github.com/prometheus/client_python)
- [Grafana Dashboards](https://grafana.com/docs/grafana/latest/dashboards/)

### MLOps
- [MLflow Quickstart](https://mlflow.org/docs/latest/quickstart.html)
- [Scikit-learn Isolation Forest](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.IsolationForest.html)

---

## 13. Post-Project Extensions (Optional)

After completing the MVP, consider adding for extra portfolio value:

| Extension | Effort | Value |
|-----------|--------|-------|
| Slack/Discord alerts | 1 day | High (real-world feature) |
| Kubernetes deployment | 1 week | Very High (fills K8s gap) |
| dbt for data transformations | 2-3 days | High (you have dbt experience) |
| Full ELK stack | 1 week | Medium (complex setup) |
| Multi-cloud (add GCP) | 2 weeks | Medium (scope creep risk) |

---

## 14. Notes for Reviewer

### What Makes This Project Stand Out:
1. **End-to-end pipeline** — Not just a tutorial, full data flow
2. **MLOps practices** — Model versioning with MLflow
3. **Infrastructure as Code** — Terraform for AWS resources
4. **Observability** — Prometheus metrics + Grafana
5. **Production API** — FastAPI with proper structure
6. **Cloud Integration** — Real AWS services (free tier)

### Skills Demonstrated:
- Data Engineering: Kafka, PostgreSQL, ETL
- Cloud: AWS S3, Lambda, DynamoDB
- MLOps: MLflow, model serving
- DevOps: Docker, Terraform, GitHub Actions
- Backend: FastAPI, REST API design
- Observability: Prometheus, Grafana

---

**Document Status:** ✅ Ready for Implementation  
**Next Step:** Create project skeleton and docker-compose.yml  
**Estimated Completion:** 8 weeks (part-time)

---

*Last Updated: January 2026*
