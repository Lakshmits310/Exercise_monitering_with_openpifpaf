# 🏋️‍♂️ Exercise Monitoring with OpenPifPaf

## Smart Exercise Form Analysis & Repetition Counting System
A hybrid AI system using **OpenPifPaf** for keypoint detection, combined with **LSTM-based classification** and **rule-based logic** to monitor and analyze workout form. The system detects **posture mistakes**, **counts repetitions**, and provides **visual + textual feedback** for five popular exercises.

## ✅ Supported Exercises
* Squats
* Pushups
* Pullups
* Leg Raises
* Planks *(form analysis only, no rep counting)*

## 🚀 Features
* **Pose Estimation:** Extracts 17 keypoints per frame using OpenPifPaf (COCO format).
* **Form Analysis (Mistake Detection):**
  * LSTM detects temporal anomalies.
  * Rule-based logic validates joint angles and movement patterns.
* **Repetition Counting:** Accurate peak detection on key movement trajectories (e.g., hips for squats, shoulders for pushups).
* **Feedback Generation:** Annotated output video with rep count and mistake timestamps.
* **Multi-Angle Support:** Works with side-view and front-view exercise videos.

## 🛠️ Implementation Overview
1. **Keypoint Extraction (`extract_keypoints.py`)**
   * Runs OpenPifPaf inference on each video frame.
   * Outputs normalized keypoint arrays (`.npy`) for consistency across users and resolutions.

2. **LSTM Training (`train_lstm.py`)**
   * Trains on good-form sequences to learn temporal movement patterns.
   * Outputs `.pth` model weights for later inference.

3. **Mistake Detection (`mistake_detection.py`)**
   * Uses LSTM anomaly scores and rule-based validation for joint angle violations.
   * Flags incorrect posture and generates feedback.

4. **Repetition Counting (`rep_count.py`)**
   * Tracks joint position changes (vertical/horizontal trajectories).
   * Uses peak detection with range and timing thresholds.

5. **Web Application (`app.py`)**
   * Flask-based UI for uploading videos.
   * Returns annotated video + detailed report.

## 📥 Input
* Video file of supported exercises
* Front or side view preferred
* Recommended resolution: **720p or higher**

## 📤 Output
* Annotated video with:
  * Skeleton overlay
  * Rep count overlay
  * Mistake indicators with timestamps

* Console/log feedback (example):
```
Processed 300 frames (10.0 FPS)
Detected 12 pushup repetitions
Form errors detected:
- Frame 45: Elbows flaring > 45°
- Frame 128: Incomplete range of motion
- Frame 215: Hip sagging
```

## 📊 Results Summary
* **Pose Estimation:** 15–20 FPS on mid-range GPU with OpenPifPaf
* **Repetition Counting Accuracy:** \~95% on validated test videos
* **Mistake Detection:** Hybrid (LSTM + rules) reduced false positives by \~30% compared to LSTM-only detection
* **Scalability:** Modular codebase supports adding new exercises with minimal changes

## ⚙️ Setup & Usage
### 1. Clone Repository
```bash
git clone https://github.com/<your-username>/Exercise_monitoring_with_openpifpaf.git
cd Exercise_monitoring_with_openpifpaf
```

### 2. Install Requirements
```bash
pip install -r requirements.txt
```

### 3. Download Pretrained Models
```bash
# Replace with your hosted model path
wget https://example.com/path/to/models.zip
unzip models.zip -d models/
```

### 4. Run Web Application
```bash
python app.py
```

Upload your video → Receive annotated output & feedback.

## 🛠 Technologies Used
* **Pose Estimation:** [OpenPifPaf](https://github.com/openpifpaf/openpifpaf)
* **Deep Learning:** PyTorch (LSTM)
* **Backend Framework:** Flask
* **Visualization & Processing:** OpenCV, Matplotlib, NumPy
* **Dataset:** [Workout Fitness Video – Kaggle](https://www.kaggle.com/datasets/hasyimabdillah/workoutfitness-video)
