# Fraud Detection Notebooks and Pipeline

This directory contains the notebook-based workflow and OpenShift AI pipeline for the fraud detection implementation.

**1_experiment_train.ipynb** : Loads and preprocesses the transaction dataset, trains the TensorFlow model, evaluates it, and prepares it for export.

**2_save_model.ipynb** : Exports the trained model to ONNX (`models/fraud/1/model.onnx`) and uploads it to MinIO AIStor (`fraud-models` bucket).

**3_rest_requests.ipynb** : Validates the deployed model via its KServe/OpenVINO REST endpoint:
`Jupyter → REST request → KServe → OpenVINO Model Server → ONNX model → prediction response` (V2 model serving API).

**4-train-save.pipeline** : Data Science Pipeline automating training and save:
`1_experiment_train.ipynb → model.onnx → 2_save_model.ipynb → MinIO AIStor`

**Storage & credentials** : Provided through OpenShift AI Connections and Kubernetes Secrets. No access keys are stored in notebooks or pipeline definitions.

**Attribution** : Adapted from the Red Hat OpenShift AI fraud detection tutorial, extended with MinIO AIStor integration, model serving, pipeline configuration, and validation for this self-hosted environment.
