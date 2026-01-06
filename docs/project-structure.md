# Project Structure

```
smart-log-analyzer/
├── pyproject.toml
├── uv.lock
├── .gitignore
├── .env.example
├── README.md
├── docker-compose.yml
│
├── src/
│   └── smartlog/
│       ├── __init__.py
│       ├── config.py
│       ├── log_generator/
│       │   ├── __init__.py
│       │   └── generator.py
│       ├── consumer/
│       │   ├── __init__.py
│       │   ├── consumer.py
│       │   └── ml_detector.py
│       ├── api/
│       │   ├── __init__.py
│       │   ├── main.py
│       │   ├── routers/
│       │   │   ├── __init__.py
│       │   │   ├── anomalies.py
│       │   │   ├── metrics.py
│       │   │   └── costs.py
│       │   └── models/
│       │       ├── __init__.py
│       │       └── schemas.py
│       ├── ml/
│       │   ├── __init__.py
│       │   ├── train.py
│       │   └── features.py
│       └── aws/
│           ├── __init__.py
│           ├── s3_exporter.py
│           └── lambda_handler.py
│
├── db/
│   └── init_db.sql
│
├── docker/
│   ├── api/Dockerfile
│   ├── consumer/Dockerfile
│   └── generator/Dockerfile
│
├── prometheus/
│   └── prometheus.yml
│
├── grafana/
│   ├── dashboard.json
│   └── provisioning/
│
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
│
├── tests/
│   ├── test_api.py
│   ├── test_consumer.py
│   └── test_ml.py
│
├── docs/
│   ├── project-structure.md
│   ├── architecture.md
│   ├── api.md
│   └── Technical requirements.md
│
└── .github/
    └── workflows/
        └── ci.yml
```
