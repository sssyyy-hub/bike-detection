# Mobility Parking Etiquette (Fallen Bicycle Detection Robot)

Autonomous Patrol Robot for Detecting Fallen Bicycles to Ensure Safety for the Visually Impaired and Pedestrians

## 1. Project Overview
Bicycles parked disorderly or fallen over on campus pose a significant safety threat to the visually impaired and pedestrians. This project utilizes a Rasbot (Raspberry Pi Robot) to autonomously patrol the campus, detect Fallen Bicycles in real-time, and immediately send location data and alerts to administrators for prompt action.

### Key Features
* Autonomous Patrol (Line Tracking): The Rasbot moves along a designated line on the ground to patrol the campus.
* Real-time Object Detection: Distinguishes between `Fallen Bike`, `Upright Bike`, `Scooter`and `Motorcycle` in real-time using Raspberry Pi 5 and YOLOv8 (TFLite) model.
* Location Tracking & Alerts (GPS & Alert): Upon detecting an accident, it collects GPS coordinates and sends an audible alert and Google Maps link to the administrator via `ntfy.sh`.
*Control Dashboard: A web dashboard provides visual monitoring of accident locations (pins) in real-time.


## 2. System Architecture

### Hardware
* **Platform:** Raspberry Pi 5
* **Robot:** Rasbot (Yahboom Rasbot Chassis)
* **Sensors:**
  * Pi Camera (For object detection)
  * GPS Module (For location tracking, UART communication)
  * 4-Channel Line Tracking Sensor (For driving)

### Software & Tech Stack
* **Language:** Python 3
* **AI Model:**
  * **Training:** YOLOv8 (Ultralytics)
  * **Inference:** TensorFlow Lite (`tflite-runtime`)
  * **IDE:** Thonny (Raspberry Pi), VS Code (Dashboard)
* **Communication:**
  * `requests` library (HTTP communication)
  * `ntfy.sh` (Push Notification Service)
* **Web Hosting:** Tiiny.host (Dashboard)

---

## 3. Quick Start

### Prerequisites
* Raspberry Pi 5 (OS installation complete)
* Python 3 environment
* Install required packages:
```bash
pip install -r requirements.txt
# Key packages: opencv-python, tflite-runtime, pynmea2, requests, pyserial
```
##Usage
1. Preparation

Ensure the model file (final_Y.tflite) and the label file (final_labels.txt) are located on the Raspberry Pi Desktop.
Place the Rasbot on the black tracking line on the ground.

2. Execution

Open the main python file in Thonny IDE on the Raspberry Pi.
Click the Run button (Green arrow) to start the autonomous patrol.
The robot will automatically follow the line and detect objects.

3. Monitoring

When a fallen bike is detected, an alert will be sent to your smartphone via ntfy.sh.
Check the real-time location on the Control Dashboard: https://indigo-ingrid-61.tiiny.site


## 4. Model & Data
Dataset
Sources:
Kaggle Vehicle Type Recognition (Apache 2.0)
Kaggle Bike and Motorbike (CC0 Public Domain)
Self-collected data (Google Drive)

Classes: fallen bike, upright bike, scooter, motorcycle(Segmented to resolve initial misclassifications).
Data Refinement: Added self-collected low-quality and low-angle data to improve performance in the Rasbot camera environment.

Performance
mAP@0.5: Achieved approx. 0.95 (High detection accuracy)
Precision / Recall: Maintained above 0.9
Latency: Within 10 seconds (Real-time alert transmission)
User Satisfaction: 4.05 / 5.0 (Survey of 20 participants)

## 5. Troubleshooting (Challenges & Solutions)

During the development process, we faced several critical errors. Here is how we identified and solved them.

### 1. Rasbot OS Compatibility
* **Problem:** The Rasbot did not run even though the basic Raspberry Pi screen was shown. We initially thought the standard Raspberry Pi OS was sufficient, but it was incompatible with the Rasbot hardware system.
* **Solution:** We consulted the manual and discovered that the Rasbot requires a dedicated OS. We re-imaged the SD card with the **Rasbot-specific OS**, rebooted the system, and successfully established control.

### 2. VNC Display Issue (Headless Mode)
* **Problem:** To run the Rasbot wirelessly, we unplugged the HDMI cable. However, the Raspberry Pi stopped sending video signals, causing the VNC screen to turn gray and become unresponsive.
* **Solution:** We solved this by forcing the display signal. We accessed `raspi-config` and set a **fixed resolution** to trick the system into thinking a monitor was connected. This restored the VNC screen immediately without an HDMI cable.

### 3. Notification System Dilemma
* **Problem:** We initially tried sending data to an external server but faced constant `HTTP Timeout` errors. Switching to local storage prevented crashes but failed to provide real-time alerts to remote managers, missing the project's core goal of accessibility.
* **Solution:** We adopted a **"Dualized Logic"** approach. We integrated **ntfy.sh** to send immediate push notifications via the server while maintaining local logs. This ensured accessibility and real-time response capabilities.

### 4. Camera Angle Mismatch
* **Problem:** The model failed to detect objects correctly because the Rasbot's camera angle is much lower than the human eye-level perspective used in the initial dataset.
* **Solution:** We collected new training images taken specifically from a **low-angle perspective** to match the robot's view and retrained the model for **300 epochs**.

### 5. Low Resolution & Accuracy
* **Problem:** The low resolution of the Rasbot camera made it difficult to achieve high detection accuracy with the existing model.
* **Solution:** We recorded videos directly using the Rasbot camera and extracted frames from various angles to create a custom dataset. We added this data and retrained the model for **200 epochs**, significantly improving performance.

## 6. Demo & Links
Control Dashboard: https://indigo-ingrid-61.tiiny.site

Alert Channel (ntfy.sh): https://ntfy.sh/hanyang-rasbot-alert

Demo Videos (YouTube):



## License
The datasets used in this project follow the Apache 2.0 and CC0 Public Domain licenses.
