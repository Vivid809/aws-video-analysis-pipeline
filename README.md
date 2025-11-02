# AWS Video Analysis Pipeline 🎥  
*A scalable, cloud-based video analysis system built on AWS and Google Vision API.*

---

## 📘 Overview
This project was developed for **CAB432: Cloud Computing** at QUT (Group 109) by **Mutahher Naseer** and **Bismillah Sultani**.  
The system bridges the gap between raw video data and meaningful analytics by automating video processing, frame extraction, and object detection using cloud-based architecture.

> ⚠️ **Note:**  
> This project was originally deployed on AWS using EC2 Auto Scaling Groups, S3, SQS, and Redis (ElastiCache).  
> The full architecture and code remain valid and deployable, but the live instances have since been **decommissioned** to reduce cost.  
> This repository now serves as a **technical showcase** and **visual portfolio** demonstrating scalable cloud architecture, automation, and AWS integration.

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

The architecture follows core principles of **statelessness, persistence, and scalability**:
- **Load Balancer:** Distributes incoming user requests across EC2 instances.
- **Elasticache (Redis):** Stores hash keys for fast lookup of previously processed videos.
- **S3 Bucket:** Stores raw uploads, extracted frames, and result metadata.
- **SQS Queue:** Triggers worker instances to process new uploads.
- **EC2 Auto Scaling Group:** Dynamically scales between 1–7 instances based on queue size.
- **CloudWatch:** Monitors queue depth and CPU utilization; triggers scaling alarms.
- **Google Vision API:** Labels objects and returns frame-level metadata.

## 📡 SQS + Auto Scaling (Network flow)
![SQS flow with ASG](./Network%20Diagram.png)

Each new upload to S3 triggers an SQS message.  
- CloudWatch monitors **ApproximateNumberOfMessages** in the queue.  
- When thresholds are breached, the **Auto Scaling Group** launches new EC2 instances.  
- As the queue drains, instances automatically scale down to reduce cost.

---

## ⚙️ Technical Components
**Frontend (client):**
- Built in React.js for intuitive uploads and progress tracking.
- Displays top object graphs and searchable timestamps.

**Backend (server):**
- Node.js with Express, AWS SDK, and Crypto-js (SHA-256 hashing).  
- Generates pre-signed S3 URLs and publishes SQS messages.

**Worker (SQS handlers):**
- Polls SQS queue → fetches video from S3 → extracts frames via ffmpeg → sends to Vision API → saves labeled output to S3.

**Testing utilities:**
- Synthetic video upload generator for stress testing scaling events.
- Python scaling script to trigger CloudWatch alarms.

---

## 🧠 Scaling & Performance
Two Auto Scaling Groups manage the pipeline:
1. **Server ASG** — handles upload traffic (t2.micro, min=1, max=3).  
2. **Worker ASG** — handles SQS processing load (t2.medium, min=1, max=7).  

Load generation and queue monitoring were validated using CloudWatch metrics.  
Scaling was successfully triggered by running a Python script that bulk-uploaded test videos, simulating heavy queue activity.

---

## 🧩 Services & Libraries
- **AWS SDK** – S3, SQS, EC2, CloudWatch, IAM integration  
- **Google Vision API** – object detection and labeling  
- **FFmpeg** – frame extraction  
- **Redis (Elasticache)** – hash key lookup for deduplication  
- **Crypto-js** – SHA-256 hashing  
- **React / Node.js** – front-end and server logic

---

## 🚀 How It Works (End-to-End)
1. User uploads an `.mp4` video via the React app.  
2. The video is stored in an **S3 bucket** and a message is sent to **SQS**.  
3. **CloudWatch alarm** detects new messages → triggers **ASG scale-out**.  
4. Worker instances fetch the message, process the video, and tag frames.  
5. Results (JSON metadata + labeled frames) are saved back to S3.  
6. The UI displays graphs and allows timestamp search for detected objects.  

---

## 🧠 Future Enhancements
- Integrate **Face Detection** for emotion and expression analysis.  
- Implement **facial recognition** or **object tracking** across frames.  
- Introduce **cost optimization** with spot instances and lifecycle hooks.  

---

## 👥 Authors
**Mutahher Naseer**  
**Bismillah Sultani**

## 📚 References
1. [AWS SQS with Auto Scaling Group (ASG) – Dilshan Fernando](https://enlear.academy/sqs-with-auto-scaling-group-asg-e2cd30100cad)  
2. [Sending and Receiving Messages in Amazon SQS (AWS Docs)](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/sqs-examples-send-receive-messages.html)  
3. [Cloud Vision API Documentation – Google](https://cloud.google.com/vision/docs)  
4. [Persistent Storage for Cloud Applications – Synopsys](https://www.synopsys.com/cloud/insights/persistent-storage.html)

