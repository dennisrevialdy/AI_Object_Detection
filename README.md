# Road Object Detection in Fisheye Cameras (YOLOv9 – YOLOv12)

Undergraduate thesis project, Department of Information Science, **University of Taipei**.

This project benchmarks nine YOLO variants on the public **[FishEye8K](https://github.com/MoyoG/FishEye8K)** dataset to find the most effective and efficient model for fisheye-based traffic surveillance. It also tests **YOLOv9-SE**, a YOLOv9 variant with a Squeeze-and-Excitation (SE) attention block.

![Detection example](https://github.com/user-attachments/assets/2784f735-5235-45a2-a12f-b5559c02a7e5)

## Key findings

- **YOLO11x** has the highest accuracy (mAP@0.5 = 0.329, mAP@0.5:0.95 = 0.166).
- **YOLOv9-SE** has the highest F1-score (0.381), the best precision/recall balance, with mAP@0.5 comparable to the best models (0.320).
- **YOLO11l** (4.4 ms) and **YOLOv9c** (4.8 ms) are the fastest, which suits real-time use.
- **YOLOv12** scored lowest (mAP@0.5 of 0.276 for l, 0.282 for x). Its attention-centric design may not transfer well to fisheye distortion without domain-specific tuning.
- Training YOLO11x for longer (200 epochs) or fine-tuning with Mosaic gave only marginal gains.

## Results

All models: COCO-pretrained, 640×640 input, 100 epochs, batch size 16, SGD (lr 0.01, momentum 0.937, weight decay 0.0005). Trained on Google Colab Pro (NVIDIA A100 40GB). Evaluated on the 1,000-image FishEye8K test set.

| Model | Precision | Recall | mAP@0.5 | mAP@0.5:0.95 | F1 | Inference (ms) |
|---|---|---|---|---|---|---|
| YOLOv9-C | 0.486 | 0.303 | 0.314 | 0.154 | 0.373 | 4.8 |
| YOLOv9-E | 0.455 | 0.313 | 0.319 | 0.156 | 0.371 | 10.0 |
| **YOLOv9-SE** | 0.474 | 0.318 | 0.320 | 0.150 | **0.381** | 7.2 |
| YOLOv10-L | 0.451 | 0.289 | 0.300 | 0.144 | 0.352 | 5.3 |
| YOLOv10-X | 0.507 | 0.284 | 0.312 | 0.156 | 0.364 | 6.9 |
| YOLO11-L | 0.453 | 0.297 | 0.304 | 0.147 | 0.359 | **4.4** |
| **YOLO11-X** | 0.502 | 0.297 | **0.329** | **0.166** | 0.373 | 7.1 |
| YOLOv12-L | 0.443 | 0.267 | 0.276 | 0.132 | 0.333 | 7.8 |
| YOLOv12-X | 0.445 | 0.276 | 0.282 | 0.137 | 0.340 | 10.1 |

### Per-class results (YOLOv9-SE)

| Class | Precision | Recall | mAP@0.5 | F1 |
|---|---|---|---|---|
| Bus | 0.226 | 0.307 | 0.207 | 0.260 |
| Bike | 0.534 | 0.405 | 0.402 | 0.461 |
| Car | 0.765 | 0.447 | 0.537 | 0.564 |
| Pedestrian | 0.553 | 0.180 | 0.252 | 0.272 |
| Truck | 0.293 | 0.251 | 0.204 | 0.270 |

Cars are detected most reliably. Pedestrians have the lowest recall, likely because they appear small and distorted in fisheye images. Bus and Truck are limited by few training instances.

## YOLOv9-SE

YOLOv9-SE adds a Squeeze-and-Excitation block to YOLOv9 so the network can re-weight feature channels. The model config is `models/extra_model/yolov9-se.yaml` and the `SE` module is in `models/common.py`, both inside `yolose.zip`.

## Dataset

[FishEye8K](https://github.com/MoyoG/FishEye8K): 5,288 training images (about 112k boxes) and a 1,000-image test set, with 5 classes: Bus, Bike, Car, Pedestrian, Truck.

## Tech stack

Python, PyTorch, Ultralytics YOLO, YOLOv9 (official repo), Google Colab, OpenCV, NumPy, Matplotlib.

## Quick start

YOLOv10, YOLO11 and YOLO12 use the [Ultralytics](https://github.com/ultralytics/ultralytics) CLI:

```bash
pip install ultralytics
yolo detect train model=yolo11x.pt data=data.yaml epochs=100 imgsz=640 batch=16
yolo detect val   model=runs/detect/train/weights/best.pt data=data.yaml
```

`data.yaml` should point to your local FishEye8K copy with these classes:

```yaml
train: path/to/FishEye8K/train/images
val: path/to/FishEye8K/val/images
nc: 5
names: ['Bus', 'Bike', 'Car', 'Pedestrian', 'Truck']
```

For YOLOv9 / YOLOv9-SE, unzip `yolose.zip` and follow the original [YOLOv9](https://github.com/WongKinYiu/yolov9) training instructions.

## Repository contents

| File | Description |
|---|---|
| `FInal_Thesis_Dennis.pdf` | Full thesis report (26 pages) |
| `Thesis_Defense_Presentation_Dennis.pdf` | Defense slides |
| `Poster_Dennis.pdf` | Project poster |
| `yolose.zip` | YOLOv9 code with the SE modification |

## Acknowledgements and license

Built on [YOLOv9](https://github.com/WongKinYiu/yolov9) (GPL-3.0), [Ultralytics](https://github.com/ultralytics/ultralytics) and the [FishEye8K](https://github.com/MoyoG/FishEye8K) dataset. Please follow their licenses when reusing this work.

## Author

Dennis, University of Taipei, Department of Information Science.
