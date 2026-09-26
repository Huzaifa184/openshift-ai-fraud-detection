# Fraud Detection MLOps on Red Hat OpenShift AI

End-to-end fraud detection workflow on **Red Hat OpenShift AI 3.4**, covering model development, ONNX packaging, S3-compatible object storage, KServe/OpenVINO model serving, REST inference, and Data Science Pipelines.

The implementation runs on a self-hosted OpenShift lab and adapts the Red Hat OpenShift AI fraud detection tutorial to use **MinIO AIStor** and the existing platform infrastructure.

---

## Architecture

<img width="1536" height="1024" alt="112" src="https://github.com/user-attachments/assets/d17ec0ae-cfe4-4607-b505-4d6f1cf5af35" />


```mermaid
flowchart LR
    DATA[Transaction Dataset]

    subgraph RHOAI[Red Hat OpenShift AI 3.4]
        WB[Jupyter Workbench]
        TRAIN[TensorFlow Training]
        PIPE[Data Science Pipelines]
        KS[KServe]
        OVMS[OpenVINO Model Server]
    end

    subgraph S3[MinIO AIStor]
        MODEL[fraud-models]
        ART[fraud-pipeline-artifacts]
    end

    CLIENT[REST Client]

    DATA --> WB
    WB --> TRAIN
    TRAIN -->|ONNX| MODEL

    PIPE --> TRAIN
    PIPE --> MODEL
    PIPE --> ART

    MODEL --> OVMS
    OVMS --> KS
    CLIENT -->|Inference Request| KS
    KS -->|Prediction| CLIENT
```

Screenshots:
OpenShift 4.22.5

<img width="1277" height="465" alt="openshift_nodes" src="https://github.com/user-attachments/assets/893be533-3bc3-46f2-934c-df6e2d5da36a" />
<img width="650" height="651" alt="image" src="https://github.com/user-attachments/assets/8c4cc404-42c4-40ab-964c-5727d8e93040" />


Project Overview

<img width="1274" height="727" alt="project" src="https://github.com/user-attachments/assets/617f4d60-8cf4-4bea-8bdc-459361eb3716" />

Storage Connections

<img width="1262" height="577" alt="connections" src="https://github.com/user-attachments/assets/4517b998-4953-4cca-a416-70a5e28f5855" />

Object Storage

<img width="1270" height="415" alt="minio_buckets" src="https://github.com/user-attachments/assets/e3344c92-e610-4b18-9cb0-6d883598e9d8" />

Jupyter Workbench

<img width="1272" height="731" alt="jupyter-1" src="https://github.com/user-attachments/assets/553edee2-cd8f-4b02-91fc-14af4b129141" />

Model Training

<img width="1268" height="746" alt="training" src="https://github.com/user-attachments/assets/5cc1b4da-080c-48be-b9f0-4984fe40962c" />

Model Deployment

<img width="1262" height="646" alt="model-deploy" src="https://github.com/user-attachments/assets/24ab7e58-e778-4fe0-9755-1cef66d5d16d" />

Normal Transaction

<img width="1250" height="578" alt="result-1" src="https://github.com/user-attachments/assets/7c8fd572-f8a0-4f3f-9cdd-11efeba762ec" />

Fraud Detection

<img width="1264" height="588" alt="result-2" src="https://github.com/user-attachments/assets/47daa751-d64d-4b37-8714-76524b28c7fa" />
