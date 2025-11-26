# Resistor Value Detection Web App

This README is meant to guide the AI agent responsible for building and maintaining the application. It outlines the architecture, workflow, and technical requirements for creating a full end-to-end system that detects resistors from an uploaded image and outputs an annotated result with resistance values.

---

## 🚀 Project Overview

This application allows users to upload an image containing electronic resistors. The backend processes the image, detects all resistors, reads their color bands, calculates their resistance values, and returns the image annotated with the detected information.

---

## 🧩 System Architecture

The system consists of the following components:

### **1. Frontend (Web UI)**

* Provides an image upload interface.
* Sends the uploaded image to the backend via API.
* Displays the processed, annotated image.
* Optional: shows numerical resistor values in a list.

### **2. Backend API (FastAPI)**

* Accepts uploaded images.
* Passes the image through the ML pipeline.
* Returns the final annotated image.

### **3. Machine Learning Pipeline**

The AI agent handles all recognition and processing steps:

#### **A. Object Detection**

* Detect resistors in the image using YOLOv8/YOLOv9.
* Output bounding boxes for each resistor.

#### **B. Color Band Extraction**

**(Updated: Includes Classical Image Processing Method)**

For each detected resistor:

* Crop bounding box region.
* Extract the colored bands using **classical image processing**, which includes:

    * Converting the crop to HSV color space.
    * Applying color-threshold masks for standard resistor colors.
    * Using contour detection to isolate vertical color stripes.
    * Sorting detected bands from left → right.
* Optionally: Use a CNN classifier if higher accuracy is needed.
  For each detected resistor:

- Crop bounding box region.
- Detect the colored bands using either:

    * Classical image processing (HSV segmentation), or
    * A band-wise CNN classifier.

#### **C. Resistance Calculation**

Use standard EIA color band rules:

* First band → digit 1
* Second band → digit 2
* Third band → multiplier
* Fourth band (optional) → tolerance

#### **D. Image Annotation**

* Draw bounding boxes around resistors.
* Display detected resistor values near each box.

---

## 🛠️ Tech Stack

**Frontend:** React.js (or equivalent)
**Backend:** FastAPI (Python)
**Model Execution:** PyTorch
**Computer Vision:** OpenCV
**ML Model:** YOLOv8/YOLOv9 for detection
**Deployment:** Docker + Cloud hosting

---

## 🔄 Processing Pipeline (Step‑By‑Step)

1. **User uploads image** through web UI.
2. **Backend receives image** and saves temporarily.
3. **YOLO model runs** to detect resistors.
4. For each resistor:

    * Crop image region.
    * Process color bands.
    * Determine resistance value.
5. **OpenCV annotates** the original image:

    * Draw box around each resistor.
    * Place resistance text above it.
6. **Backend returns final annotated image**.
7. **Frontend displays result** to the user.

---

## 📦 Folder Structure (Suggested)

```
project-root/
│
├── backend/
│   ├── main.py
│   ├── models/
│   │   └── yolo_model.pt
│   ├── utils/
│   │   ├── color_detection.py
│   │   ├── resistance_calc.py
│   │   └── annotate.py
│   ├── requirements.txt
│   └── Dockerfile
│
├── frontend/
│   ├── src/
│   └── package.json
│
└── README.md
```

---

## 📘 Requirements for AI Model Training

* Dataset of resistor images.
* Annotated bounding boxes (via LabelImg or Roboflow).
* Varied backgrounds and lighting.
* Augmentations (brightness, noise, rotation).
* Color band classifier dataset if using ML for band reading.

---

## 🧪 Testing Checklist

* Works on different resistor orientations.
* Handles 3‑band, 4‑band, and 5‑band resistors.
* Accepts images from phone cameras.
* Correct color interpretation in low light.
* Proper bounding box placement.

---

## 🧭 Future Improvements

* Add capacitor/inductor detection.
* Support SMD resistor code reading.
* Add batch image upload.
* Provide resistance tolerance and error range.

---

## 📄 License

MIT License (or customize based on project need).

---

This README equips the AI agent with the full plan to build, extend, and maintain the project end‑to‑end.
