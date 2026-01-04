# Liveness Detection Utilizing Ensemble Deep Learning Method for Biometric Authentication

This project implements a robust deep learning-based **Face Liveness Detection** system to distinguish between "real" human faces and various spoofing attacks, including 3D masks, deepfakes, printed photos, and video replays.

## Overview

The core of this project is an **Ensemble Pipeline** that combines the strengths of two distinct deep learning architectures:

1. **YOLOv11-cls**: High-speed image classification optimized for real-time performance.
2. **EfficientNetV2-S**: A highly accurate convolutional neural network known for its efficient parameter usage and scaling.

The pipeline utilizes a **weighted average ensemble** strategy. Through extensive grid search, optimal weights are assigned to each model's prediction to minimize error rates, specifically the **ACER (Average Classification Error Rate)**.

## Dataset

The models are trained and evaluated on a multi-class dataset categorized into:

* `real`: Authentic human face presentations.
* `3dmask`: Physical 3D mask attacks.
* `deepfake`: Digitally manipulated face swaps or generations.
* `print`: High-resolution printed photo attacks.
* `replay`: Video replay attacks on digital screens.

## Project Structure

* **Scenario 1 (`liveness_detection-s1.ipynb`)**: Baseline model training and initial ensemble testing.
* **Scenario 2 (`liveness_detection-s2.ipynb`)**: Evaluation and fine-tuning across different dataset splits or parameters.
* **Augmentation & Evaluation (`liveness_detection-s1-aug-eval.ipynb`)**: Detailed performance analysis, grid search for optimal ensemble weights, and data augmentation experiments.

## 📈 Performance & Metrics

The project uses industry-standard metrics for biometric security:

* **APCER** (Attack Presentation Classification Error Rate): Rate of "fakes" classified as "real."
* **BPCER** (Bona Fide Presentation Classification Error Rate): Rate of "reals" classified as "fake."
* **ACER**: The average of APCER and BPCER (Lower is better).

### Best Results (Scenario 1 Evaluation)

| Model | Accuracy | ACER | AUC |
| --- | --- | --- | --- |
| **YOLOv11** | ~93.3% | 0.1020 | 0.9920 |
| **EfficientNetV2** | ~94.2% | - | 0.9926 |
| **Ensemble (Optimal)** | **~96.7%** | **0.0675** | **0.9971** |

> **Note**: The optimal weights found for the ensemble were approximately **85.3% for YOLOv11** and **14.7% for EfficientNetV2**.

## Installation & Usage

### Prerequisites

* Python 3.8+
* PyTorch
* Ultralytics (for YOLOv11)
* Scikit-learn, Pandas, Matplotlib

### Setup

```bash
pip install ultralytics pulp torch torchvision scikit-learn

```

### Running the Pipeline

The ensemble pipeline can be executed as follows:

```python
# Initialize and run the pipeline
pipeline = LivenessEnsemblePipeline(
    yolo_model_path="path/to/yolo.pt",
    effnet_model_path="path/to/effnet.pth",
    device="cuda"
)

# Evaluate on test set
pipeline.evaluate(test_dataloader)

```

## Acknowledgments

Developed using **Ultralytics YOLOv11** and **Torchvision EfficientNet** implementations. Dataset processing and evaluation scripts are tailored for high-security liveness detection benchmarks.
