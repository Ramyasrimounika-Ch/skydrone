# Aerial GCP Pose Estimation

## Problem Statement

Predict:
- GCP center coordinates (x,y)
- Marker shape (Cross, Square, L-Shape)

## Dataset Analysis

Total Images: 1000

Valid Labels: 996

Missing Labels: 4

### Class Distribution

- L-Shape: 491
- Square: 328
- Cross: 177

### Image Resolutions

- 4096 x 3068
- 4096 x 2730

### Marker Analysis

Markers occupy approximately 20-25 pixels.

## Preprocessing

Since only center coordinates were provided, synthetic 50x50 bounding boxes were generated around each marker.

## Model

YOLOv8s

Image Size: 1280

Early Stopping at Epoch 45

Best Epoch: 25

## Validation Results

Precision: 0.867

Recall: 0.890

mAP50: 0.876

mAP50-95: 0.441

## Inference

Highest confidence detection was selected for each image.

For images with no detections, a fallback prediction was used.

## Model Weights

https://drive.google.com/file/d/1Gl8og_6c5muVQkQLJMt2HKTALI7uZW96/view?usp=sharing

## How to Run

1. Open notebook
2. Mount Google Drive
3. Run all cells
4. Generate predictions.json
