# AI_Object_Detection
# 🎯 AI-Based Object Detection Model

This is my undergraduate thesis project at the **University of Taipei**.  
I developed an **object detection model using YOLO** on a custom fisheye road dataset.

## ⚙️ Tech Stack
- Python, PyTorch, YOLO
- Google Colab
- OpenCV, NumPy, Matplotlib

## 📊 Results
- Large models (YOLOv11x, YOLOv12x) → higher accuracy
- Small models (YOLOv9n, YOLOv11n) → faster speed for real-time
- Best choice depends on accuracy vs. speed trade-off

## 🚀 Example Command
```bash
yolo task=detect mode=train model=yolov11x.pt data=data.yaml epochs=100 imgsz=640
