# 📋 Cahier de Charge — Smart Log Analyzer

## 🎯 Project: Real-Time Log Anomaly Detection with AWS Cost Estimation

**Version:** 1.0  
**Author:** Houssam Ibnchakroune  
**Date:** December 2024  
**Target Role:** Data Engineer / ML Engineer  

---

## 1. Executive Summary

### 1.1 Project Vision
Build a **production-ready log analytics platform** that:
1. Ingests application logs in real-time via Kafka
2. Detects anomalies using ML (error spikes, unusual patterns)
3. Stores results in PostgreSQL + AWS (S3/DynamoDB)
4. Estimates monthly cloud costs based on usage metrics
5. Exposes insights via REST API + Grafana dashboard

### 1.2 Business Value
| Problem | Solution |
|---------|----------|
| DevOps teams miss critical errors buried in logs | Real-time anomaly alerts |
| Cloud bills surprise teams at month-end | Proactive cost estimation |
| No visibility into infrastructure health | Live Grafana dashboard |
| Manual log analysis is slow | Automated ML-based detection |

### 1.3 Target Audience
- DevOps Engineers monitoring microservices
- Platform Teams managing cloud infrastructure
- FinOps Teams tracking cloud spending

---

## 2. Functional Requirements

### 2.1 Core Features (MVP — Must Have)

| ID | Feature | Description | Priority |
|----|---------|-------------|----------|
| F1 | Log Ingestion | Accept logs via Kafka topic in JSON format | 🔴 Critical |
| F2 | Log Parsing | Extract timestamp, level, service, message, metadata | 🔴 Critical |
| F3 | Anomaly Detection | Detect error rate spikes, unusual log volume | 🔴 Critical |
| F4 | Data Storage | Store processed logs in PostgreSQL | 🔴 Critical |
| F5 | AWS S3 Export | Archive raw logs daily to S3 bucket | 🔴 Critical |
| F6 | AWS Lambda Trigger | Process S3 uploads, compute daily stats | 🔴 Critical |
| F7 | AWS DynamoDB | Store anomaly summaries for fast lookup | 🔴 Critical |
| F8 | REST API | Expose `/anomalies`, `/costs`, `/health` endpoints | 🔴 Critical |
| F9 | Grafana Dashboard | Visualize log volume, errors, anomalies | 🔴 Critical |
| F10 | Cost Estimation | Estimate monthly AWS bill from usage metrics | 🔴 Critical |

### 2.2 Nice-to-Have Features (Post-MVP)

| ID | Feature | Description | Priority |
|----|---------|-------------|----------|
| F11 | Slack Alerts | Send anomaly notifications to Slack channel | 🟡 Medium |
| F12 | What-If Scenarios | Simulate cost with 2x/5x traffic | 🟡 Medium |
| F13 | Log Search API | Full-text search on historical logs | 🟢 Low |
| F14 | Multi-tenant | Support multiple applications/services | 🟢 Low |

---

## 3. Technical Requirements

### 3.1 Technology Stack

| Layer | Technology | Justification |
|-------|------------|---------------|
| **Streaming** | Apache Kafka | Already in your skillset, industry standard |
| **Processing** | Python 3.11+ | Fast development, ML libraries |
| **ML** | Scikit-learn (Isolation Forest) | Simple, explainable anomaly detection |
| **Database** | PostgreSQL 15 | Reliable, you know it well |
| **Time-Series** | TimescaleDB extension (optional) | Better for metrics queries |
| **API** | FastAPI | Modern, async, auto-docs |
| **Dashboard** | Grafana | Already in your skillset |
| **Cloud Storage** | AWS S3 | Industry standard, free tier |
| **Serverless** | AWS Lambda | Event-driven, free tier |
| **NoSQL** | AWS DynamoDB | Fast key-value lookups |
| **Containers** | Docker + Docker Compose | Local development |
| **CI/CD** | GitHub Actions | Free for public repos |
| **Deployment** | Railway.app / Render.com | Free tier for API |

