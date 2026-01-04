# Liveness Detection Utilizing Ensemble Deep Learning Method for Biometric Authentication

This repository contains the implementation of a facial liveness detection system using an ensemble of **YOLOv11** and **EfficientNetV2**. The research focuses on strengthening biometric authentication against diverse presentation attacks, including print, replay, 3D masks, and deepfakes.

## Overview

The core of this project is an **Ensemble Pipeline** that combines the strengths of two distinct deep learning architectures:

1. **YOLOv11-cls**: High-speed image classification optimized for real-time performance.
2. **EfficientNetV2-S**: A highly accurate convolutional neural network known for its efficient parameter usage and scaling.

The pipeline utilizes a **weighted average ensemble** strategy. Through extensive grid search, optimal weights are assigned to each model's prediction to minimize error rates, specifically the **ACER (Average Classification Error Rate)**.

### Key Contributions:

1. **Diverse Attack Classes**: Expansion beyond standard 2D attacks to include 3D masks (HKBU-MARS V1+) and deepfakes (Celeb-DF v2).
2. **Optimized Ensemble**: Implementation of a grid search technique to determine optimal weights for each model.
3. **Robust Preprocessing**: Comprehensive pipeline involving face extraction, resizing, and extensive data augmentation (Color Jitter, Gaussian Noise, etc.) to enhance generalization.

## Dataset

The models are trained and evaluated on a multi-class dataset categorized into:

* `real`: Authentic human face presentations.
* `3dmask`: Physical 3D mask attacks.
* `deepfake`: Digitally manipulated face swaps or generations.
* `print`: High-resolution printed photo attacks.
* `replay`: Video replay attacks on digital screens.


* **In-house**: Print, Replay, Real.

* **HKBU-MARS V1+**: 3D Mask.

* **Celeb-DF v2**: Deepfakes.

**Comparison Datasets**: NUAA, iBeta Level 1, CASIA-SURF CeFA, Faceforensics++.

## Project Structure

* **Scenario 1 (`liveness_detection-s1.ipynb`)**: Baseline model training and initial ensemble testing.
* **Scenario 2 (`liveness_detection-s2.ipynb`)**: Evaluation and fine-tuning across different dataset splits or parameters.
* **Augmentation & Evaluation (`liveness_detection-s1-aug-eval.ipynb`)**: Detailed performance analysis, grid search for optimal ensemble weights, and data augmentation experiments.

## Performance & Metrics

The project uses industry-standard metrics for biometric security:

* **APCER** (Attack Presentation Classification Error Rate): Rate of "fakes" classified as "real."
* **BPCER** (Bona Fide Presentation Classification Error Rate): Rate of "reals" classified as "fake."
* **ACER**: The average of APCER and BPCER (Lower is better).

## Experimental Results

The ensemble model significantly outperformed individual architectures across two experimental scenarios.

### Scenario 1 Performance (In-house, HKBU-MARS V1+, Celeb-DF V2)

| Model | Accuracy | BPCER | APCER | ACER |
| --- | --- | --- | --- | --- |
| **YOLOv11s-cls** | 0.9642 | 0.1067 | 0.0177 | 0.0622 |
| **EfficientNetV2-S** | 0.9332 | 0.2850 | 0.0102 | 0.1476 |
| **Weighted Averaging Ensemble (Proposed)** | **0.9715** | **0.1025** | **0.0094** | **0.0559** |


### Weight Optimization

Grid search identified the following optimal weights for the ensemble:

* **YOLOv11 Weight**: 0.8920 (Scenario 1) / 0.8630 (Scenario 2).
* **EfficientNetV2 Weight**: 0.1080 (Scenario 1) / 0.1370 (Scenario 2).



## Methodology

1. **Preprocessing**: Video frames are converted to images, faces are cropped to focus on main features, and images are resized to 224x224.
2. **Training**: Individual models are trained in parallel using the **AdamW** optimizer and **CrossEntropyLoss**.
3. **Ensemble**: A weighted sum of class probabilities is computed, with the final class determined by the maximum cumulative score.



## Computational Analysis

While the ensemble model provides superior accuracy, it involves a trade-off in speed.

* **YOLOv11 Inference**: ~0.37 - 0.44 ms.
* **EfficientNetV2 Inference**: ~1.71 - 1.72 ms.
* **Ensemble Inference**: ~2.07 ms.

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

## Citation

If you use this work, please cite the original research:

> 
> **Tsabita Bayu Kandi & Vera Suryani**, "Liveness Detection Utilizing Ensemble Deep Learning Method for Biometric Authentication," Telkom University, 2025.
