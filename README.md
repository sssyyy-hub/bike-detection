# Mobility Parking Etiquette (Fallen Bicycle Detection Robot)

**Autonomous Patrol Robot for Detecting Fallen Bicycles to Ensure Safety for the Visually Impaired and Pedestrians**

---

## 1. Project Overview
Disorderly parked or fallen bicycles on campus pose a significant safety threat to the visually impaired, who often rely on spatial memory for navigation. This project utilizes a **Rasbot (Raspberry Pi Robot)** to autonomously patrol the campus, detect **Fallen Bicycles** in real-time using AI, and immediately send location data and alerts to administrators via `ntfy.sh` for prompt action.

###  Key Features
* **Autonomous Patrol:** Navigates along designated black lines using a 4-channel line-tracking sensor.
* **Real-time Object Detection:** Distinguishes between `Fallen Bike`, `Upright Bike`, `Scooter`, and `Motorcycle` using a **YOLOv8 (TFLite)** model.
* **Instant Alert System:** Sends push notifications with Google Maps links to smartphones via **ntfy.sh** upon accident detection.
* **Control Dashboard:** A web-based interface for visual monitoring of incident locations in real-time.

---

## 2. Quick Start

###  Prerequisites (External Libraries)
To run this project, you need to install the following libraries on your Raspberry Pi:

```bash
pip install opencv-python tflite-runtime pynmea2 pyserial requests numpy

```
##  Execution Steps
### Run Dashboard locally:

```
python -m http.server 8000
```
Note: Access the UI via http://localhost:8000 in your browser.

### Run Detection System:

Open the main script final_code.py in Thonny IDE on your Raspberry Pi 5.
Click the 'Run' button to start the monitoring and patrol system.

## 3. Live Demo
-Control Dashboard: https://indigo-ingrid-61.tiiny.site

-Alert Channel (ntfy.sh): https://ntfy.sh/hanyang-rasbot-alert

-Demo Video (YouTube): https://youtu.be/DDVC02X5CDU

## 4. Dataset & Model
### Dataset Sources:
Combined Kaggle datasets ("Bike and Motorbike", "Vehicle Type Recognition") with self-collected low-angle footage from the Rasbot camera.

### Model Performance:
- Accuracy (mAP@0.5): 0.95 (Exceeded goal of 70%).
- Precision: ≥ 0.9.
- Latency: Alert delivery within 10 seconds.
### User Satisfaction: Scored 4.05 / 5.0 in a survey of 20 participants.

## 5. Troubleshooting (Key Challenges)
GPS Signal Issues: During outdoor testing, the GPS module struggled to fix coordinates due to weak signals and EMI from the Raspberry Pi 5.
Solution: Implemented fallback logic that sends a default Simulation Coordinate (37.557, 127.046) when live GPS is unavailable.

Spam Alerts: The AI detected the same fallen bike repeatedly, causing redundant notifications.
Solution: Set COOLDOWN_SEC to 15 seconds to prevent duplicate alerts.

Recognition Accuracy: Initial models failed due to the robot's low camera angle.
Solution: Collected custom data from the Rasbot's perspective and retrained the model for 300~500 epochs.

## 6. System Architecture
Hardware: Raspberry Pi 5 (Rasbot OS), Yahboom Rasbot Chassis, Pi Camera, GPS Module (UART).
Software Stack: Python 3, YOLOv8, TensorFlow Lite, ntfy.sh, Tiiny.host.

## 7. Credits
Team: [Group 9]

Tools: Ultralytics YOLOv8, TensorFlow Lite, VS Code, Thonny.
Data Sources: Kaggle users nqa112 and kaggleashwin.

## License
The datasets used in this project follow the Apache License 2.0 and CC0 Public Domain licenses.