### 3.2 Infrastructure Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        SMART LOG ANALYZER                               │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                    LOCAL (Docker Compose)                        │   │
│  │                                                                  │   │
│  │   ┌──────────────┐     ┌──────────────┐     ┌──────────────┐    │   │
│  │   │    Log       │     │    Apache    │     │   Python     │    │   │
│  │   │  Generator   │────▶│    Kafka     │────▶│  Consumer    │    │   │
│  │   │  (Python)    │     │  (Zookeeper) │     │  + ML Model  │    │   │
│  │   └──────────────┘     └──────────────┘     └──────┬───────┘    │   │
│  │                                                    │            │   │
│  │                              ┌─────────────────────┼────────┐   │   │
│  │                              │                     │        │   │   │
│  │                              ▼                     ▼        │   │   │
│  │                        ┌──────────┐         ┌──────────┐   │   │   │
│  │                        │PostgreSQL│         │  AWS S3  │   │   │   │
│  │                        │  (Logs)  │         │ (Export) │   │   │   │
│  │                        └────┬─────┘         └────┬─────┘   │   │   │
│  │                             │                    │         │   │   │
│  │                             ▼                    │         │   │   │
│  │                        ┌──────────┐              │         │   │   │
│  │                        │ Grafana  │              │         │   │   │
│  │                        │Dashboard │              │         │   │   │
│  │                        └──────────┘              │         │   │   │
│  │                                                  │         │   │   │
│  │   ┌──────────────┐                               │         │   │   │
│  │   │   FastAPI    │◀──────────────────────────────┘         │   │   │
│  │   │  (REST API)  │                                         │   │   │
│  │   └──────────────┘                                         │   │   │
│  │                                                             │   │   │
│  └─────────────────────────────────────────────────────────────┘   │   │
│                                                                     │   │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                      AWS (Free Tier)                            │   │
│  │                                                                  │   │
│  │   ┌──────────┐     ┌──────────┐     ┌──────────┐               │   │
│  │   │   S3     │────▶│  Lambda  │────▶│ DynamoDB │               │   │
│  │   │ (Logs)   │     │(Trigger) │     │(Summary) │               │   │
│  │   └──────────┘     └──────────┘     └──────────┘               │   │
│  │       5GB              1M req           25GB                    │   │
│  │      FREE              FREE             FREE                    │   │
│  │                                                                  │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### 3.3 Data Flow

```
1. LOG GENERATION
   Application → JSON log → Kafka topic "logs"
   
2. STREAM PROCESSING  
   Kafka → Python Consumer → Parse + Enrich → PostgreSQL
                          → ML Anomaly Check → If anomaly → Alert table
                          
3. DAILY EXPORT (Batch)
   PostgreSQL → Daily aggregates → S3 bucket (Parquet/JSON)
   
4. SERVERLESS PROCESSING
   S3 upload → Lambda trigger → Compute stats → DynamoDB
   
5. API ACCESS
   Client → FastAPI → Query PostgreSQL/DynamoDB → JSON response
   
6. VISUALIZATION
   Grafana → Query PostgreSQL → Dashboard panels
```

---

## 4. Data Models

### 4.1 Log Event Schema (Kafka Message)

```json
{
  "timestamp": "2024-12-29T10:30:00Z",
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

### 4.2 PostgreSQL Tables

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

-- Anomalies table
CREATE TABLE anomalies (
    id BIGSERIAL PRIMARY KEY,
    detected_at TIMESTAMPTZ NOT NULL,
    anomaly_type VARCHAR(50) NOT NULL,
    service VARCHAR(100),
    severity VARCHAR(20),
    description TEXT,
    metric_value FLOAT,
    threshold FLOAT,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Hourly metrics (for cost estimation)
CREATE TABLE hourly_metrics (
    id BIGSERIAL PRIMARY KEY,
    hour TIMESTAMPTZ NOT NULL,
    service VARCHAR(100),
    log_count INTEGER,
    error_count INTEGER,
    avg_message_size INTEGER,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Indexes
CREATE INDEX idx_logs_timestamp ON logs(timestamp);
CREATE INDEX idx_logs_service ON logs(service);
CREATE INDEX idx_logs_level ON logs(level);
CREATE INDEX idx_anomalies_detected_at ON anomalies(detected_at);
```

