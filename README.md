# Surgical Robotics: Vision-Guided Autonomous Soft-Tissue Manipulation

A computer vision pipeline designed for autonomous surgical tool guidance during soft-tissue manipulation. It fine-tunes a YOLOv8n object detection model on a custom dataset of 1,247 annotated images, mapping 2D bounding boxes to 3D coordinates using Intel RealSense Depth API with OpenCV on an Intel NUC i7-1260P running an Ubuntu 22.04 LTS real-time kernel (28ms inference, 5ms depth mapping).

## Features
- Tool detection using custom-trained YOLOv8n (nano) model trained on 1,247 annotated frames.
- 3D spatial mapping by projecting 2D bounding boxes to 3D coordinate space using Intel RealSense Depth API and OpenCV.
- Real-time performance benchmarked at 28ms inference and 5ms depth mapping on Intel NUC i7-1260P.

## Tech Stack
- PyTorch
- YOLOv8
- OpenCV
- Intel RealSense Depth API
- ROS2 Humble
- Linux RT-Kernel

## Getting Started
To configure and run the project locally, clone the repository and execute the setup instructions:

```bash
git clone https://github.com/Raghuram-sekar/Surgical-Robotics-Tissue-Manipulation.git
cd Surgical-Robotics-Tissue-Manipulation

# Execute local setup commands:
# Ensure python dependencies are installed:
pip install ultralytics opencv-python pyrealsense2
```
