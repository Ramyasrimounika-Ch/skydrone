# Aerial GCP Pose Estimation 🚁

## 📌 Problem Statement
This project focuses on detecting Ground Control Points (GCPs) in high-resolution aerial images. The goal is to:
1. Predict the exact pixel coordinates (x, y) of the GCP marker center
2. Classify the marker shape into one of: Cross, Square, L-Shape

---

## 📂 Dataset Overview
- Train images: High-resolution aerial images (~4096×3000)
- Test images: Unlabeled images in nested folder structure
- Labels provided in JSON format with:
  - Center coordinates (x, y)
  - Shape class

### Dataset Statistics
- Total images: ~1000
- Valid annotations: 996
- Missing labels: 4

### Class Distribution
- L-Shape: 491
- Square: 328
- Cross: 177

---

## 🔍 Exploratory Data Analysis (EDA)
Key insights from dataset:
- Images have fixed width (4096px) but varying height
- GCP markers are very small (~20–25 pixels)
- Significant class imbalance exists (Cross is underrepresented)
- Annotation consistency is good with minor missing entries

---

## 🛠️ Data Preprocessing
Since only center-point annotations were provided:
- Converted keypoints into synthetic bounding boxes (50×50 px)
- Mapped shapes to class IDs:
  - Cross → 0
  - Square → 1
  - L-Shape → 2
- Created YOLO format dataset
- Performed stratified train/validation split (80/20)

---

## 🧠 Model Architecture
- Model: YOLOv8s (Ultralytics)
- Input size: 1280×1280
- Loss functions:
  - Bounding box regression loss
  - Classification loss
- Early stopping applied (patience = 20)

---

## ⚙️ Training Details
- Epochs: 45 (best at epoch 25)
- Batch size: 8
- Optimizer: SGD/Adam (default YOLO optimizer)
- Device: Tesla T4 GPU

---

## 📊 Evaluation Results

### Validation Performance
- Precision: **0.867**
- Recall: **0.890**
- mAP@50: **0.876**
- mAP@50-95: **0.441**

### Class-wise Performance
- Cross: 0.738 mAP50
- Square: 0.949 mAP50
- L-Shape: 0.941 mAP50

---

## 🚀 Inference Pipeline
1. Load trained YOLO model (`best.pt`)
2. Run inference on test images
3. Select highest-confidence detection per image
4. Convert bounding box → center coordinates:
   - x = (x1 + x2) / 2
   - y = (y1 + y2) / 2
5. Map predicted class to shape label
6. Save results in `predictions.json`

---

## 📦 Output Format
```json
{
  "project/survey/gcp/image.JPG": {
    "mark": {
      "x": 1234.56,
      "y": 789.12
    },
    "verified_shape": "Cross"
  }
}