### 4.3 DynamoDB Table

```
Table: anomaly_summaries
Partition Key: date (String) — e.g., "2024-12-29"
Sort Key: service (String)

Attributes:
- date: "2024-12-29"
- service: "payment-service"
- total_logs: 50000
- error_count: 150
- anomaly_count: 3
- estimated_s3_cost_usd: 0.023
- estimated_lambda_cost_usd: 0.0001
- estimated_dynamodb_cost_usd: 0.0005
- total_estimated_cost_usd: 0.0236
```

---

## 5. API Specification

### 5.1 Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/health` | Health check |
| GET | `/api/v1/anomalies` | List recent anomalies |
| GET | `/api/v1/anomalies/{id}` | Get anomaly details |
| GET | `/api/v1/metrics/hourly` | Hourly log metrics |
| GET | `/api/v1/costs/estimate` | Current month cost estimate |
| GET | `/api/v1/costs/forecast` | Next month forecast |
| GET | `/api/v1/costs/what-if` | Simulate cost scenarios |

### 5.2 Example Responses

**GET /api/v1/anomalies**
```json
{
  "anomalies": [
    {
      "id": 1,
      "detected_at": "2024-12-29T10:30:00Z",
      "type": "error_rate_spike",
      "service": "payment-service",
      "severity": "high",
      "description": "Error rate increased 300% in last 5 minutes",
      "metric_value": 0.15,
      "threshold": 0.05
    }
  ],
  "total": 1,
  "page": 1
}
```

**GET /api/v1/costs/estimate**
```json
{
  "period": "2024-12",
  "current_usage": {
    "s3_storage_gb": 2.5,
    "s3_requests": 150000,
    "lambda_invocations": 45000,
    "lambda_duration_ms": 2250000,
    "dynamodb_read_units": 50000,
    "dynamodb_write_units": 45000,
    "dynamodb_storage_gb": 0.5
  },
  "estimated_costs_usd": {
    "s3": 0.06,
    "lambda": 0.00,
    "dynamodb": 0.00,
    "total": 0.06
  },
  "projected_month_end_usd": 0.12,
  "free_tier_remaining": {
    "s3_storage_gb": 2.5,
    "lambda_invocations": 955000
  },
  "notes": "All services within free tier limits"
}
```

**GET /api/v1/costs/what-if?traffic_multiplier=5**
```json
{
  "scenario": "5x traffic increase",
  "projected_usage": {
    "s3_storage_gb": 12.5,
    "lambda_invocations": 225000,
    "dynamodb_write_units": 225000
  },
  "projected_costs_usd": {
    "s3": 0.17,
    "lambda": 0.00,
    "dynamodb": 0.28,
    "total": 0.45
  },
  "warning": "Will exceed free tier for S3 storage"
}
```

---

## 6. Machine Learning Component

### 6.1 Anomaly Detection Approach

**Algorithm:** Isolation Forest (scikit-learn)

**Why Isolation Forest?**
- Works well with streaming data
- No need for labeled anomalies (unsupervised)
- Fast inference (important for real-time)
- Easy to explain to recruiters

### 6.2 Features for Anomaly Detection

| Feature | Description | Window |
|---------|-------------|--------|
| `log_rate` | Logs per minute | 5 min rolling |
| `error_rate` | Errors / Total logs | 5 min rolling |
| `avg_message_length` | Average message size | 5 min rolling |
| `unique_hosts` | Number of unique hosts | 5 min rolling |
| `hour_of_day` | Current hour (0-23) | Point-in-time |
| `day_of_week` | Current day (0-6) | Point-in-time |

### 6.3 Model Pipeline

```python
# Training (offline, on historical data)
from sklearn.ensemble import IsolationForest
from sklearn.preprocessing import StandardScaler

# 1. Extract features from historical logs
features = extract_features(historical_logs)

# 2. Scale features
scaler = StandardScaler()
features_scaled = scaler.fit_transform(features)

# 3. Train model
model = IsolationForest(
    contamination=0.05,  # Expect 5% anomalies
    random_state=42,
    n_estimators=100
)
model.fit(features_scaled)

# 4. Save model + scaler
joblib.dump(model, 'models/anomaly_model.pkl')
joblib.dump(scaler, 'models/scaler.pkl')
```

