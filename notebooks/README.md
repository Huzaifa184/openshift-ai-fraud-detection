# Fraud Detection Notebooks and Pipeline

This directory contains the notebook-based workflow and OpenShift AI pipeline used for the fraud detection implementation.

## Files

### `01-model-training.ipynb`

Trains and evaluates the fraud detection model using the transaction dataset.

Main steps:

- load and preprocess the dataset
- train the TensorFlow model
- evaluate the model
- prepare the model for export

---

### `02-model-export-and-upload.ipynb`

Exports the trained model to ONNX format and uploads the artifact to S3-compatible object storage.

Model artifact:

```text
models/fraud/1/model.onnx
```

Storage target:

```text
MinIO AIStor
└── fraud-models
```

---

### `03-rest-inference.ipynb`

Tests the deployed fraud detection model through its KServe/OpenVINO REST inference endpoint.

Request flow:

```text
Jupyter Workbench
      ↓
REST Request
      ↓
KServe
      ↓
OpenVINO Model Server
      ↓
ONNX Model
      ↓
Prediction Response
```

The notebook validates that the deployed model can receive transaction data and return an inference result through the V2 model serving API.

---

### `04-train-save.pipeline`

OpenShift AI Data Science Pipeline that automates the training and model-save workflow.

Pipeline flow:

```text
01-model-training.ipynb
        ↓
models/fraud/1/model.onnx
        ↓
02-model-export-and-upload.ipynb
        ↓
MinIO AIStor
```

The pipeline connects the training and model-save stages into a repeatable workflow.

## Storage and Credentials

Storage credentials are provided through OpenShift AI Connections and Kubernetes Secrets.

No access keys or secret keys are stored directly in the notebooks or pipeline definition.


## Attribution

This workflow is adapted from the Red Hat OpenShift AI fraud detection tutorial.

The notebooks and pipeline structure are based on the upstream example and were adapted for this self-hosted OpenShift AI environment, including MinIO AIStor integration, model serving, pipeline configuration, and validation.
