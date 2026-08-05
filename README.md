# Smartphone-Based AI System for Automated Detection of Pinworm Eggs in Microscopy Images
This repository contains the code accompanying the manuscript "Smartphone-Based artificial intelligence system for automated detection of pinworm eggs in microscopy images" (Muenjak J, Pongpanitanont P, Khampeeramao N, Thanchomnang T, Laymanivong S, Maleewong W, Janwan P).

The system detects and localizes Enterobius vermicularis (pinworm) eggs in smartphone-captured microscopic images obtained via the Scotch tape technique, using a two-phase pipeline: (1) image classification and (2) patch-based multi-object detection.

Overview
The pipeline follows a four-stage process:
  1. Data acquisition — Microscopic images (4,080 × 2,296 px) captured at 400× magnification under Olympus CX31 microscope using a smartphone.
  2. Data preprocessing — Cropping to 500 × 500 px patches, train/test split (80:20), and data augmentation, expanding the training set from 2,264 to 54,336 images.
  3. Model training and selection — Four CNN architectures (Custom CNN, ResNet-50, MobileNet, DenseNet-121) trained and compared for binary classification (pinworm egg vs. artifact), evaluated with accuracy, precision, recall, F1-score, and AUC (Area under the curve of ROC).
  4. Object detection — The best-performing lightweight model (MobileNet) integrated into a sliding-window patch search over full-resolution images, followed by Non-Maximum Suppression (NMS) and bounding-box merging to enable multi-object detection.

## Repository Structure

> Update the tree below to match the actual folders/files in this repository.

```
.
├── data_preprocessing/     # Image cropping, dataset splitting, augmentation scripts
├── classification/         # Training/evaluation scripts for Custom CNN, ResNet-50, MobileNet, DenseNet-121
├── object_detection/       # Sliding-window patch search, NMS, bounding-box merging
├── evaluation/             # Metric computation (accuracy, precision, recall, F1, AUC, mAP, IoU)
├── models/                 # Saved model weights (or download instructions, if hosted externally)
├── requirements.txt        # Python dependencies
└── README.md
```

## Requirements

- Python ≥ 3.x
- TensorFlow / Keras (or PyTorch — specify which framework was used)
- OpenCV, NumPy, scikit-learn, Matplotlib
- Google Colaboratory or an equivalent GPU environment (models were trained on an NVIDIA Tesla T4)

Install dependencies:

```bash
pip install -r requirements.txt
```

## Usage

### 1. Classification

Train and evaluate a classification model (e.g., MobileNet):

```bash
python classification/train.py --model mobilenet --epochs 100
```

### 2. Object Detection

Run multi-object detection on a full-resolution test image using the trained MobileNet backbone:

```bash
python object_detection/detect.py --image path/to/image.jpg --model models/mobilenet_best.h5 --confidence 0.95
```

The detector slides a 500 × 500 px window (step size 50 px) across the image, classifies each patch, and applies NMS and box-merging to produce final bounding boxes.

## Data

The image dataset used to train and evaluate the models is available at:
- Zenodo: https://doi.org/10.5281/zenodo.21679591

## Results Summary

| Model | Accuracy | Precision | Recall | F1-score | AUC |
|---|---|---|---|---|---|
| Custom CNN | 0.98 | 0.98 | 0.98 | 0.98 | 0.98 |
| ResNet-50 | 0.99 | 0.99 | 0.99 | 0.99 | 1.00 |
| MobileNet | 0.99 | 0.99 | 0.99 | 0.99 | 0.99 |
| DenseNet-121 | 0.99 | 0.99 | 0.99 | 0.99 | 0.99 |

Object detection performance (n = 247 test images): mAP@0.50 = 91.07%, mAP@0.50:0.95 = 0.27, mean IoU = 0.59.

## Citation

If you use this code, please cite:

> Muenjak J, Pongpanitanont P, Khampeeramao N, Thanchomnang T, Laymanivong S, Maleewong W, Janwan P. Smartphone-Based artificial intelligence system for automated detection of pinworm eggs in microscopy images. 

## Ethics Statement

This study was approved by the Ethics Committee in Human Research, Walailak University (protocol code WUEC-23-051-01, approved 20 February 2023), in accordance with the Declaration of Helsinki.

## Contact

Corresponding author: Penchom Janwan — penchom.ja@wu.ac.th
Department of Medical Technology, School of Allied Health Sciences, Walailak University, Nakhon Si Thammarat 80160, Thailand


