# MLOps — Complete Notes for AI/ML Engineers
### Interview-Ready | Placement-Focused | End-to-End ML Lifecycle

> **How to use this:** Every section is written so the concept explanation *is* the answer to the interview question at the top. Covers theory + intuition + tools + code patterns. Flows from fundamentals → pipeline → deployment → monitoring → tools → interview Q&A.

---

## Table of Contents

**Part 1 — MLOps Foundations**
1. [What is MLOps? MLOps vs DevOps](#1-what-is-mlops-mlops-vs-devops)
2. [ML Lifecycle & MLOps Maturity Levels](#2-ml-lifecycle--mlops-maturity-levels)
3. [Reproducibility — The Core Problem](#3-reproducibility--the-core-problem)

**Part 2 — Data & Feature Management**
4. [Data Versioning — DVC](#4-data-versioning--dvc)
5. [Feature Stores — Feast, Hopsworks](#5-feature-stores--feast-hopsworks)
6. [Data Validation & Quality](#6-data-validation--quality)

**Part 3 — Experiment Tracking & Model Registry**
7. [Experiment Tracking — MLflow, W&B](#7-experiment-tracking--mlflow-wb)
8. [Model Registry & Versioning](#8-model-registry--versioning)

**Part 4 — Training Pipelines & Orchestration**
9. [ML Pipelines — Design & Components](#9-ml-pipelines--design--components)
10. [Orchestration — Airflow, Kubeflow, Prefect](#10-orchestration--airflow-kubeflow-prefect)
11. [Containerisation — Docker for ML](#11-containerisation--docker-for-ml)
12. [Kubernetes for ML Workloads](#12-kubernetes-for-ml-workloads)

**Part 5 — CI/CD for Machine Learning**
13. [CI/CD for ML — Concepts & Pipeline Design](#13-cicd-for-ml--concepts--pipeline-design)
14. [Testing ML Systems](#14-testing-ml-systems)
15. [Continuous Training (CT)](#15-continuous-training-ct)

**Part 6 — Model Deployment & Serving**
16. [Deployment Strategies](#16-deployment-strategies)
17. [Model Serving — REST, gRPC, Batch, Streaming](#17-model-serving--rest-grpc-batch-streaming)
18. [Serving Frameworks — TorchServe, TFServing, Triton, BentoML](#18-serving-frameworks--torchserve-tfserving-triton-bentoml)
19. [Model Optimisation for Serving](#19-model-optimisation-for-serving)

**Part 7 — Monitoring & Observability**
20. [Model Monitoring — What, Why, How](#20-model-monitoring--what-why-how)
21. [Data Drift & Concept Drift](#21-data-drift--concept-drift)
22. [Alerting, Logging & Observability Stack](#22-alerting-logging--observability-stack)
23. [Retraining Strategies](#23-retraining-strategies)

**Part 8 — Cloud & Platform MLOps**
24. [Cloud ML Platforms — SageMaker, Vertex AI, AzureML](#24-cloud-ml-platforms--sagemaker-vertex-ai-azureml)
25. [Infrastructure as Code for ML](#25-infrastructure-as-code-for-ml)
26. [Cost Optimisation in MLOps](#26-cost-optimisation-in-mlops)

**Part 9 — Governance, Ethics & Security**
27. [Model Governance & Compliance](#27-model-governance--compliance)
28. [Responsible AI & Bias Detection](#28-responsible-ai--bias-detection)

**Part 10 — Interview Q&A**
29. [Interview Q&A — Fundamentals & Concepts](#29-interview-qa--fundamentals--concepts)
30. [Interview Q&A — Deployment & Serving](#30-interview-qa--deployment--serving)
31. [Interview Q&A — Monitoring & Drift](#31-interview-qa--monitoring--drift)
32. [Interview Q&A — Tools & Systems Design](#32-interview-qa--tools--systems-design)
33. [Quick Reference Cheat Sheet](#33-quick-reference-cheat-sheet)

---

## 1. What is MLOps? MLOps vs DevOps

> **Interview Q:** *What is MLOps and how does it differ from DevOps?*

**MLOps (Machine Learning Operations)** is the set of practices, processes, and tools that combine ML, DevOps, and Data Engineering to automate and streamline the end-to-end ML lifecycle — from data preparation and model training to deployment, monitoring, and retraining — reliably at scale.

### Why MLOps Exists

Without MLOps:
- Data scientists train models locally → hand off a pickle file → "works on my machine" → weeks of engineering effort to deploy
- Models degrade silently in production (data drift) with no alerts
- Experiments are lost in Jupyter notebooks — no one can reproduce results
- Retraining is manual, slow, error-prone
- No audit trail — impossible to explain why a model made a decision

With MLOps:
- Automated pipeline from data → training → evaluation → deployment
- Continuous monitoring with automatic drift detection and alerting
- Full reproducibility — any experiment can be recreated from Git hash + data version
- One-click (or zero-click) retraining and rollback

### MLOps vs DevOps

| Dimension | DevOps | MLOps |
|---|---|---|
| **Primary artefact** | Code / application | Code + Data + Model |
| **Testing** | Unit, integration, e2e tests | All DevOps tests + data validation + model quality gates |
| **What can "break"** | Code bugs | Code bugs + data drift + concept drift + model staleness |
| **Versioning** | Code (Git) | Code + Data + Models + Experiments |
| **Deployment** | Static binary | Dynamic model + serving infrastructure |
| **Monitoring** | CPU, memory, latency | All DevOps metrics + prediction distribution + model accuracy |
| **CI trigger** | Code commit | Code commit + new data arrival + performance degradation |
| **Key tools** | Jenkins, Ansible, Nagios | MLflow, DVC, Kubeflow, Evidently, Seldon |

### The Three Pillars of MLOps

```
        Data Engineering
             ↕
    ML ←→ MLOps ←→ DevOps
        System operability
```

1. **ML:** Model development, training, evaluation
2. **DevOps:** CI/CD, containerisation, infrastructure automation, monitoring
3. **Data Engineering:** Pipelines, feature engineering, data quality, versioning

---

## 2. ML Lifecycle & MLOps Maturity Levels

> **Interview Q:** *What are the stages of the ML lifecycle? What are MLOps maturity levels?*

### End-to-End ML Lifecycle

```
┌─────────────────────────────────────────────────────────────────┐
│  1. PROBLEM DEFINITION                                           │
│     Define ML objective, success metrics, business KPIs         │
├─────────────────────────────────────────────────────────────────┤
│  2. DATA MANAGEMENT                                             │
│     Collection → Versioning → Validation → Feature Engineering  │
├─────────────────────────────────────────────────────────────────┤
│  3. EXPERIMENTATION                                             │
│     Model selection → Hyperparameter tuning → Tracking          │
├─────────────────────────────────────────────────────────────────┤
│  4. MODEL EVALUATION                                            │
│     Offline metrics → Bias/fairness → Business KPI alignment    │
├─────────────────────────────────────────────────────────────────┤
│  5. MODEL PACKAGING                                             │
│     Serialisation → Containerisation → Registry                 │
├─────────────────────────────────────────────────────────────────┤
│  6. DEPLOYMENT                                                  │
│     Blue-green / Canary / Shadow → A/B testing                  │
├─────────────────────────────────────────────────────────────────┤
│  7. MONITORING                                                  │
│     Data drift → Concept drift → Performance → Infrastructure   │
├─────────────────────────────────────────────────────────────────┤
│  8. RETRAINING                                                  │
│     Trigger → New data → Retrain → Validate → Re-deploy         │
└─────────────────────────────────────────────────────────────────┘
                          ↑ feedback loop ↑
```

### MLOps Maturity Levels (Google's Framework)

**Level 0 — Manual Process:**
- Data scientists manually train models in notebooks
- Model deployed as a one-time effort (pickle file + Flask script)
- No pipeline automation, no monitoring, no CI/CD
- Common in early-stage companies and academic settings

**Level 1 — ML Pipeline Automation:**
- Automated training pipeline (triggered on schedule or data arrival)
- Feature store for reusable features
- Experiment tracking (MLflow)
- Model registry
- Basic model monitoring
- Still: manual deployment trigger

**Level 2 — CI/CD Pipeline Automation:**
- Automated testing of data, model code, and model performance
- CI/CD pipeline — commit triggers test → train → evaluate → deploy
- Automated retraining when drift detected
- Full A/B deployment infrastructure
- Comprehensive monitoring and alerting
- Model governance and audit trails

> Most companies target Level 1. FAANG-level companies operate at Level 2.

---

## 3. Reproducibility — The Core Problem

> **Interview Q:** *What is reproducibility in ML and how do you ensure it?*

Reproducibility means being able to recreate a model's exact training conditions (data, code, environment, randomness) and obtain the same (or statistically equivalent) results.

### Why It's Hard

A model depends on ALL of:
- Code version (model architecture, preprocessing logic)
- Data version (which records, which transformations)
- Environment (library versions, CUDA version, OS)
- Hyperparameters (learning rate, batch size, regularisation)
- Random seeds (weight initialisation, data shuffling, dropout)

Change any one → potentially different model.

### The Reproducibility Stack

```
Random Seeds:     torch.manual_seed(42), numpy.random.seed(42), random.seed(42)
                  CUDA: torch.cuda.manual_seed_all(42)
                  + torch.backends.cudnn.deterministic = True

Environment:      requirements.txt / pyproject.toml (pin exact versions)
                  Docker image (freeze the entire environment)
                  conda environment.yml

Code:             Git commit hash → tag every experiment with the hash

Data:             DVC (data version control) → link data version to Git commit

Experiments:      MLflow / W&B → log all params, metrics, artifacts, code hash

Model:            Save model + architecture + preprocessing pipeline together
```

**Golden rule:** Any experiment must be fully described by `(git_hash, data_version, hyperparameters, random_seeds)`. Given these four, anyone should be able to reproduce the results.

---

## 4. Data Versioning — DVC

> **Interview Q:** *What is DVC and why do you need it alongside Git?*

**Problem:** Git is designed for code (text files). ML datasets are large binary files (GBs to TBs) — you can't store them in Git (too large, binary diffs are useless).

**DVC (Data Version Control)** works alongside Git:
- Git tracks code + DVC metadata files (`.dvc` files, tiny text)
- DVC tracks the actual large files in cloud storage (S3, GCS, Azure Blob)
- A `.dvc` file is a pointer to the actual data location + hash

```
Git repository:
  ├── train.py           (tracked by Git)
  ├── data/
  │   └── train.csv.dvc  (tiny metadata file tracked by Git)
  └── models/
      └── model.pkl.dvc  (metadata pointer)

DVC remote (S3):
  ├── data/train.csv     (actual large file)
  └── models/model.pkl   (actual model file)
```

### DVC Core Commands

```bash
# Initialise DVC in a Git repo
dvc init
git add .dvc .dvcignore && git commit -m "init DVC"

# Add data to DVC tracking
dvc add data/train.csv       # creates data/train.csv.dvc
git add data/train.csv.dvc
git commit -m "add training data v1"

# Set up remote storage (S3)
dvc remote add -d myremote s3://my-bucket/dvc-store
dvc push   # upload data to S3

# Pull data on a new machine
git clone <repo>
dvc pull   # downloads data from S3

# Switch to a previous data version
git checkout HEAD~1 -- data/train.csv.dvc
dvc checkout   # restores the data file to the version at HEAD~1

# Run a tracked pipeline step
dvc run -n preprocess \
  -d src/preprocess.py -d data/raw.csv \
  -o data/processed.csv \
  python src/preprocess.py

dvc repro   # re-run only changed stages (like make for ML)
dvc dag     # visualise pipeline DAG
```

### DVC Pipelines (`dvc.yaml`)

```yaml
stages:
  preprocess:
    cmd: python src/preprocess.py
    deps:
      - src/preprocess.py
      - data/raw.csv
    outs:
      - data/processed.csv

  train:
    cmd: python src/train.py --lr 0.001
    deps:
      - src/train.py
      - data/processed.csv
    params:
      - params.yaml:
        - train.lr
        - train.epochs
    outs:
      - models/model.pkl
    metrics:
      - metrics/eval.json

  evaluate:
    cmd: python src/evaluate.py
    deps:
      - src/evaluate.py
      - models/model.pkl
      - data/test.csv
    metrics:
      - metrics/eval.json
```

```bash
dvc repro        # runs only changed stages
dvc metrics show # compare metrics across runs
dvc params diff  # see what params changed
```

---

## 5. Feature Stores — Feast, Hopsworks

> **Interview Q:** *What is a Feature Store and why is it important in MLOps?*

A Feature Store is a centralised repository for storing, sharing, and serving ML features. It solves the problem of teams independently recomputing the same features for different models — inconsistently, with different logic, causing training-serving skew.

### Problems Feature Stores Solve

**Duplication:** Team A computes "user 30-day purchase count" differently from Team B → different models, inconsistent predictions.

**Training-Serving Skew:** Feature computation logic in training code differs from production code → model trained on one distribution, served on another → silent accuracy degradation.

**Point-in-Time Correctness:** Historical feature values must reflect what was known at the time of the label, not future information → feature store handles "time travel" queries.

**Sharing & Discovery:** Hard to find what features already exist. Feature store provides a catalogue.

### Feature Store Architecture

```
┌─────────────────────────────────────────────────────┐
│                  FEATURE STORE                       │
│                                                      │
│  ┌─────────────────┐     ┌────────────────────────┐ │
│  │  OFFLINE STORE  │     │    ONLINE STORE        │ │
│  │  (Historical)   │     │   (Low-latency)        │ │
│  │                 │     │                        │ │
│  │  S3 / BigQuery  │     │  Redis / DynamoDB      │ │
│  │  Parquet/Delta  │     │  <10ms lookup          │ │
│  │                 │     │                        │ │
│  │  Batch training │     │  Real-time serving     │ │
│  │  feature lookup │     │  feature lookup        │ │
│  └────────┬────────┘     └──────────┬─────────────┘ │
│           │                         │                │
│      Feature Transformation Pipeline                 │
│      (Batch jobs / Streaming / On-demand)            │
└─────────────────────────────────────────────────────┘
           ↑                          ↑
    Training pipeline         Inference pipeline
```

### Feast (Feature Store)

```python
# Define a feature view
from feast import FeatureView, Entity, Field, FileSource
from feast.types import Float32, Int64

user = Entity(name="user_id", join_keys=["user_id"])

user_features = FeatureView(
    name="user_purchase_features",
    entities=[user],
    ttl=timedelta(days=30),
    schema=[
        Field(name="total_spend_30d", dtype=Float32),
        Field(name="order_count_30d", dtype=Int64),
        Field(name="avg_order_value", dtype=Float32),
    ],
    source=FileSource(path="data/user_features.parquet", timestamp_field="event_timestamp"),
)

# Retrieve features for training (offline)
feature_store = FeatureStore("feature_repo/")
training_df = feature_store.get_historical_features(
    entity_df=entity_df,
    features=["user_purchase_features:total_spend_30d",
               "user_purchase_features:order_count_30d"]
).to_df()

# Retrieve features for serving (online)
feature_store.materialize_incremental(end_date=datetime.now())
online_features = feature_store.get_online_features(
    features=["user_purchase_features:total_spend_30d"],
    entity_rows=[{"user_id": 42}]
).to_dict()
```

### Offline vs Online Store

| | Offline Store | Online Store |
|---|---|---|
| **Purpose** | Training, batch prediction | Real-time inference |
| **Storage** | S3, BigQuery, Snowflake, Delta Lake | Redis, DynamoDB, Cassandra |
| **Latency** | Minutes to hours | < 10ms |
| **Data** | Full history | Latest value per entity |
| **Operations** | Point-in-time correct joins | Key-value lookup |

---

## 6. Data Validation & Quality

> **Interview Q:** *How do you ensure data quality in an ML pipeline?*

Data quality issues are the #1 cause of silent model degradation in production. Validate data at every pipeline stage.

### Great Expectations

Industry-standard Python library for data validation. Define "expectations" (assertions about data) and run them as checkpoints:

```python
import great_expectations as ge

# Load data as GE dataframe
df = ge.read_csv("data/train.csv")

# Define expectations
df.expect_column_to_exist("user_id")
df.expect_column_values_to_not_be_null("user_id")
df.expect_column_values_to_be_between("age", min_value=0, max_value=120)
df.expect_column_values_to_be_in_set("country", ["IN", "US", "UK", "DE"])
df.expect_column_mean_to_be_between("purchase_amount", min_value=10, max_value=500)
df.expect_table_row_count_to_be_between(min_value=10000, max_value=1000000)

# Run validation
result = df.validate()
print(result["success"])  # True/False — fail pipeline if False
```

### Key Data Validation Checks

```
Schema validation:    Column names, types, counts match expected schema
Null checks:          % nulls per column within acceptable bounds
Range checks:         Values within expected min/max
Cardinality checks:   Unique values count for categorical columns
Distribution checks:  Mean, std, quantiles match historical baseline
Referential integrity: Foreign keys exist in reference tables
Freshness:            Data arrives on time (timestamp within expected range)
Volume checks:        Row count not anomalously high or low
```

### TFX Data Validation (TensorFlow Extended)

```python
# TFX generates schema from data and validates future batches against it
import tensorflow_data_validation as tfdv

# Generate schema from training data
train_stats = tfdv.generate_statistics_from_csv("train.csv")
schema = tfdv.infer_schema(train_stats)

# Validate new data against schema
new_stats = tfdv.generate_statistics_from_csv("new_data.csv")
anomalies = tfdv.validate_statistics(new_stats, schema)
tfdv.display_anomalies(anomalies)
# Catches: new values in categorical columns, type mismatches, missing columns
```

---

## 7. Experiment Tracking — MLflow, W&B

> **Interview Q:** *What is experiment tracking and what does MLflow track?*

Experiment tracking is the systematic logging of every model training run's parameters, metrics, code, and artefacts so you can compare runs, reproduce results, and promote the best model.

### MLflow

Four core components:

**1. Tracking** — Log parameters, metrics, and artefacts:
```python
import mlflow

mlflow.set_experiment("churn_prediction")

with mlflow.start_run(run_name="lgbm_baseline"):
    # Log hyperparameters
    mlflow.log_param("n_estimators", 500)
    mlflow.log_param("learning_rate", 0.05)
    mlflow.log_param("max_depth", 6)

    # Train model
    model = LGBMClassifier(n_estimators=500, learning_rate=0.05)
    model.fit(X_train, y_train)

    # Log metrics
    mlflow.log_metric("accuracy", accuracy_score(y_test, preds))
    mlflow.log_metric("f1", f1_score(y_test, preds))
    mlflow.log_metric("auc_roc", roc_auc_score(y_test, probs))

    # Log artefacts (plots, files)
    mlflow.log_artifact("confusion_matrix.png")
    mlflow.log_artifact("feature_importance.csv")

    # Log model
    mlflow.sklearn.log_model(model, "model")

    # Auto-log (logs everything automatically for supported frameworks)
    mlflow.sklearn.autolog()
```

**2. Projects** — Package ML code for reproducibility (conda env + entry points).

**3. Models** — Standard format for packaging models with inference signature.

**4. Registry** — Manage model versions and lifecycle stages.

```bash
# Launch MLflow UI
mlflow ui --port 5000   # go to http://localhost:5000
```

### MLflow Model Registry

```python
# Register a model from a run
result = mlflow.register_model(
    "runs:/abc123/model",
    "ChurnPredictor"
)

# Transition model stage
client = mlflow.tracking.MlflowClient()
client.transition_model_version_stage(
    name="ChurnPredictor",
    version=3,
    stage="Production"   # Staging → Production → Archived
)

# Load the production model for serving
model = mlflow.pyfunc.load_model("models:/ChurnPredictor/Production")
predictions = model.predict(X_new)
```

### Weights & Biases (W&B)

More feature-rich than MLflow — better visualisations, collaboration, and hyperparameter sweeps:

```python
import wandb

wandb.init(project="churn_prediction", name="lgbm_v2", config={
    "n_estimators": 500,
    "learning_rate": 0.05
})

# During training
for epoch in range(100):
    wandb.log({"train_loss": loss, "val_loss": val_loss, "epoch": epoch})

# Log model
wandb.save("model.pkl")

# Hyperparameter sweep
sweep_config = {
    "method": "bayes",
    "metric": {"name": "val_auc", "goal": "maximize"},
    "parameters": {
        "lr": {"distribution": "log_uniform", "min": 1e-5, "max": 1e-2},
        "n_estimators": {"values": [100, 300, 500, 1000]},
    }
}
sweep_id = wandb.sweep(sweep_config, project="churn_prediction")
wandb.agent(sweep_id, function=train_fn, count=50)
```

### MLflow vs W&B

| Feature | MLflow | W&B |
|---|---|---|
| Open source | ✓ Full OSS | Partial (SDK OSS, backend proprietary) |
| Self-hosted | ✓ Easy | ✓ Enterprise only |
| UI quality | Good | Excellent |
| Sweeps/tuning | Basic | Advanced (Bayesian, hyperband) |
| Collaboration | Basic | Strong (team workspaces) |
| Integrations | Wide | Very wide |
| Cost | Free | Free tier; paid for large teams |
| Best for | Production MLOps stack | Research teams, exploration |

---

## 8. Model Registry & Versioning

> **Interview Q:** *What is a model registry and what is its role in MLOps?*

A Model Registry is a centralised store that manages the lifecycle of ML models — from experimental to staging to production, with version history, metadata, and approval workflows.

### What a Model Registry Stores

```
For each model version:
├── Model artefact (weights, parameters)
├── Code reference (Git commit hash)
├── Training data reference (DVC hash / dataset version)
├── Hyperparameters
├── Evaluation metrics (on standard test set)
├── Training environment (Docker image, dependencies)
├── Model schema (input/output signature)
├── Stage: [None → Staging → Production → Archived]
└── Approval status (for regulated industries)
```

### Lifecycle Stages

```
Experiment runs → Register best run → None (registered, not deployed)
                                     ↓
                               Staging (QA testing)
                                     ↓
                              Production (live traffic)
                                     ↓
                               Archived (replaced)
```

**Transition policy:** Only models that pass evaluation gates (accuracy threshold, latency SLA, fairness checks) can be promoted to Staging. Human sign-off required before Production in regulated industries (finance, healthcare).

### Model Serialisation Formats

| Format | Framework | Portable | Notes |
|---|---|---|---|
| `.pkl` (pickle) | Scikit-learn | Python only | Fast, unsafe (arbitrary code on load) |
| `.joblib` | Scikit-learn | Python only | Better for large arrays |
| `SavedModel` | TensorFlow | Yes | Full TF graph |
| `.pt` / `.pth` | PyTorch | Python | State dict (preferred) |
| TorchScript | PyTorch | Yes | C++ runtime compatible |
| ONNX | Framework-agnostic | Yes | Cross-framework; production standard |
| GGUF | LLM inference | Yes | llama.cpp format |
| MLflow `pyfunc` | Any | Python | Standardised wrapper |

**ONNX (Open Neural Network Exchange):** Convert PyTorch/TensorFlow model to a standard graph format, then optimise with ONNX Runtime for fast inference across platforms.

```python
# Export PyTorch model to ONNX
import torch.onnx
dummy_input = torch.randn(1, 3, 224, 224)
torch.onnx.export(model, dummy_input, "model.onnx",
                  input_names=["image"], output_names=["logits"],
                  dynamic_axes={"image": {0: "batch_size"}})

# Load and run with ONNX Runtime
import onnxruntime as ort
session = ort.InferenceSession("model.onnx")
outputs = session.run(None, {"image": input_array})
```

---

## 9. ML Pipelines — Design & Components

> **Interview Q:** *What is an ML pipeline? How is it different from a data pipeline?*

An ML pipeline is a sequence of automated, repeatable steps that transforms raw data into a deployed model artefact. Unlike a data pipeline (ETL), an ML pipeline includes model training, evaluation, and serving steps.

### Standard ML Pipeline Components

```
┌────────────────────────────────────────────────────────────────────┐
│                         ML PIPELINE                                 │
│                                                                     │
│  [Data Ingestion]                                                   │
│    Pull from source (DB, S3, API) → validate → store versioned     │
│         ↓                                                           │
│  [Data Validation]                                                  │
│    Schema check → distribution check → fail fast if bad data       │
│         ↓                                                           │
│  [Feature Engineering]                                              │
│    Transforms, encodings, scaling → push to Feature Store          │
│         ↓                                                           │
│  [Data Split]                                                       │
│    Train / Val / Test split → stratified → no leakage              │
│         ↓                                                           │
│  [Model Training]                                                   │
│    Train with logged hyperparameters → track with MLflow           │
│         ↓                                                           │
│  [Model Evaluation]                                                 │
│    Offline metrics → fairness checks → performance gates           │
│         ↓  (if passes gates)                                        │
│  [Model Registration]                                               │
│    Push to model registry with metadata                            │
│         ↓                                                           │
│  [Model Deployment]                                                 │
│    Shadow deploy → canary → full rollout                           │
└────────────────────────────────────────────────────────────────────┘
```

### Pipeline Design Principles

**Idempotency:** Running a pipeline stage multiple times with the same input produces the same output. Essential for safe retries.

**Modularity:** Each stage is independently testable and replaceable. Don't write one giant script.

**Fail-fast:** Validate data early. Don't train for 10 hours only to fail during evaluation because the input schema was wrong.

**Artefact lineage:** Every output artefact is traceable to its inputs (data version + code version).

**Parameterisation:** Hyperparameters, file paths, and thresholds in config files (YAML/JSON), not hardcoded.

```yaml
# params.yaml — single source of truth
data:
  train_path: "s3://bucket/data/train_v3.parquet"
  test_size: 0.2
  random_seed: 42

model:
  algorithm: "lightgbm"
  n_estimators: 500
  learning_rate: 0.05
  max_depth: 6

evaluation:
  min_accuracy: 0.85
  min_auc_roc: 0.90
```

### Training-Serving Skew (Critical Problem)

> **Interview Q:** *What is training-serving skew and how do you prevent it?*

Training-serving skew is when the feature computation logic during training differs from production serving logic — the model is evaluated on different data than it was trained on.

**Common causes:**
- Preprocessing in Python/pandas during training; different logic in Java/C++ at serving
- Using future data in training features (data leakage)
- Different handling of nulls, outliers, or encoding in training vs. serving code
- Feature store materialisation lag — stale features at serving time

**Prevention:**
- Share feature computation code between training and serving (single Python function)
- Use a Feature Store — same feature logic computed once, used everywhere
- Log prediction features at serving time → compare distribution to training
- Validate: apply training preprocessing to serving data → check distribution match

---

## 10. Orchestration — Airflow, Kubeflow, Prefect

> **Interview Q:** *What is pipeline orchestration and which tools are used?*

Orchestration tools schedule, execute, and monitor multi-step pipelines. They handle dependencies (don't run step B until step A succeeds), retries, parallelism, and alerting.

### Apache Airflow

DAG-based workflow orchestrator. Each pipeline is a Directed Acyclic Graph of tasks:

```python
from airflow import DAG
from airflow.operators.python import PythonOperator
from airflow.operators.bash import BashOperator
from datetime import datetime, timedelta

default_args = {
    "owner": "mlops-team",
    "retries": 2,
    "retry_delay": timedelta(minutes=5),
    "email_on_failure": True,
    "email": ["alerts@company.com"]
}

with DAG(
    dag_id="churn_model_retraining",
    default_args=default_args,
    schedule_interval="0 2 * * *",   # daily at 2 AM
    start_date=datetime(2025, 1, 1),
    catchup=False,
    tags=["ml", "churn"]
) as dag:

    ingest = PythonOperator(
        task_id="ingest_data",
        python_callable=ingest_new_data
    )

    validate = PythonOperator(
        task_id="validate_data",
        python_callable=run_data_validation
    )

    train = PythonOperator(
        task_id="train_model",
        python_callable=train_churn_model
    )

    evaluate = PythonOperator(
        task_id="evaluate_model",
        python_callable=evaluate_and_register
    )

    deploy = BashOperator(
        task_id="deploy_model",
        bash_command="kubectl rollout restart deployment/churn-model"
    )

    # Define dependencies (task order)
    ingest >> validate >> train >> evaluate >> deploy
```

### Kubeflow Pipelines

Kubernetes-native ML pipeline orchestrator. More ML-focused than Airflow:

```python
from kfp import dsl

@dsl.component
def preprocess(data_path: str, output_path: str):
    import pandas as pd
    df = pd.read_parquet(data_path)
    # ... preprocessing
    df.to_parquet(output_path)

@dsl.component
def train(data_path: str, model_path: str, lr: float = 0.01):
    import lightgbm as lgb
    # ... train model
    model.save_model(model_path)

@dsl.pipeline(name="churn-training-pipeline")
def churn_pipeline(data_path: str, lr: float = 0.01):
    preprocess_task = preprocess(data_path=data_path, output_path="/tmp/processed")
    train_task = train(
        data_path=preprocess_task.output,
        model_path="/tmp/model",
        lr=lr
    )

# Compile and submit
from kfp import compiler
compiler.Compiler().compile(churn_pipeline, "pipeline.yaml")
```

### Airflow vs Kubeflow vs Prefect

| | Airflow | Kubeflow | Prefect |
|---|---|---|---|
| **Focus** | General workflows | ML on Kubernetes | Modern Python workflows |
| **Interface** | Python DAG | Python components | Python flow/task |
| **Infrastructure** | Any (not K8s-native) | Kubernetes-native | Any |
| **ML-specific** | No | Yes | No |
| **UI** | Good | Good | Excellent |
| **Learning curve** | Moderate | High | Low |
| **Best for** | Data pipelines, ETL | End-to-end ML on K8s | Python-first teams |

---

## 11. Containerisation — Docker for ML

> **Interview Q:** *Why is Docker important for MLOps?*

Docker packages your model, code, and dependencies into a container image — a portable, reproducible unit that runs identically everywhere (local, CI, staging, production).

**Without Docker:** "Works on my machine" — different Python versions, different library versions, different OS → inconsistent results.
**With Docker:** Build once, run anywhere.

### Dockerfile for ML Model Serving

```dockerfile
# Use official Python slim image as base
FROM python:3.11-slim

# Set working directory
WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Copy and install Python dependencies first (Docker layer caching)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy model artefact
COPY models/model.pkl models/

# Copy application code
COPY src/ src/

# Expose inference port
EXPOSE 8000

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:8000/health || exit 1

# Run the inference server
CMD ["uvicorn", "src.serve:app", "--host", "0.0.0.0", "--port", "8000"]
```

### Multi-Stage Dockerfile (Training + Serving)

```dockerfile
# Stage 1: Training environment (heavier)
FROM nvidia/cuda:12.1.0-cudnn8-runtime-ubuntu22.04 AS trainer
WORKDIR /train
COPY requirements-train.txt .
RUN pip install -r requirements-train.txt
COPY src/train.py .
RUN python train.py  # trains and saves model.onnx

# Stage 2: Serving environment (lighter)
FROM python:3.11-slim AS server
WORKDIR /serve
COPY requirements-serve.txt .
RUN pip install -r requirements-serve.txt
COPY --from=trainer /train/model.onnx /serve/models/   # copy only the model
COPY src/serve.py .
EXPOSE 8000
CMD ["uvicorn", "serve:app", "--host", "0.0.0.0", "--port", "8000"]
```

**Multi-stage builds:** Training image needs CUDA, PyTorch, etc. (~8 GB). Serving image only needs ONNX Runtime (~500 MB). Copy only the model artefact into the lean serving image.

### Docker Best Practices for ML

```
✓ Pin exact versions in requirements.txt (torch==2.3.0, not torch>=2)
✓ Use .dockerignore (exclude data/, __pycache__, .git, notebooks/)
✓ Order Dockerfile: rarely-changing → frequently-changing (maximise cache)
✓ Non-root user for security (USER appuser)
✓ Multi-stage builds to minimise image size
✓ Health check endpoint in every serving container
✓ Environment variables for config (never hardcode paths or credentials)
```

---

## 12. Kubernetes for ML Workloads

> **Interview Q:** *How is Kubernetes used in MLOps? What is a Deployment vs a Job?*

Kubernetes (K8s) orchestrates containerised workloads — scheduling, scaling, healing, and routing. In MLOps, it manages both training jobs and inference deployments.

### Key K8s Concepts for ML

**Pod:** Smallest deployable unit — one or more containers sharing network/storage. A running model server is a Pod.

**Deployment:** Manages a set of identical Pods. Handles rolling updates, rollbacks, desired replica count. Used for inference servers (long-running).

**Job / CronJob:** Runs a Pod to completion, then stops. Used for training jobs, batch inference, data preprocessing.

**Service:** Stable network endpoint for a Deployment. Clients connect to the Service, K8s routes to healthy Pods.

**Ingress:** HTTP routing rules — route `/predict` to model service, `/health` to health check endpoint.

**ConfigMap / Secret:** Inject configuration and credentials into Pods without hardcoding.

**HPA (Horizontal Pod Autoscaler):** Automatically scale Pods based on CPU, memory, or custom metrics (e.g., requests per second).

### Inference Deployment YAML

```yaml
# model-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: churn-model
  labels:
    app: churn-model
    version: "v3"
spec:
  replicas: 3                      # 3 inference server instances
  selector:
    matchLabels:
      app: churn-model
  template:
    metadata:
      labels:
        app: churn-model
    spec:
      containers:
      - name: inference-server
        image: myregistry/churn-model:v3
        ports:
        - containerPort: 8000
        resources:
          requests:
            cpu: "500m"            # 0.5 CPU cores requested
            memory: "512Mi"
          limits:
            cpu: "2000m"           # max 2 CPU cores
            memory: "2Gi"
        env:
        - name: MODEL_PATH
          value: "/models/model.onnx"
        - name: LOG_LEVEL
          value: "INFO"
        readinessProbe:
          httpGet:
            path: /health
            port: 8000
          initialDelaySeconds: 10
          periodSeconds: 5
        livenessProbe:
          httpGet:
            path: /health
            port: 8000
          initialDelaySeconds: 30
          periodSeconds: 10
---
apiVersion: v1
kind: Service
metadata:
  name: churn-model-svc
spec:
  selector:
    app: churn-model
  ports:
  - port: 80
    targetPort: 8000
---
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: churn-model-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: churn-model
  minReplicas: 2
  maxReplicas: 20
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70    # scale up when average CPU > 70%
```

### Training Job YAML

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: churn-model-training-20250101
spec:
  template:
    spec:
      containers:
      - name: trainer
        image: myregistry/churn-trainer:latest
        args: ["python", "train.py", "--config", "params.yaml"]
        resources:
          limits:
            nvidia.com/gpu: 1     # request 1 GPU
      restartPolicy: Never
  backoffLimit: 2                 # retry up to 2 times on failure
```

---

## 13. CI/CD for ML — Concepts & Pipeline Design

> **Interview Q:** *What does CI/CD look like for an ML project? How is it different from software CI/CD?*

### CI/CD for ML — The Three Cs

**Continuous Integration (CI):**
- Every code commit triggers automated tests
- For ML: test data preprocessing code, model training code, serving code
- Run data validation on a sample
- Verify model trains without errors

**Continuous Delivery (CD):**
- Every passing CI run produces a deployable artefact
- For ML: a validated model version in the model registry, ready to deploy

**Continuous Training (CT) — ML-specific:**
- New data triggers automated retraining
- Retrained model validated against quality gates
- If passes → automatically promotes to production (or queues for human review)

### GitHub Actions ML Pipeline

```yaml
# .github/workflows/ml-pipeline.yml
name: ML CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  schedule:
    - cron: "0 2 * * *"    # daily retrain trigger
  workflow_dispatch:         # manual trigger

env:
  PYTHON_VERSION: "3.11"

jobs:
  test:
    name: Code Quality & Unit Tests
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: ${{ env.PYTHON_VERSION }}
      - name: Install dependencies
        run: pip install -r requirements.txt
      - name: Lint
        run: ruff check src/ tests/
      - name: Type check
        run: mypy src/
      - name: Unit tests
        run: pytest tests/unit/ -v --cov=src --cov-report=xml
      - name: Upload coverage
        uses: codecov/codecov-action@v3

  data-validation:
    name: Data Validation
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Pull latest data
        run: dvc pull data/
      - name: Run Great Expectations
        run: python src/validate_data.py
      - name: Upload validation report
        uses: actions/upload-artifact@v3
        with:
          name: data-validation-report
          path: reports/ge_report.html

  train-and-evaluate:
    name: Train & Evaluate
    needs: data-validation
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Pull data
        run: dvc pull
      - name: Train model
        run: |
          python src/train.py
          dvc push    # push model to DVC remote
      - name: Evaluate model
        id: evaluate
        run: |
          python src/evaluate.py --output metrics.json
          # Sets output: metrics.f1, metrics.auc
      - name: Quality gate
        run: |
          python -c "
          import json
          with open('metrics.json') as f:
              m = json.load(f)
          assert m['auc_roc'] >= 0.90, f'AUC {m[\"auc_roc\"]} below threshold 0.90'
          assert m['f1'] >= 0.85, f'F1 {m[\"f1\"]} below threshold 0.85'
          print('Quality gate PASSED')
          "
      - name: Register model in MLflow
        run: python src/register_model.py --stage Staging

  deploy-staging:
    name: Deploy to Staging
    needs: train-and-evaluate
    runs-on: ubuntu-latest
    environment: staging
    steps:
      - name: Build Docker image
        run: docker build -t myregistry/churn-model:${{ github.sha }} .
      - name: Push image
        run: docker push myregistry/churn-model:${{ github.sha }}
      - name: Deploy to staging
        run: |
          kubectl set image deployment/churn-model-staging \
            inference-server=myregistry/churn-model:${{ github.sha }}
          kubectl rollout status deployment/churn-model-staging

  deploy-production:
    name: Deploy to Production
    needs: deploy-staging
    runs-on: ubuntu-latest
    environment: production    # requires manual approval in GitHub
    steps:
      - name: Canary deploy (10% traffic)
        run: kubectl apply -f k8s/canary-deployment.yaml
      - name: Wait and validate canary
        run: python scripts/validate_canary.py --duration 30m --threshold 0.95
      - name: Full production rollout
        run: kubectl apply -f k8s/production-deployment.yaml
      - name: Promote model to Production in registry
        run: python src/promote_model.py --stage Production
```

---

## 14. Testing ML Systems

> **Interview Q:** *What types of tests should an ML system have?*

ML systems need all traditional software tests PLUS ML-specific tests:

### Test Pyramid for ML

```
                   ┌──────────────────┐
                   │   E2E Tests      │   Full pipeline: data→prediction (few, slow)
                  ┌┤  (Integration)   ├┐
                 ┌┤└──────────────────┘├┐
                 │   Model Quality     │   Metrics on test set, fairness, robustness
                ┌┤   Tests            ├┐
               ┌┤└────────────────────┘├┐
               │   Data Tests         │   Schema, distributions, freshness
              ┌┤                      ├┐
             ┌┤└──────────────────────┘├┐
             │      Unit Tests         │   Individual functions (fast, many)
             └─────────────────────────┘
```

### Unit Tests (ML-specific)

```python
# tests/unit/test_preprocessing.py
import pytest
import pandas as pd
from src.preprocessing import preprocess

def test_handles_null_age():
    df = pd.DataFrame({"age": [25, None, 30], "salary": [50000, 60000, 70000]})
    result = preprocess(df)
    assert result["age"].isnull().sum() == 0     # nulls imputed

def test_salary_scaling():
    df = pd.DataFrame({"age": [25], "salary": [100000]})
    result = preprocess(df)
    assert 0 <= result["salary_scaled"].iloc[0] <= 1  # within [0,1]

def test_output_schema():
    df = pd.DataFrame({"age": [25], "salary": [50000], "country": ["IN"]})
    result = preprocess(df)
    expected_cols = {"age_scaled", "salary_scaled", "country_IN", "country_US"}
    assert expected_cols.issubset(set(result.columns))

def test_deterministic():
    df = pd.DataFrame({"age": [25, 30], "salary": [50000, 60000]})
    result_1 = preprocess(df)
    result_2 = preprocess(df)
    pd.testing.assert_frame_equal(result_1, result_2)  # same input → same output
```

### Model Quality Tests

```python
# tests/model/test_model_quality.py
def test_minimum_accuracy():
    model = load_model("models/model.pkl")
    X_test, y_test = load_test_data()
    preds = model.predict(X_test)
    assert accuracy_score(y_test, preds) >= 0.85

def test_no_regression_from_baseline():
    new_model = load_model("models/candidate.pkl")
    baseline = load_model("models/production.pkl")
    X_test, y_test = load_test_data()
    new_auc = roc_auc_score(y_test, new_model.predict_proba(X_test)[:, 1])
    baseline_auc = roc_auc_score(y_test, baseline.predict_proba(X_test)[:, 1])
    assert new_auc >= baseline_auc - 0.01  # within 1% of baseline

def test_inference_latency():
    model = load_model("models/model.pkl")
    sample = load_single_sample()
    start = time.time()
    for _ in range(100):
        model.predict(sample)
    avg_ms = (time.time() - start) * 1000 / 100
    assert avg_ms < 50    # must predict in < 50ms per sample

def test_prediction_stability():
    model = load_model("models/model.pkl")
    sample = load_single_sample()
    preds = [model.predict(sample)[0] for _ in range(10)]
    assert len(set(preds)) == 1   # deterministic: same output every time
```

---

## 15. Continuous Training (CT)

> **Interview Q:** *What is continuous training? When should a model be retrained?*

Continuous Training (CT) is the automation of model retraining in response to triggers — new data, detected drift, or performance degradation.

### Retraining Triggers

```
Scheduled:          Retrain every week/month regardless (simplest)
Data volume:        Retrain when N new labelled samples accumulate
Performance drop:   Retrain when monitored metric falls below threshold
Drift detection:    Retrain when data drift score exceeds threshold
Manual:             Data scientist decides after reviewing dashboards
```

### CT Pipeline Design

```
Trigger (schedule / alert / event)
         ↓
Fetch new data from data warehouse
         ↓
Run data validation (fail fast if data quality is bad)
         ↓
Feature engineering + push to Feature Store
         ↓
Train model (track with MLflow)
         ↓
Evaluate against quality gates:
  - Accuracy ≥ threshold
  - Latency ≤ SLA
  - Fairness checks pass
  - No regression vs production model
         ↓
[FAIL] → Alert → human review
[PASS] → Promote to Staging → Canary deploy → Full deploy
```

### Champion-Challenger Pattern

```
Production (Champion): 90% traffic → existing model
Staging (Challenger):  10% traffic → newly trained model

Monitor metrics for both.
If Challenger consistently outperforms Champion after 72h:
    Promote Challenger → Champion
    Archive old Champion
Else:
    Discard Challenger
```

---

## 16. Deployment Strategies

> **Interview Q:** *What are the different model deployment strategies? When would you use each?*

### Blue-Green Deployment

Run two identical environments — Blue (current production) and Green (new version). Switch all traffic instantly from Blue to Green.

```
Traffic: Blue (100%) ─────────────────────→ Green (100%)
                       ↑ instant cutover
```

**Pros:** Zero downtime, instant rollback (switch back to Blue).
**Cons:** Requires 2× infrastructure cost; no gradual validation; bad rollout affects 100% of users.
**Use when:** Fast rollback is critical, infrastructure cost is acceptable.

### Canary Deployment

Gradually increase traffic to the new model:

```
Day 0:  New model 1%  traffic, Old model 99%
Day 1:  New model 5%  traffic, Old model 95%
Day 3:  New model 20% traffic, Old model 80%
Day 7:  New model 100% traffic — full rollout
         (or rollback to 0% if metrics degrade)
```

**Pros:** Limits blast radius — if new model is bad, only affects a small fraction of users. Real production traffic validation before full rollout.
**Cons:** More complex to implement; takes time; need robust monitoring.
**Use when:** Standard production ML deployment — safest default.

### Shadow Deployment (Dark Launch)

Run new model in parallel with production, but don't serve its predictions to users. Compare outputs offline.

```
User request → Production Model → User response (prediction served)
            ↘ Shadow Model    → Logs only (never shown to user)
```

**Pros:** Zero user impact. Can identify bugs, latency issues, distribution shifts.
**Cons:** Infrastructure cost; no real user feedback on shadow model quality.
**Use when:** Testing a new model that might have edge cases before any live exposure.

### A/B Testing

Split users into groups, serve different model versions, measure business metrics:

```
Group A (50%): Model v1 → measure conversion rate, CTR, etc.
Group B (50%): Model v2 → measure same metrics
Statistical test → determine winner after N days / N samples
```

**Key difference from canary:** A/B is designed for statistical comparison — held constant for the duration of the experiment. Canary is designed for safe rollout — traffic shifts increase over time.

### Rolling Update

Gradually replace old Pods with new ones in Kubernetes — default K8s strategy:

```
Step 1: Start 1 new Pod → wait until healthy
Step 2: Terminate 1 old Pod
Step 3: Repeat until all Pods are updated
```

Zero downtime; gradual; automatic health checks.

### Deployment Strategy Summary

| Strategy | Risk | Rollback | Cost | Use For |
|---|---|---|---|---|
| Blue-Green | Low (instant rollback) | Instant | 2× infra | High-stakes critical services |
| Canary | Very low | Fast | Normal | Standard ML deployment |
| Shadow | Zero user risk | N/A | 2× compute | Pre-production validation |
| A/B Test | Low | Fast | Normal | Business metric experiments |
| Rolling | Low | Moderate | Normal | Kubernetes default |

---

## 17. Model Serving — REST, gRPC, Batch, Streaming

> **Interview Q:** *What is the difference between online and batch inference?*

### Online (Real-Time) Inference

Predict in real-time for individual or small-batch requests. Latency is critical (< 200ms typically).

```python
# FastAPI serving endpoint
from fastapi import FastAPI
import numpy as np
import onnxruntime as ort

app = FastAPI()
session = ort.InferenceSession("model.onnx")

@app.get("/health")
def health(): return {"status": "ok"}

@app.post("/predict")
def predict(features: dict):
    input_array = np.array([[features[k] for k in feature_order]], dtype=np.float32)
    outputs = session.run(None, {"input": input_array})
    return {"prediction": int(outputs[0][0]),
            "probability": float(outputs[1][0][1])}
```

**Latency sources:**
- Network: 5–50ms
- Feature lookup from Feature Store (Redis): 1–5ms
- Preprocessing: 1–10ms
- Model inference: 1–100ms (depends on model size)
- Post-processing: 1ms

### Batch Inference

Process large datasets offline on a schedule. Throughput matters; latency is not critical.

```python
# Batch prediction script (run by Airflow/CronJob)
import pandas as pd
import joblib

model = joblib.load("model.pkl")
df = pd.read_parquet("s3://bucket/inference-data/2025-01-15.parquet")

# Predict in chunks to avoid OOM
chunk_size = 10000
predictions = []
for i in range(0, len(df), chunk_size):
    chunk = df.iloc[i:i+chunk_size]
    features = preprocess(chunk)
    preds = model.predict_proba(features)[:, 1]
    predictions.extend(preds.tolist())

df["churn_probability"] = predictions
df.to_parquet("s3://bucket/predictions/2025-01-15.parquet")
```

### Online vs Batch Inference

| | Online (Real-Time) | Batch |
|---|---|---|
| **Latency** | < 200ms | Hours acceptable |
| **Throughput** | Moderate | Very high |
| **Trigger** | Per request | Scheduled / event |
| **Infrastructure** | Always-on servers | Ephemeral compute |
| **Cost** | Higher (idle capacity) | Lower (pay per compute) |
| **Use cases** | Fraud detection, recommendations, chatbots | Risk scoring, campaign targeting, report generation |

### gRPC vs REST for Model Serving

| | REST (HTTP/JSON) | gRPC (HTTP/2 + Protobuf) |
|---|---|---|
| **Payload** | JSON (text, verbose) | Protobuf (binary, compact) |
| **Speed** | Moderate | 5–10× faster (binary + HTTP/2) |
| **Tooling** | Universal (curl, Postman) | Needs protobuf tools |
| **Streaming** | Hard | Native (bidirectional) |
| **Use when** | External API, web clients | Internal microservices, high throughput |

---

## 18. Serving Frameworks — TorchServe, TFServing, Triton, BentoML

> **Interview Q:** *What serving framework would you use for a production PyTorch model?*

### NVIDIA Triton Inference Server

Framework-agnostic, high-performance inference server for NVIDIA GPUs. Supports PyTorch, TensorFlow, ONNX, TensorRT, Python models.

**Key features:**
- Concurrent model execution (multiple models on one GPU)
- Dynamic batching (accumulate requests, batch them automatically)
- Model ensemble (chain multiple models)
- gRPC + REST endpoints

```
Model repository structure:
models/
  churn_model/
    1/                 # version 1
      model.onnx
    config.pbtxt       # model config
```

```protobuf
# config.pbtxt
name: "churn_model"
platform: "onnxruntime_onnx"
max_batch_size: 64

input [{ name: "input" data_type: TYPE_FP32 dims: [28] }]
output [{ name: "probability" data_type: TYPE_FP32 dims: [2] }]

dynamic_batching {
  preferred_batch_size: [16, 32, 64]
  max_queue_delay_microseconds: 5000   # wait up to 5ms to fill a batch
}
```

### BentoML

Pythonic framework for packaging and serving ML models:

```python
import bentoml
from bentoml.io import JSON, NumpyNdarray

# Save model to BentoML store
bentoml.sklearn.save_model("churn_model", model,
                            signatures={"predict_proba": {"batchable": True}})

# Define service
svc = bentoml.Service("churn_predictor")

runner = bentoml.sklearn.get("churn_model:latest").to_runner()
svc.add_runner(runner)

@svc.api(input=JSON(), output=JSON())
async def predict(features: dict) -> dict:
    arr = preprocess(features)
    proba = await runner.predict_proba.async_run(arr)
    return {"churn_probability": float(proba[0][1])}

# Build and serve
# bentoml serve service:svc --port 3000
# bentoml build → bentoml containerize → docker push
```

### Serving Framework Comparison

| Framework | Best For | GPU Support | Batching | Complexity |
|---|---|---|---|---|
| **FastAPI** | Custom logic, quick start | Manual | Manual | Low |
| **TorchServe** | PyTorch models | Yes | Yes | Medium |
| **TF Serving** | TensorFlow/Keras | Yes | Yes | Medium |
| **Triton** | High-throughput GPU serving | Yes (NVIDIA) | Dynamic batching | High |
| **BentoML** | Python-first, packaging | Yes | Yes | Medium |
| **Seldon Core** | K8s-native, multi-model | Yes | Yes | High |
| **Ray Serve** | Distributed, complex pipelines | Yes | Yes | Medium |

---

## 19. Model Optimisation for Serving

> **Interview Q:** *How do you optimise an ML model for production serving?*

### Quantisation

Reduce model weight precision → smaller size, faster inference:

```python
# PyTorch dynamic quantisation (CPU)
import torch.quantization

model_int8 = torch.quantization.quantize_dynamic(
    model,
    {torch.nn.Linear},  # quantise only linear layers
    dtype=torch.qint8
)
# Typical: 4× smaller, 2-3× faster on CPU, small accuracy loss

# ONNX + quantisation
from onnxruntime.quantization import quantize_dynamic, QuantType
quantize_dynamic("model.onnx", "model_int8.onnx", weight_type=QuantType.QInt8)
```

### Pruning

Remove redundant weights (set to zero) or entire neurons:

```python
import torch.nn.utils.prune as prune

# Remove 40% of weights in a linear layer (lowest magnitude)
prune.l1_unstructured(model.fc1, name="weight", amount=0.4)

# Global pruning across all layers
parameters_to_prune = [(model.fc1, "weight"), (model.fc2, "weight")]
prune.global_unstructured(parameters_to_prune, pruning_method=prune.L1Unstructured, amount=0.3)
```

### Knowledge Distillation

Train a small "student" model to mimic a large "teacher" model's outputs (soft labels), not just hard labels:

```python
# Distillation loss: cross-entropy on soft targets from teacher
T = 4   # temperature: higher = softer targets

with torch.no_grad():
    teacher_logits = teacher_model(x)

student_logits = student_model(x)

# Soft target loss (KL divergence on softened probabilities)
soft_loss = F.kl_div(
    F.log_softmax(student_logits / T, dim=1),
    F.softmax(teacher_logits / T, dim=1),
    reduction="batchmean"
) * (T ** 2)

# Hard label loss
hard_loss = F.cross_entropy(student_logits, labels)

loss = 0.7 * soft_loss + 0.3 * hard_loss
```

### TensorRT Optimisation

NVIDIA's optimisation library for GPU inference — fuses operations, optimises layer layout, applies quantisation:

```python
import tensorrt as trt

builder = trt.Builder(trt.Logger(trt.Logger.WARNING))
network = builder.create_network()
parser = trt.OnnxParser(network, builder.logger)

# Parse ONNX model
with open("model.onnx", "rb") as f:
    parser.parse(f.read())

# Build TensorRT engine with FP16 precision
config = builder.create_builder_config()
config.set_flag(trt.BuilderFlag.FP16)
config.set_memory_pool_limit(trt.MemoryPoolType.WORKSPACE, 1 << 30)  # 1GB

engine = builder.build_serialized_network(network, config)
# Typical speedup: 2-5× over raw PyTorch on GPU
```

### Optimisation Techniques Summary

| Technique | Reduction | Quality Loss | Notes |
|---|---|---|---|
| FP32 → FP16 | 2× size, 2× speed | Negligible | Start here |
| FP32 → INT8 | 4× size, 3-4× speed | Small | Use calibration dataset |
| FP32 → INT4 | 8× size, 5-6× speed | Moderate | LLM quantisation (GPTQ, AWQ) |
| Pruning | 30-90% params | Small-moderate | Structured pruning better for hardware |
| Distillation | 10-100× smaller model | Moderate | Train student from scratch |
| ONNX export | Marginal | None | Platform portability |
| TensorRT | 2-5× speed | Negligible | NVIDIA GPU only |
| Batch dynamic | N× throughput | None (latency ↑) | Same model, batch requests |

---

## 20. Model Monitoring — What, Why, How

> **Interview Q:** *How do you monitor a model in production? What metrics do you track?*

Model monitoring detects when a deployed model is no longer performing as expected — before users notice.

### Four Monitoring Layers

**1. Infrastructure Monitoring**
```
CPU / GPU utilisation
Memory usage
Request latency (P50, P95, P99)
Throughput (requests per second)
Error rate (5xx responses)
Pod restarts, OOMKills
```

**2. Pipeline Monitoring**
```
Feature computation latency
Feature freshness (are features up to date?)
Data pipeline failures / delays
```

**3. Data / Input Monitoring**
```
Input feature distributions (vs. training distribution)
Null rates per feature
Value range violations
New categorical values not seen in training
```

**4. Model Output / Performance Monitoring**
```
Prediction distribution (score distribution shifted?)
Prediction confidence distribution
Label / ground truth drift (when labels available with delay)
Business metrics (conversion rate, click-through, revenue)
Fairness metrics across demographic groups
```

### Monitoring Stack

```
Model Server → Prometheus (metrics scraping) → Grafana (dashboards)
            → Evidently AI (data + drift reports)
            → PagerDuty / Opsgenie (alerts)
            → ELK / Datadog (logs)
```

### Evidently AI — Data & Model Monitoring

```python
from evidently.report import Report
from evidently.metric_preset import DataDriftPreset, ClassificationPreset

# Compare production data to training data
report = Report(metrics=[
    DataDriftPreset(),        # check if input features drifted
    ClassificationPreset()    # check prediction quality (if labels available)
])

report.run(reference_data=training_df, current_data=production_df)
report.save_html("drift_report.html")
report.show()

# Extract drift scores programmatically
result = report.as_dict()
drift_detected = result["metrics"][0]["result"]["dataset_drift"]
n_drifted_features = result["metrics"][0]["result"]["number_of_drifted_columns"]
```

---

## 21. Data Drift & Concept Drift

> **Interview Q:** *What is the difference between data drift and concept drift? How do you detect them?*

### Types of Drift

**Data Drift (Covariate Shift):** The distribution of input features P(X) changes, but the relationship P(Y|X) stays the same.

```
Example: A fraud model trained on 2023 transactions.
In 2025, users make more mobile transactions than before (new behaviour).
→ The 'device_type' feature distribution has shifted.
→ If model was good at predicting fraud for mobile, it may still work,
  but the input distribution it was trained on is stale.
```

**Concept Drift:** The underlying relationship P(Y|X) changes — the meaning of "fraud" for a given set of features has changed.

```
Example: Fraudsters change tactics — patterns that previously indicated fraud
now appear in legitimate transactions. The feature values are similar but
what they mean for the label has changed.
→ Even perfect feature distributions won't help — the model is wrong.
```

**Label Drift:** Distribution of targets P(Y) changes — more fraud cases overall (independent of features).

**Prediction Drift:** Model's output distribution shifts — model predicting "fraud" more or less often overall.

### Drift Detection Methods

**Statistical Tests:**
```python
from scipy import stats
from evidently.calculations.stattests import ks_stat_test, chi_stat_test

# KS test for continuous features (age, amount)
ks_statistic, p_value = stats.ks_2samp(train_age, production_age)
drift_detected = p_value < 0.05   # significant difference in distribution

# Chi-squared test for categorical features (country, device_type)
observed = production_df["country"].value_counts()
expected = train_df["country"].value_counts()
chi2, p_value = stats.chisquare(observed, expected)
```

**Population Stability Index (PSI):**
```
PSI = Σ (actual% - expected%) × ln(actual% / expected%)
PSI < 0.1:  No significant drift
0.1–0.25:  Moderate drift — monitor
PSI > 0.25: Significant drift — investigate/retrain
```

**Wasserstein Distance:** Earth Mover's Distance — measures how much "work" it takes to transform one distribution into another. Better than KS for high-dimensional data.

**CUSUM (Cumulative Sum) / ADWIN:** Sequential drift detection algorithms that detect when a running statistic (e.g., prediction accuracy) changes significantly.

### Detecting Concept Drift (Harder)

Concept drift requires ground truth labels — often delayed (fraud labels arrive weeks later, medical outcomes take months):

```
Strategy 1: Ground truth monitoring
  → Collect delayed labels → compute actual vs. predicted metrics
  → Alert when F1 / AUC drops below threshold

Strategy 2: Proxy metrics
  → Monitor business KPIs (conversion rate, complaint rate)
  → Surrogate for model quality without labels

Strategy 3: Model confidence monitoring
  → If model is less confident (scores clustering near 0.5) → likely struggling
  → Entropy of prediction distribution as a drift signal
```

---

## 22. Alerting, Logging & Observability Stack

> **Interview Q:** *What does the observability stack look like for an ML system?*

### Metrics — Prometheus + Grafana

```python
# Instrument your FastAPI serving code with Prometheus metrics
from prometheus_client import Counter, Histogram, Gauge
import time

REQUEST_COUNT = Counter("ml_requests_total", "Total predictions", ["model_version", "status"])
REQUEST_LATENCY = Histogram("ml_request_duration_seconds", "Prediction latency",
                             buckets=[0.01, 0.05, 0.1, 0.5, 1.0])
PREDICTION_SCORE = Histogram("ml_prediction_score", "Score distribution",
                              buckets=[0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9])

@app.post("/predict")
async def predict(features: dict):
    start = time.time()
    try:
        pred, proba = model.predict(features)
        REQUEST_COUNT.labels(model_version="v3", status="success").inc()
        PREDICTION_SCORE.observe(float(proba))
        return {"prediction": int(pred), "probability": float(proba)}
    except Exception as e:
        REQUEST_COUNT.labels(model_version="v3", status="error").inc()
        raise
    finally:
        REQUEST_LATENCY.observe(time.time() - start)

# Expose metrics endpoint
@app.get("/metrics")
def metrics():
    from prometheus_client import generate_latest
    return Response(generate_latest(), media_type="text/plain")
```

### Logging — Structured JSON Logs

```python
import structlog
import json

logger = structlog.get_logger()

@app.post("/predict")
async def predict(features: dict, request_id: str):
    pred, proba = model.predict(features)
    logger.info("prediction_made",
                request_id=request_id,
                model_version="v3",
                prediction=int(pred),
                probability=float(proba),
                features=features,   # log input features for drift analysis
                latency_ms=latency)
    return {"prediction": int(pred)}
```

**Why structured logging:** JSON logs can be queried in ELK/Datadog. Log input features → sample and run drift analysis offline.

### Alerting Rules (Prometheus AlertManager)

```yaml
# alerting_rules.yaml
groups:
- name: ml_model_alerts
  rules:
  - alert: HighPredictionLatency
    expr: histogram_quantile(0.95, ml_request_duration_seconds) > 0.5
    for: 5m
    annotations:
      summary: "P95 latency > 500ms for 5 minutes"

  - alert: HighErrorRate
    expr: rate(ml_requests_total{status="error"}[5m]) / rate(ml_requests_total[5m]) > 0.05
    for: 2m
    annotations:
      summary: "Error rate > 5%"

  - alert: PredictionScoreDrift
    expr: histogram_quantile(0.5, ml_prediction_score) < 0.2 or
          histogram_quantile(0.5, ml_prediction_score) > 0.8
    for: 10m
    annotations:
      summary: "Median prediction score drifted — model may be degrading"
```

---

## 23. Retraining Strategies

> **Interview Q:** *When should you retrain a model and how do you automate it?*

### When to Retrain

```
Trigger Type         Indicator                              Threshold Example
────────────────────────────────────────────────────────────────────────────
Scheduled            Time-based                             Every 2 weeks
Data volume          New labelled samples accumulated       10,000 new labels
Performance drop     AUC below threshold                    AUC < 0.88
Data drift           PSI score exceeded                     PSI > 0.25
Concept drift        Ground truth accuracy drop             Accuracy < 0.80
Business metric      Conversion rate dropped                CTR down 5%
```

### Retraining Data Strategy

**Full retraining:** Train from scratch on all historical + new data. Clean slate, but expensive.

**Incremental (continual) learning:** Fine-tune existing model on new data only. Fast but risks catastrophic forgetting.

**Sliding window:** Train only on the most recent N months of data. Adapts to recent patterns; forgets old ones (intentional for drifting concepts).

**Weighted sampling:** Use all data but weight recent data higher. Balance between full history and recency.

### Automated Retraining Pipeline

```python
# drift_monitor.py — runs as a CronJob every hour
from evidently.report import Report
from evidently.metric_preset import DataDriftPreset

def check_drift_and_trigger_retraining():
    report = Report(metrics=[DataDriftPreset()])
    report.run(reference_data=training_features, current_data=last_24h_features)

    result = report.as_dict()
    psi = result["metrics"][0]["result"]["dataset_drift_score"]

    if psi > 0.25:
        logger.warning("Drift detected", psi=psi)
        # Trigger Airflow DAG
        import requests
        requests.post(
            "http://airflow:8080/api/v1/dags/retrain_churn_model/dagRuns",
            json={"conf": {"trigger_reason": f"drift_psi_{psi:.3f}"}},
            auth=("admin", AIRFLOW_PASSWORD)
        )
```

---

## 24. Cloud ML Platforms — SageMaker, Vertex AI, AzureML

> **Interview Q:** *Compare AWS SageMaker, Google Vertex AI, and Azure ML.*

### AWS SageMaker

End-to-end ML platform on AWS. Managed infrastructure for every stage of the ML lifecycle.

**Key features:**
- **SageMaker Studio:** Jupyter-based IDE for data science
- **Training Jobs:** Managed distributed training (any framework)
- **SageMaker Pipelines:** MLOps pipeline orchestration (like Kubeflow but native to AWS)
- **Feature Store:** Managed feature store (online + offline)
- **Model Registry:** Version and deploy models
- **Endpoints:** Managed real-time inference (auto-scaling)
- **Batch Transform:** Managed batch inference
- **Model Monitor:** Automated drift detection and data quality monitoring
- **Clarify:** Bias detection and explainability

```python
# SageMaker training job
from sagemaker.sklearn import SKLearn

estimator = SKLearn(
    entry_point="train.py",
    framework_version="1.2-1",
    instance_type="ml.m5.xlarge",
    role=ROLE,
    hyperparameters={"n_estimators": 500, "max_depth": 6}
)
estimator.fit({"train": "s3://bucket/train/", "test": "s3://bucket/test/"})

# Deploy to endpoint
predictor = estimator.deploy(
    initial_instance_count=2,
    instance_type="ml.m5.large"
)
```

### Google Vertex AI

GCP's unified ML platform. Similar to SageMaker.

**Distinctive features:**
- Vertex AI Pipelines (based on Kubeflow Pipelines)
- Managed Tensorboard for training visualisation
- Vertex AI Feature Store
- Model Evaluation with TFMA
- Strong BigQuery integration

### Azure Machine Learning

Microsoft's ML platform on Azure.

**Distinctive features:**
- Tight integration with Azure Databricks for data engineering
- MLflow as first-class experiment tracking
- Managed Responsible AI dashboard (fairness, explainability, error analysis)
- Designer (drag-and-drop pipeline builder)

### Platform Comparison

| | SageMaker | Vertex AI | Azure ML |
|---|---|---|---|
| **Ecosystem** | AWS | GCP | Azure |
| **Pipelines** | SageMaker Pipelines | KFP-based Vertex Pipelines | Azure Pipelines + MLflow |
| **Feature Store** | SageMaker FS | Vertex Feature Store | Azure FS (limited) |
| **Experiment tracking** | SageMaker Experiments | Vertex Experiments | MLflow (native) |
| **Best if you use** | AWS stack | GCP / BigQuery | Microsoft / Azure stack |
| **Strength** | Mature, wide feature set | BigQuery + TFX integration | Azure DevOps integration |

---

## 25. Infrastructure as Code for ML

> **Interview Q:** *What is Infrastructure as Code and why does it matter in MLOps?*

IaC means defining infrastructure (servers, databases, networks) in code — version-controlled, repeatable, and automated. Eliminates "it works in staging but not prod" due to infrastructure differences.

### Terraform for ML Infrastructure

```hcl
# main.tf — provision ML infrastructure on AWS
provider "aws" { region = "ap-south-1" }

# S3 bucket for model artefacts
resource "aws_s3_bucket" "model_artifacts" {
  bucket = "mycompany-ml-models"
  versioning { enabled = true }
}

# ECR repository for Docker images
resource "aws_ecr_repository" "model_server" {
  name = "churn-model-server"
  image_scanning_configuration { scan_on_push = true }
}

# EKS cluster for inference
module "eks" {
  source          = "terraform-aws-modules/eks/aws"
  cluster_name    = "ml-inference-cluster"
  cluster_version = "1.29"
  vpc_id          = module.vpc.vpc_id
  subnet_ids      = module.vpc.private_subnets

  node_groups = {
    inference_cpu = {
      desired_capacity = 3
      max_capacity     = 20
      min_capacity     = 2
      instance_types   = ["m5.xlarge"]
    }
    inference_gpu = {
      desired_capacity = 1
      max_capacity     = 5
      min_capacity     = 0
      instance_types   = ["g4dn.xlarge"]
    }
  }
}
```

```bash
terraform init      # initialise providers
terraform plan      # preview changes
terraform apply     # provision infrastructure
terraform destroy   # tear down
```

---

## 26. Cost Optimisation in MLOps

> **Interview Q:** *How do you reduce costs in an ML production system?*

**Training costs:**
- Spot/Preemptible instances (60–90% cheaper) — checkpoint frequently to survive interruptions
- Mixed precision training (FP16) — faster, smaller memory → smaller GPU needed
- Distributed training only when needed — single GPU often sufficient for < 1B param models
- Gradient accumulation — simulate larger batch without more memory

**Inference costs:**
- Autoscaling — scale to zero during off-hours for non-critical services
- Batch inference where real-time not needed — 10× cheaper
- Model quantisation (INT8/INT4) — 2–4× lower inference cost
- Model distillation — 10–100× smaller model → smaller instance needed
- Caching — identical input = cached output (semantic cache for LLMs)
- Right-sizing — don't use A100 for a logistic regression model

**Storage costs:**
- DVC with lifecycle policies — auto-archive old model versions to cold storage
- Delete failed experiment artefacts — MLflow has built-in cleanup utilities
- Parquet over CSV — 5–10× smaller; columnar → faster queries

**Overall:**
```
Monthly ML cost audit:
1. Training compute: are all jobs necessary? right-sized?
2. Inference compute: are all endpoints active? auto-scaling configured?
3. Storage: old model versions, stale datasets, unused feature store data
4. Data transfer: are you moving data across regions unnecessarily?
```

---

## 27. Model Governance & Compliance

> **Interview Q:** *What is model governance and why does it matter?*

Model governance is the set of policies, processes, and controls ensuring that ML models are developed, deployed, and operated in a reliable, explainable, fair, and compliant manner.

### What Governance Requires

**Auditability — For every model in production, you must be able to answer:**
```
Who trained it? (data scientist, team)
When was it trained? (timestamp)
What data was used? (data version, source, date range)
What code was used? (Git commit hash)
What metrics does it have? (offline evaluation)
Who approved it for production? (sign-off trail)
What monitoring is in place?
How are predictions explained? (SHAP, LIME)
When was it last retrained?
```

**Model Cards:** Documentation template for every production model:
```
Model Card: Churn Prediction Model v3
─────────────────────────────────────
Model: LightGBM Classifier
Version: 3.2.1
Last trained: 2025-11-01
Training data: 2024-01-01 to 2025-10-31 (1.2M transactions)
Features used: [28 features listed]

Performance:
  AUC-ROC: 0.923
  F1 Score: 0.871
  Precision: 0.879
  Recall: 0.863

Fairness:
  AUC by region: IN=0.921, US=0.925, UK=0.918
  No significant disparate impact detected

Limitations:
  May underperform for accounts < 3 months old
  Not validated for B2B accounts

Monitoring:
  Drift alert threshold: PSI > 0.25
  Retraining trigger: AUC < 0.88

Owner: ML Platform Team
Contact: ml-platform@company.com
```

### GDPR / AI Act Compliance Considerations

- **Right to explanation:** For consequential automated decisions (loan rejection, healthcare), must explain why
- **Data lineage:** Track what personal data was used in training
- **Right to be forgotten:** Remove individual's data from training set → potentially retrain
- **Model bias audits:** Document demographic fairness metrics
- **Human oversight:** High-stakes decisions (lending, hiring, medical) require human review

---

## 28. Responsible AI & Bias Detection

> **Interview Q:** *How do you detect and mitigate bias in ML models?*

### Types of Bias

**Historical bias:** Training data reflects past discrimination (e.g., historical loan approvals biased against women).
**Representation bias:** Certain groups underrepresented in training data → worse performance for them.
**Measurement bias:** Features measured differently across groups (e.g., self-reported income varies by education level).
**Label bias:** Human annotators introduce subjective biases into labels.
**Aggregation bias:** Single model for heterogeneous populations — one group's patterns dominate.

### Fairness Metrics

```python
from sklearn.metrics import confusion_matrix
import numpy as np

def compute_fairness_metrics(y_true, y_pred, sensitive_feature):
    groups = np.unique(sensitive_feature)
    metrics = {}

    for group in groups:
        mask = sensitive_feature == group
        tn, fp, fn, tp = confusion_matrix(y_true[mask], y_pred[mask]).ravel()

        metrics[group] = {
            "TPR":  tp / (tp + fn),   # True Positive Rate (Recall)
            "FPR":  fp / (fp + tn),   # False Positive Rate
            "PPV":  tp / (tp + fp),   # Positive Predictive Value (Precision)
            "accuracy": (tp + tn) / (tp + tn + fp + fn)
        }

    # Equalised odds: TPR and FPR should be equal across groups
    tpr_diff = max(m["TPR"] for m in metrics.values()) - min(m["TPR"] for m in metrics.values())
    fpr_diff = max(m["FPR"] for m in metrics.values()) - min(m["FPR"] for m in metrics.values())

    return metrics, tpr_diff, fpr_diff

# Disparate impact: ratio of positive prediction rates
# Ratio < 0.8 → adverse impact (the "4/5 rule" in US employment law)
```

### Explainability — SHAP

```python
import shap

explainer = shap.TreeExplainer(model)
shap_values = explainer.shap_values(X_test)

# Global feature importance
shap.summary_plot(shap_values, X_test, plot_type="bar")

# Local explanation for one prediction
shap.force_plot(explainer.expected_value, shap_values[0], X_test.iloc[0])
# Shows: "This prediction of churn=0.92 was driven by: high_usage_30d (+0.3),
#         no_support_calls (+0.2), recent_plan_downgrade (+0.15) ..."
```

**Fairlearn:** Microsoft library for assessing and mitigating unfairness:

```python
from fairlearn.metrics import MetricFrame
from sklearn.metrics import accuracy_score

mf = MetricFrame(
    metrics=accuracy_score,
    y_true=y_test,
    y_pred=preds,
    sensitive_features=test_df["gender"]
)
print(mf.by_group)          # accuracy per gender group
print(mf.difference())      # max difference in accuracy across groups
```

---

## 29. Interview Q&A — Fundamentals & Concepts

**Q: What is MLOps and what problems does it solve?**
> MLOps combines ML, DevOps, and Data Engineering to automate and operationalise the ML lifecycle. It solves: (1) Slow, manual model deployment — MLOps CI/CD reduces deployment from weeks to hours; (2) Silent model degradation — monitoring and drift detection catch issues before users notice; (3) Irreproducibility — version control for data, code, environments, and experiments ensures any result can be recreated; (4) Lack of collaboration — standardised pipelines and registries let data scientists and engineers work together; (5) No audit trail — governance tools document every model decision.

**Q: What is training-serving skew?**
> When the feature computation logic used during training differs from what runs at serving time, the model sees different data than it was trained on — silently degrading performance. Common causes: different preprocessing code in Python (training) vs Java (production), using future information in training features, or different null-handling logic. Prevention: use a Feature Store so features are computed once and shared, log production features and compare their distribution to training, write preprocessing as a single shared function used in both paths.

**Q: What is the difference between data drift and concept drift?**
> Data drift (covariate shift): the input feature distribution P(X) changes but the relationship P(Y|X) stays the same. The model may still be correct, but input patterns it hasn't seen are becoming more common. Concept drift: the underlying relationship P(Y|X) changes — what a given set of features predicts has shifted. Requires retraining to fix. Data drift is detected by comparing current input distributions to training distributions (KS test, PSI). Concept drift requires ground truth labels (often delayed) to detect through performance metric degradation.

**Q: What is the purpose of a model registry?**
> A model registry is a centralised store for managing model lifecycle stages from experiment to production. It stores: model artefacts (weights), metadata (training data version, Git hash, hyperparameters, metrics), environment info, and stage (Staging/Production/Archived). It enables: reproducibility (recreate any deployed model), auditability (who approved this model and when), safe deployment (human review gate before Production), and rollback (revert to a previous version in seconds).

**Q: Explain the three types of tests in an ML CI/CD pipeline.**
> (1) Unit tests: test individual functions — preprocessing, feature engineering, data validation logic — in isolation with mock data. Fast, run on every commit. (2) Model quality tests: run the model on a held-out evaluation set and assert metrics (AUC ≥ 0.90, F1 ≥ 0.85, latency < 50ms) — ensure the candidate model doesn't regress from the production model. (3) Integration/E2E tests: run the full pipeline end-to-end on a small representative dataset and verify the final prediction output format, schema, and value range. Slower, run before deployment.

---

## 30. Interview Q&A — Deployment & Serving

**Q: What is a canary deployment and why use it over blue-green?**
> Canary deployment routes a small percentage (1–5%) of real traffic to the new model, gradually increasing if metrics hold. Blue-green switches all traffic instantly. Canary is preferred for ML because: (1) It limits blast radius — a bad model only affects a small fraction of users; (2) Validates on real production traffic rather than staging data; (3) Allows statistical validation before full rollout. Blue-green is better when you need instant rollback and can't tolerate any degraded performance during the rollout window.

**Q: What is the difference between online and batch inference?**
> Online inference serves predictions in real-time for individual requests — latency < 200ms, requires always-on infrastructure, suitable for fraud detection or product recommendations at click time. Batch inference processes large datasets offline on a schedule — latency in hours is acceptable, compute is ephemeral, much cheaper, suitable for pre-computing scores for all users overnight. Choose batch when predictions don't need to be real-time; choose online when user experience depends on the immediate prediction.

**Q: How would you serve 1000 QPS from a deep learning model?**
> (1) Use a high-performance serving framework (Triton for GPU, BentoML/Ray Serve for CPU); (2) Enable dynamic batching — accumulate requests for up to 5ms and batch them together for GPU efficiency; (3) Optimise the model with FP16 or TensorRT for 2–5× speedup; (4) Deploy multiple replicas behind a load balancer with a Kubernetes HPA; (5) Use asynchronous request handling (FastAPI async endpoints); (6) Cache identical requests (semantic or exact); (7) Add a CDN for cacheable responses.

**Q: How do you perform a rollback if a newly deployed model is causing issues?**
> (1) Canary rollback: immediately route 100% traffic back to the previous model version (Kubernetes rollout undo); (2) Model registry rollback: transition the previous version back to Production stage, triggering an automatic redeployment; (3) A/B testing infrastructure: instantly set the traffic split to 100% old model via a feature flag or config change without any redeployment. All these take < 1 minute. Key: never delete or overwrite the previous model version — keep it in the registry for instant recovery.

---

## 31. Interview Q&A — Monitoring & Drift

**Q: What metrics do you monitor for a classification model in production?**
> Infrastructure: request latency (P50/P95/P99), error rate, CPU/memory/GPU. Data input: feature distribution per column (vs training baseline), null rates, new categorical values, feature freshness. Model outputs: prediction score distribution (histogram), confidence distribution (are predictions becoming less confident?), class distribution (is positive prediction rate changing?). Ground truth (when labels arrive): accuracy, F1, AUC-ROC vs baseline. Business: conversion rate, fraud prevented, churn reduction — the ultimate measure of model value.

**Q: How would you detect that your fraud model has degraded in production?**
> Layer 1 (immediate): monitor prediction score distribution — if the median fraud probability drops significantly, the model may be missing fraud cases. Layer 2 (proxy): monitor business metric — investigate rate, chargeback rate, false decline rate (24-48h delay). Layer 3 (ground truth): as fraud labels come in (1–2 week delay), compute AUC/F1 on labelled production data and alert if below threshold. Layer 4: run PSI on input features weekly to detect data drift that might explain degradation. The combination of these four layers catches degradation at different latencies.

**Q: What is PSI and how is it used for drift detection?**
> Population Stability Index (PSI) measures how much a feature's distribution has shifted between reference (training) and current (production) data. PSI = Σ (current_bucket% - reference_bucket%) × ln(current%/reference%). PSI < 0.1: no significant shift; 0.1–0.25: moderate drift, monitor closely; > 0.25: significant drift, investigate and likely retrain. PSI is computed per feature. If many features have high PSI simultaneously, the input distribution has fundamentally changed.

---

## 32. Interview Q&A — Tools & Systems Design

**Q: Compare MLflow and Weights & Biases.**
> MLflow is fully open-source, can be entirely self-hosted, has a model registry built-in, and integrates well with Spark/Databricks. Best for production ML teams that need data sovereignty or on-premises deployment. W&B has a better UI, richer visualisations, built-in hyperparameter sweep management (Bayesian), and team collaboration features. Best for research-focused teams doing heavy experimentation. Both track params, metrics, and artefacts — the choice often depends on existing infrastructure and team culture.

**Q: What is DVC and when do you use it?**
> DVC (Data Version Control) solves the problem of versioning large ML datasets and model files that can't go in Git. DVC stores metadata pointers (`.dvc` files) in Git while the actual data lives in cloud storage (S3, GCS). This lets you track `git checkout HEAD~5` and then `dvc checkout` to restore the exact data state from 5 commits ago. Use DVC for: reproducible experiment tracking, dataset versioning, pipeline automation (`dvc repro`), and connecting data versions to code versions.

**Q: Design an end-to-end MLOps system for a churn prediction model.**
> Data layer: Raw data in S3 versioned with DVC. Great Expectations validates schema and distributions daily. Feast Feature Store provides consistent features to both training and serving. Training layer: Airflow schedules weekly retraining DAG. MLflow tracks all experiments (params, metrics, artefacts). Model trained and evaluated against gates (AUC ≥ 0.90, F1 ≥ 0.85). Best model registered in MLflow Model Registry. CI/CD: GitHub Actions runs unit tests, data validation, training, evaluation, Docker build, and canary deploy on merge to main. Serving: FastAPI + Triton on Kubernetes, HPA scales 2–20 replicas based on CPU. Monitoring: Prometheus + Grafana for infrastructure, Evidently for drift (PSI), weekly ground truth evaluation against incoming labels. Alerts via PagerDuty. Auto-retraining triggered when AUC drops below 0.88 or PSI > 0.25.

**Q: What is the difference between Airflow and Kubeflow?**
> Airflow is a general-purpose workflow orchestrator — DAGs of tasks in Python, runs on any infrastructure (VM, Docker, K8s). Widely adopted for data pipelines and ETL. Not ML-specific. Kubeflow is ML-specific and Kubernetes-native — every pipeline component runs as a K8s Pod, supports GPU scheduling natively, integrates with Kubernetes ecosystem tools (Seldon, KFServing). Better for ML workloads that need GPU resource scheduling and tight K8s integration. Harder to set up. Many teams use Airflow for data pipelines and Kubeflow for ML training pipelines.

---

## 33. Quick Reference Cheat Sheet

### MLOps Stack by Layer

```
Data Versioning:        DVC + Git + S3/GCS
Data Validation:        Great Expectations, TFX Data Validation
Feature Store:          Feast, Hopsworks, SageMaker Feature Store
Experiment Tracking:    MLflow, Weights & Biases
Model Registry:         MLflow Registry, Vertex Model Registry
Pipeline Orchestration: Airflow, Kubeflow, Prefect
CI/CD:                  GitHub Actions, GitLab CI, Jenkins
Containerisation:       Docker, Kaniko (CI), BuildKit
Container Orchestration:Kubernetes (EKS, GKE, AKS)
Model Serving:          FastAPI, BentoML, Triton, TorchServe, Seldon
Model Optimisation:     ONNX, TensorRT, Quantisation, Pruning
Monitoring:             Prometheus + Grafana, Evidently, Fiddler
Logging:                ELK Stack, Datadog, Structlog
Alerting:               PagerDuty, Opsgenie, Prometheus AlertManager
Cloud Platform:         AWS SageMaker, Google Vertex AI, Azure ML
IaC:                    Terraform, Pulumi, AWS CDK
```

### Deployment Strategy Selection

```
New model, unknown risks:   Shadow deploy first → Canary → Full rollout
Standard production update: Canary (1% → 5% → 20% → 100%)
Critical system, fast fix:  Blue-Green (instant rollback)
Comparing business impact:  A/B Test (held constant, statistical analysis)
Hotfix/rollback:            Kubernetes rollout undo → < 1 minute
```

### Drift Detection Quick Reference

```
Data drift (P(X)):      KS test (continuous), Chi-squared (categorical), PSI
Concept drift (P(Y|X)): Ground truth monitoring, business KPI metrics
Label drift (P(Y)):     Prediction distribution monitoring
PSI thresholds:         < 0.1 (ok), 0.1–0.25 (monitor), > 0.25 (retrain)

Tools: Evidently AI, WhyLogs, Fiddler, SageMaker Model Monitor
```

### Retraining Trigger Decision

```
IF PSI > 0.25 on key features → retrain (data drift)
IF AUC drops below threshold  → retrain (performance drop)
IF N new labels accumulated   → retrain (data volume)
IF scheduled interval passed  → retrain (scheduled)
ALWAYS:  validate new model beats production before deploying
```

### MLOps Maturity Quick Reference

```
Level 0: Manual notebooks, one-time deployment, no monitoring
Level 1: Automated training pipeline, MLflow tracking, feature store, basic monitoring
Level 2: Full CI/CD/CT, automated drift detection + retraining, A/B deployment, full governance
```

### Key Concepts to Know by Heart

```
Reproducibility:       (git_hash, data_version, hyperparams, random_seeds) → same model
Training-serving skew: Different feature logic at train vs. serve → silent degradation
Model registry stages: None → Staging → Production → Archived
Champion-Challenger:   90% traffic old model, 10% new → promote if new wins
ONNX:                  Cross-framework model format for portable production inference
KV Cache:              Store past attention keys/values → fast autoregressive LLM serving
PSI:                   Population Stability Index — drift detection metric (> 0.25 = retrain)
SHAP:                  SHapley Additive exPlanations — explain individual predictions
Canary:                Gradual traffic shift to new model with monitoring
Blue-Green:            Instant traffic switch between two environments
```

---

*Sources: DataCamp MLOps Interview Questions, LockedIn AI Top 25 MLOps Questions, CredoSystemz Top 50, Devinterview.io MLOps repo, Medium Dr. Sanjay Kumar MLOps Q&A, IGMGuru MLOps Interview, GrowAI MLOps 2026 Guide, Hopsworks Feature Store blog, Medium Airflow+DVC+MLflow+Kubernetes — question banks from 2024–2026 MLOps engineer interviews at Google, Meta, Amazon, Databricks, Uber, and ML infrastructure startups.*
