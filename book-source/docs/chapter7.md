# Chapter 7: Hardware Requirements & Lab Setup

Building intelligent robots is a computationally intensive endeavor. Unlike pure software projects, Physical AI sits at the demanding intersection of three heavy workloads: the complex physics of simulation, the high-throughput data processing of visual perception, and the massive model inference of generative AI. This chapter details the hardware required to build, test, and deploy the humanoid robot systems discussed in this book.

We will cover three main categories of hardware:
1.  **The "Digital Twin" Workstation:** The high-performance PC where you will run your simulations.
2.  **The "Physical AI" Edge Kit:** The brain and senses of your physical robot.
3.  **The Robot Lab:** The physical robot hardware options, from budget-friendly to premium.

## 1. The "Digital Twin" Workstation

This is the most critical and significant investment. Your workstation is where you will run NVIDIA Isaac Sim, a powerful Omniverse application that requires "RTX" (Ray Tracing) capabilities for its advanced rendering and simulation features. A standard laptop, including MacBooks or non-RTX Windows machines, will not be sufficient for the tasks in this book.

-   **GPU (The Primary Bottleneck):** NVIDIA RTX 4070 Ti (12GB VRAM) or higher.
    -   **Why:** A powerful GPU with ample VRAM is non-negotiable. You need it to load the large 3D assets (in USD format) for the robot and its environment, and to simultaneously run the Vision-Language-Action (VLA) models that power the robot's intelligence. For the smoothest "Sim-to-Real" training experience, an RTX 3090 or 4090 (with 24GB VRAM) is ideal.

-   **CPU:** Intel Core i7 (13th Gen or newer) or AMD Ryzen 9.
    -   **Why:** While the GPU handles rendering and AI, the CPU is responsible for the demanding physics calculations (like Rigid Body Dynamics) in both Gazebo and Isaac Sim. A powerful CPU ensures your simulations run smoothly and accurately.

-   **RAM:** 64 GB of DDR5.
    -   **Why:** 32 GB is the absolute minimum, but you will likely experience crashes and slowdowns when rendering complex scenes or training large models. 64 GB provides a stable and robust development experience.

-   **Operating System:** Ubuntu 22.04 LTS.
    -   **Why:** While some NVIDIA tools run on Windows, the heart of the robotics community, ROS 2 (specifically the Humble and Iron distributions), is developed and supported natively on Linux. Using Ubuntu is mandatory for a friction-free development experience. Dual-booting or having a dedicated Linux machine is the recommended approach.

## 2. The "Physical AI" Edge Kit

A full humanoid robot is prohibitively expensive for most developers. However, you can learn and master Physical AI by building the robot's "nervous system" on a desk before deploying it to a physical chassis. This kit contains the essential components for the robot's brain and senses.

-   **The Brain: NVIDIA Jetson Orin Nano (8GB) or Orin NX (16GB)**
    -   **Role:** This is the industry-standard embedded AI computer. You will deploy your ROS 2 nodes here to test how they perform under the resource constraints of a real robot, as opposed to your powerful workstation.

-   **The Eyes: Intel RealSense D435i or D455**
    -   **Role:** This camera provides both color (RGB) and depth (distance) data, which is essential for VSLAM (Visual Simultaneous Localization and Mapping) and other perception algorithms. The 'i' variant includes a built-in IMU.

-   **The Inner Ear: Generic USB IMU (e.g., BNO055)**
    -   **Role:** An Inertial Measurement Unit is crucial for balance and estimating the robot's orientation. While some cameras and Jetson boards have a built-in IMU, using a separate module is a great way to learn about IMU calibration and data fusion.

-   **The Voice: USB Microphone/Speaker Array (e.g., ReSpeaker)**
    -   **Role:** This enables the conversational AI capabilities of your robot, allowing it to process voice commands using models like Whisper and generate spoken responses.

## 3. The Robot Lab Hardware

For the final "physical" stage of the capstone project, you need a robot body. Here are three tiers of options depending on your budget and goals.

### Option A: The "Proxy" Approach (Recommended for Budget)
You can learn 90% of the software principles by using a non-humanoid robot as a proxy. Quadruped (dog-like) robots or robotic arms are excellent, durable platforms for this.
-   **Robot Example:** Unitree Go2 Edu (~$1,800 - $3,000).
-   **Pros:** Extremely durable, fantastic ROS 2 support, and affordable enough to be used in a classroom setting.
-   **Cons:** It is not a biped, so you will not be able to test humanoid-specific locomotion.

### Option B: The "Miniature Humanoid" Approach
These are small, table-top humanoids that are excellent for learning kinematics and control.
-   **Robot Examples:** Unitree G1 (~$16k) or Robotis OP3 (~$12k).
-   **Budget Alternative:** Hiwonder TonyPi Pro (~$600).
-   **Warning:** The cheaper kits are often powered by a Raspberry Pi, which cannot run the NVIDIA Isaac ROS stack efficiently. You would use these for learning walking kinematics, but perform the AI tasks on your separate Jetson Edge Kit.

### Option C: The "Premium" Lab (For True Sim-to-Real)
If your goal is to fully deploy the capstone project to a physical, walking humanoid, you will need a premium platform.
-   **Robot Example:** Unitree G1 Humanoid.
-   **Why:** This is one of the few commercially available humanoids that can walk dynamically and has an SDK open enough for students and developers to inject their own ROS 2 controllers, enabling a true sim-to-real workflow.