```python
# Inference (real-time, in Kafka consumer)
def detect_anomaly(current_features):
    features_scaled = scaler.transform([current_features])
    prediction = model.predict(features_scaled)
    score = model.score_samples(features_scaled)
    
    is_anomaly = prediction[0] == -1
    confidence = abs(score[0])
    
    return is_anomaly, confidence
```

### 6.4 Anomaly Types to Detect

| Type | Trigger Condition | Severity |
|------|-------------------|----------|
| `error_rate_spike` | Error rate > 3x baseline | High |
| `log_volume_spike` | Log volume > 5x baseline | Medium |
| `log_volume_drop` | Log volume < 0.1x baseline | Medium |
| `new_error_pattern` | New error message cluster | Low |

---

## 7. Cost Estimation Logic

### 7.1 AWS Pricing Model (Free Tier Aware)

```python
# AWS Free Tier Limits (as of 2024)
FREE_TIER = {
    "s3": {
        "storage_gb": 5,
        "put_requests": 2000,
        "get_requests": 20000
    },
    "lambda": {
        "requests": 1_000_000,
        "compute_gb_seconds": 400_000
    },
    "dynamodb": {
        "read_capacity_units": 25,
        "write_capacity_units": 25,
        "storage_gb": 25
    }
}

# AWS Pricing (per unit, after free tier)
PRICING = {
    "s3": {
        "storage_per_gb": 0.023,
        "put_per_1000": 0.005,
        "get_per_1000": 0.0004
    },
    "lambda": {
        "per_request": 0.0000002,
        "per_gb_second": 0.0000166667
    },
    "dynamodb": {
        "read_per_unit": 0.00013,
        "write_per_unit": 0.00065,
        "storage_per_gb": 0.25
    }
}
```

### 7.2 Estimation Formula

```python
def estimate_monthly_cost(usage: dict) -> dict:
    costs = {}
    
    # S3
    s3_storage = max(0, usage["s3_storage_gb"] - FREE_TIER["s3"]["storage_gb"])
    costs["s3"] = s3_storage * PRICING["s3"]["storage_per_gb"]
    
    # Lambda
    lambda_requests = max(0, usage["lambda_invocations"] - FREE_TIER["lambda"]["requests"])
    costs["lambda"] = lambda_requests * PRICING["lambda"]["per_request"]
    
    # DynamoDB (simplified)
    costs["dynamodb"] = 0  # Usually within free tier for this project
    
    costs["total"] = sum(costs.values())
    
    return costs
```

---

## 8. Project Timeline

### Phase 1: Foundation (Week 1)
| Day | Task | Deliverable |
|-----|------|-------------|
| 1-2 | Setup Docker Compose (Kafka, Zookeeper, PostgreSQL) | `docker-compose.yml` working |
| 3-4 | Build log generator (realistic fake logs) | `log_generator.py` |
| 5-6 | Build Kafka consumer + PostgreSQL writer | `consumer.py`, tables created |
| 7 | Test end-to-end: generator → Kafka → consumer → DB | Logs visible in PostgreSQL |

### Phase 2: ML + API (Week 2)
| Day | Task | Deliverable |
|-----|------|-------------|
| 1-2 | Implement feature extraction + Isolation Forest | `ml_model.py` |
| 3-4 | Integrate ML into consumer, write anomalies to DB | Anomalies detected |
| 5-6 | Build FastAPI with `/anomalies`, `/health` | `api/main.py` |
| 7 | Add Grafana dashboard (log volume, errors, anomalies) | `grafana/dashboard.json` |

### Phase 3: AWS Integration (Week 3)
| Day | Task | Deliverable |
|-----|------|-------------|
| 1-2 | Create AWS account, S3 bucket, IAM user | AWS credentials configured |
| 3-4 | Add S3 export to consumer (daily batch) | Logs in S3 bucket |
| 5-6 | Create Lambda function triggered by S3 | Lambda processing logs |
| 7 | Setup DynamoDB, Lambda writes summaries | Summaries in DynamoDB |

