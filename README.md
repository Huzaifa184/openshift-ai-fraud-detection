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





