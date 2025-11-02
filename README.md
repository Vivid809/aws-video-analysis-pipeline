# AWS Video Analysis Pipeline 🎥  
*A scalable, cloud-based video analysis system built on AWS and Google Vision API.*

---

## 📘 Overview
This project was developed for **CAB432: Cloud Computing** at QUT (Group 109) by **Mutahher Naseer** and **Bismillah Sultani**.  
The system bridges the gap between raw video data and meaningful analytics by automating video processing, frame extraction, and object detection using cloud-based architecture.

Users can:
- Upload `.mp4` videos to an S3 bucket via a **React** web interface.  
- Automatically trigger analysis jobs through **SQS** and **Auto Scaling Groups (ASG)**.  
- View detected objects, their frequency, and timestamps within a processed video.  

---

## 🧩 Key Features
- **Automated pipeline:** S3 → SQS → EC2 (Auto Scaling) → Redis → Google Vision API.  
- **SHA-256 hashing** via Redis for duplicate video detection (prevents reprocessing).  
- **Horizontal scaling:** Scales EC2 workers dynamically based on queue load.  
- **Data visualization:** Generates object frequency graphs and searchable timestamps.  
- **Fault-tolerant design:** Stateless processing with managed message queues.


## 🧭 Architecture
![System architecture](./Architecture%20Diagram.png)

## 📡 SQS + Auto Scaling (Network flow)
![SQS flow with ASG](./Network%20Diagram.png)