### Phase 4: Polish + Deploy (Week 4)
| Day | Task | Deliverable |
|-----|------|-------------|
| 1-2 | Implement cost estimation API endpoints | `/costs/estimate` working |
| 3-4 | Setup GitHub Actions CI/CD | Auto-build on push |
| 5-6 | Deploy FastAPI to Railway/Render | Public URL |
| 7 | Write README, record demo GIF | Project complete |

---

## 9. Deliverables Checklist

### 9.1 Code Deliverables
- [ ] `docker-compose.yml` — One-command local setup
- [ ] `log_generator/` — Realistic log simulator
- [ ] `kafka_consumer/` — Stream processor + ML
- [ ] `api/` — FastAPI application
- [ ] `aws/lambda/` — Lambda function code
- [ ] `grafana/` — Dashboard JSON
- [ ] `.github/workflows/` — CI/CD pipeline
- [ ] `tests/` — Unit tests (at least for ML + API)

### 9.2 Documentation Deliverables
- [ ] `README.md` — Project overview + quick start
- [ ] `docs/ARCHITECTURE.md` — Technical design
- [ ] `docs/API.md` — API documentation
- [ ] `docs/DEPLOYMENT.md` — How to deploy
- [ ] Demo GIF/video (30-60 seconds)

### 9.3 AWS Resources
- [ ] S3 bucket: `smart-log-analyzer-logs`
- [ ] Lambda function: `process-daily-logs`
- [ ] DynamoDB table: `anomaly_summaries`
- [ ] IAM user with minimal permissions

---

## 10. Success Criteria

### 10.1 Technical Success
| Criteria | Target |
|----------|--------|
| Local setup time | < 2 minutes (`docker-compose up`) |
| Log ingestion rate | > 100 logs/second |
| Anomaly detection latency | < 1 second |
| API response time | < 200ms |
| S3 export | Daily, automatic |
| Cost estimation accuracy | ±10% of actual |

### 10.2 Portfolio Success
| Criteria | Target |
|----------|--------|
| README quality | Professional, with GIF demo |
| Code quality | Linted, typed, documented |
| GitHub stars | Not important, but clean repo |
| Interview talking points | 5+ clear technical decisions to discuss |

### 10.3 Interview Talking Points
1. **"Why Kafka over direct DB writes?"** — Decoupling, buffering, replay capability
2. **"Why Isolation Forest?"** — Unsupervised, fast, explainable
3. **"Why S3 + Lambda + DynamoDB?"** — Serverless, cost-effective, scalable
4. **"How accurate is cost prediction?"** — Based on AWS pricing API, ±10%
5. **"How would you scale this?"** — Kafka partitions, Lambda concurrency, DynamoDB on-demand

---

## 11. Risk Mitigation

| Risk | Mitigation |
|------|------------|
| AWS costs exceed budget | Use free tier only, set billing alerts at $1 |
| Kafka too complex locally | Use single-node setup, Redpanda as alternative |
| ML model not accurate | Start with simple threshold rules, add ML later |
| Time overrun | Prioritize MVP features, defer nice-to-haves |
| Can't deploy API | Use Streamlit as fallback (you know it) |

---

## 12. Post-Project Extensions (Future)

Once MVP is complete, consider adding:

1. **Kubernetes deployment** — Deploy on local K8s (minikube) or EKS
2. **Terraform IaC** — Codify AWS resources
3. **Multi-cloud** — Add GCP Cloud Functions alternative
4. **Real-time alerts** — Slack/Discord webhook on anomaly
5. **Log search** — Elasticsearch integration
6. **ML improvements** — Add LSTM for time-series forecasting

---

## 13. References

- [Apache Kafka Documentation](https://kafka.apache.org/documentation/)
- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [AWS Free Tier](https://aws.amazon.com/free/)
- [Isolation Forest Paper](https://cs.nju.edu.cn/zhouzh/zhouzh.files/publication/icdm08b.pdf)
- [AWS Pricing Calculator](https://calculator.aws/)

---

**Document Status:** Ready for Implementation  
**Next Step:** Create project skeleton with `docker-compose.yml`
