# 🔍 Lost Person Detection

A computer vision project that detects faces from video footage and searches for potential matches using a lightweight face similarity algorithm.

> **Course Project:** Data Mining  
> **Objective:** Assist in identifying a lost person by extracting faces from videos and comparing them against a reference image.

---

# Table of Contents

- [Overview](#overview)
- [Project Workflow](#project-workflow)
- [Features](#features)
- [Project Structure](#project-structure)
- [Technologies Used](#technologies-used)
- [Installation](#installation)
- [Usage](#usage)
- [How the Detection Pipeline Works](#how-the-detection-pipeline-works)
- [How the Matching Pipeline Works](#how-the-matching-pipeline-works)
- [Sample Workflow](#sample-workflow)
- [Current Limitations](#current-limitations)
- [Future Improvements](#future-improvements)
- [Contributors](#contributors)

---

# Overview

Finding a missing person from surveillance footage is a difficult and time-consuming task.

This project automates part of that process by:

1. Processing a video stream.
2. Detecting faces appearing in each frame.
3. Saving detected faces.
4. Comparing those faces with a reference image.
5. Returning the most similar candidates.

The system consists of two independent modules:

- **Face Detection Module**
- **Face Matching Module**

---

# Project Workflow

```
                  Video Input
                       │
                       ▼
            Image Preprocessing
                       │
                       ▼
           Face Detection (OpenCV)
                       │
                       ▼
          Confidence Verification
                       │
                       ▼
            Save Cropped Face Images
                       │
                       ▼
             Reference Face Image
                       │
                       ▼
              Face Matching Engine
                       │
                       ▼
          Ranked Similar Face Results
```

---

# Features

## Face Detection

- Detects frontal faces
- Detects profile faces
- Eye verification
- Histogram Equalization
- CLAHE enhancement
- Gamma correction
- Bilateral filtering
- Confidence-based verification
- Automatic duplicate filtering
- Live webcam/video visualization
- Automatic face cropping
- Timestamped image saving

---

## Face Matching

- Reference image comparison
- Facial signature extraction
- Pattern similarity calculation
- Image attribute comparison
- Combined similarity scoring
- Match ranking
- Confidence estimation
- Visual comparison using Matplotlib

---

# Project Structure

```
Lost_Person_Detection/
│
├── extract_faces.py
├── facematching.py
├── README.md
│
├── haarcascade_frontalface_default.xml
├── haarcascade_profileface.xml
├── haarcascade_eye.xml
│
├── faces/
│     ├── face_001.jpg
│     ├── face_002.jpg
│     └── ...
│
└── ClassGumbal.mp4
```

---

# Technologies Used

| Technology | Purpose |
|------------|---------|
| Python | Programming Language |
| OpenCV | Face Detection |
| NumPy | Numerical Operations |
| Matplotlib | Visualization |
| Tabulate | Console Tables |
| Haar Cascade Classifiers | Face & Eye Detection |

---

# Installation

Clone the repository

```bash
git clone https://github.com/VishwaKrithik/Lost_Person_Detection.git
```

Move into the repository

```bash
cd Lost_Person_Detection
```

Install dependencies

```bash
pip install opencv-python numpy matplotlib tabulate
```

---

# Usage

## Step 1: Extract Faces

Place your video in the project directory.

Edit the `main()` function inside `extract_faces.py`:

```python
process_video(
    video_source="ClassGumbal.mp4",
    save_faces=True,
    display_video=True,
    confidence_threshold=0.75,
    save_interval=30
)
```

Run

```bash
python extract_faces.py
```

Detected faces will automatically be saved inside

```
faces/
```

---

## Step 2: Find Similar Faces

Provide a reference image.

Run

```bash
python facematching.py reference.jpg --dir faces
```

Example

```bash
python facematching.py person.jpg --dir faces
```

---

## Optional Parameters

Show only top matches

```bash
python facematching.py person.jpg --top 5
```

Change similarity threshold

```bash
python facematching.py person.jpg --threshold 0.75
```

Disable image display

```bash
python facematching.py person.jpg --no-display
```

---

# How the Detection Pipeline Works

The detection module performs several preprocessing operations before detecting faces.

## 1. Image Preprocessing

Each frame undergoes:

- Grayscale conversion
- Histogram Equalization
- Gamma Correction
- Bilateral Filtering
- CLAHE enhancement

These improve contrast while reducing image noise.

---

## 2. Face Detection

The project uses OpenCV Haar Cascades for

- Frontal face detection
- Profile face detection

Both results are combined to maximize detection coverage.

---

## 3. Confidence Verification

Each detected face is assigned a confidence score using:

- Face size
- Aspect ratio
- Eye detection
- Eye positioning
- Skin color consistency

Only faces above the confidence threshold are retained.

---

## 4. Duplicate Removal

The system compares

- Face position
- Bounding box size

to avoid repeatedly saving the same face across multiple frames.

---

## 5. Face Saving

Accepted detections are saved as

```
video_frame30_face_score0.92_20260723_121030.jpg
```

inside the `faces/` directory.

---

# How the Matching Pipeline Works

The matching algorithm compares a reference face against all extracted faces.

It computes three independent similarity metrics.

---

## 1. Facial Signature Similarity

A lightweight facial signature is generated from image data and compared using partial hash matching.

---

## 2. Pattern Similarity

The algorithm compares

- byte distributions
- texture estimates
- transition statistics

to estimate similarity between facial regions.

---

## 3. Image Attribute Similarity

The following attributes are compared:

- Image dimensions
- Aspect ratio
- Estimated skin tone distribution
- File characteristics

---

## Final Similarity Score

The final score is computed as

```
Similarity =
0.30 × Signature Similarity
+
0.45 × Pattern Similarity
+
0.25 × Attribute Similarity
```

Images exceeding the similarity threshold are returned as matches.

---

# Sample Workflow

```
Video
   │
   ▼
Detect Faces
   │
   ▼
Save Face Images
   │
   ▼
Reference Image
   │
   ▼
Compare Against Saved Faces
   │
   ▼
Rank Matches
   │
   ▼
Display Results
```

---

# Current Limitations

This project is intended as an academic proof-of-concept.

Current limitations include:

- Uses classical Haar Cascade detection.
- Does not use deep learning face recognition.
- Sensitive to lighting conditions.
- Sensitive to face orientation.
- No face embeddings.
- No database integration.
- No real-time surveillance pipeline.
- Limited robustness for large datasets.

---

# Future Improvements

Potential enhancements include:

- FaceNet embeddings
- ArcFace embeddings
- InsightFace integration
- Dlib face recognition
- YOLO face detection
- FAISS vector database
- Real-time CCTV processing
- Flask/FastAPI web application
- Mobile application
- Cloud deployment
- GPU acceleration
- Face tracking across video frames
- Automatic report generation

---

# Acknowledgements

This project was developed as part of a **Data Mining** course to explore practical applications of computer vision in assisting lost person identification.

Special thanks to the **OpenCV** community for providing the Haar Cascade classifiers used in this project.

---

# Contributors

**Srihari Kothandapani**

Artificial Intelligence & Data Science  
Shiv Nadar University Chennai

GitHub: https://github.com/VishwaKrithik

---

# Disclaimer

This project is intended **solely for educational and research purposes**.

The face matching component uses handcrafted similarity measures and **should not be considered a production-grade biometric identification system**. For real-world deployments, modern deep learning models such as FaceNet, ArcFace, or InsightFace are recommended for improved accuracy and robustness.

---
