# 🤖 Surgical Robotics: Vision-Guided Autonomous Soft-Tissue Manipulation
![PyTorch](https://img.shields.io/badge/PyTorch-%23EE4C2C.svg?style=for-the-badge&logo=PyTorch&logoColor=white) ![OpenCV](https://img.shields.io/badge/opencv-%23white.svg?style=for-the-badge&logo=opencv&logoColor=white) ![ROS2](https://img.shields.io/badge/ROS2-Humble-blue?style=for-the-badge) ![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black) ![License](https://img.shields.io/badge/License-MIT-green.svg)

## 📋 Table of Contents
- [Project Overview](#🎯-project-overview)
- [What This Project Does](#🚀-what-this-project-does)
- [Key Innovation](#🔬-key-innovation)
- [Performance Highlights](#📊-performance-highlights)
- [Architecture](#🏗️-architecture)
- [Methodology & Technical Details](#⚙️-methodology--technical-details)
- [Project Structure](#📂-project-structure)
- [Tech Stack](#🧱-tech-stack)
- [Quick Start](#💻-quick-start)

---

## 🎯 Project Overview
A computer vision pipeline designed for autonomous surgical tool guidance during soft-tissue manipulation. It fine-tunes a YOLOv8n object detection model on a custom dataset of 1,247 annotated images, mapping 2D bounding boxes to 3D coordinates using Intel RealSense Depth API with OpenCV on an Intel NUC i7-1260P running an Ubuntu 22.04 LTS real-time kernel.

---

## 🚀 What This Project Does
* **The Challenge:** Surgical soft-tissue manipulation requires real-time 3D coordinate feedback with high precision, which traditional force sensors struggle to track due to sterilizability constraints and mechanical latency.
* **Our Solution:** An autonomous vision pipeline combining deep learning detection with structured-light depth sensing, outputting coordinates to Doosan collaborative robots.

---

## 🔬 Key Innovation
| Feature | Traditional Approach ❌ | Our Vision Solution ✅ | Benefit |
|---------|------------------------|------------------------|---------|
| **Tracking** | Mechanical force sensors requiring contact | **YOLOv8 + Intel RealSense D435 depth projection** | Non-contact, high-resolution 3D coordinate tracking |
| **Planning** | Static waypoint trajectories | **Curved-path topological planning** | Follows tissue contours dynamically |
| **Response** | Delayed software interrupts | **Real-time Ubuntu RT-kernel execution** | Emergency response under 50ms (SIL-2) |

---

## 📊 Performance Highlights
- ✅ **28ms YOLOv8n inference** and **5ms depth mapping** on Intel NUC.
- ✅ **Homogeneous transformation chain** with reprojection error **< 1.0 mm**.
- ✅ **Tested on tissue-analog fruits** spanning Young's moduli from 0.5 to 5.0 MPa.

---

## 🏗️ Architecture
```mermaid
graph TD
    RGBD[Intel RealSense RGB-D Stream] -->|RGB Frame| YOLO[YOLOv8n Tool Detection]
    RGBD -->|Depth Frame| Projection[Depth Mapping & Projection]
    YOLO -->|2D Bounding Box| Projection
    Projection -->|3D Coordinates| Trajectory[PoE Trajectory Planning]
    Trajectory -->|Joint Angles| Robot[Doosan Collaborative Robot]
```

---

## ⚙️ Methodology & Technical Details
### Object Detection and Bounding Box Extraction
We fine-tuned the YOLOv8n object detection network on a custom dataset of 1,247 annotated surgical tool-tip frames. The model predicts a bounding box \([u_{\min}, v_{\min}, u_{\max}, v_{\max}]\) corresponding to the tool-tip location in the 2D camera frame.

### 3D Coordinate Mapping & Intrinsic Projection
Using the pinhole camera model, we map the center pixel \((u_c, v_c)\) of the bounding box to a 3D coordinate in the camera frame. The Intel RealSense D435 camera provides the focal lengths \((f_x, f_y)\) and principal point coordinates \((c_x, c_y)\). Given the depth value \(d\) from the registered depth map, the 3D position \(\mathbf{P}_c = [X_c, Y_c, Z_c]^T\) is computed as:
$$Z_c = d$$
$$X_c = \frac{(u_c - c_x) \cdot Z_c}{f_x}$$
$$Y_c = \frac{(v_c - c_y) \cdot Z_c}{f_y}$$

### Robot Frame Calibration (SE(3) Transformation)
We calibrate a homogeneous transformation matrix \(\mathbf{T}_{	ext{cam}}^{	ext{robot}} \in 	ext{SE}(3)\) mapping the camera coordinate system to the Doosan collaborative robot's base frame:
$$\mathbf{P}_r = \mathbf{R}_{	ext{cam}}^{	ext{robot}} \mathbf{P}_c + \mathbf{t}_{	ext{cam}}^{	ext{robot}}$$
Using a calibration checkerboard grid, we minimize the reprojection error to obtain \(\mathbf{R}\) and \(\mathbf{t}\) with a mean error of **0.87 mm**.

---

## 📂 Project Structure
```
surgical_robotics/
├── yolov8_training.ipynb    # YOLOv8 model training notebook
├── doosan-robot2/           # ROS2 package for robot kinematics
│   ├── src/                 # MoveIt trajectory execution nodes
│   └── launch/              # ROS2 camera-robot launch scripts
└── surgical_robotics_ieee_paper.tex # Academic paper drafts
```

---

## 🧱 Tech Stack
- PyTorch & YOLOv8 computer vision models
- Intel RealSense Depth API (D435/D455 structured-light cameras)
- OpenCV for frame processing and pixel projection matrices
- ROS2 Humble and MoveIt 2 robot control stacks

---

## 💻 Quick Start
To configure and run the project locally, clone the repository and execute the setup instructions:

```bash
git clone https://github.com/Raghuram-sekar/Surgical-Robotics-Tissue-Manipulation.git
cd Surgical-Robotics-Tissue-Manipulation

# Execute local setup commands:
pip install ultralytics opencv-python pyrealsense2
```
