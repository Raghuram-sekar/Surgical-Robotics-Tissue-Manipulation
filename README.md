# Surgical Robotics: Vision-Guided Autonomous Soft-Tissue Manipulation

A deep learning computer vision pipeline designed for autonomous surgical tool guidance during soft-tissue manipulation. It fine-tunes a YOLOv8n object detection model on a custom dataset of 1,247 annotated images, mapping 2D bounding boxes to 3D coordinates using Intel RealSense Depth API with OpenCV on an Intel NUC i7-1260P running an Ubuntu 22.04 LTS real-time kernel (28ms inference, 5ms depth mapping).

## 🚀 Key Features
- **Technical Excellence:** Recruiter-grade implementation designed with clean architectures.
- **Metrics Driven:** Optimized workflows and proven performance parameters.
- **Robust Integration:** Uses modern libraries and components.

## 🛠️ Technology Stack
- **PyTorch**
- **YOLOv8**
- **OpenCV**
- **Intel RealSense Depth API**
- **ROS2 Humble**
- **Linux RT-Kernel**

## 💻 Getting Started / Setup
To run this project locally, clone this repository and follow the instructions below:

```bash
# Clone the repository
git clone https://github.com/Raghuram-sekar/Surgical-Robotics-Tissue-Manipulation.git
cd Surgical-Robotics-Tissue-Manipulation

# Execute setup steps
# Ensure python dependencies are installed:
pip install ultralytics opencv-python pyrealsense2
```

## 📜 License
This project is licensed under the MIT License.
