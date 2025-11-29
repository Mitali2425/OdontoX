# OdontoX — AI-Powered Dental Forensics Identification System

<p align="center">

<!-- Frontend logos -->
<img src="https://img.shields.io/badge/React-20232A?style=flat&logo=react&logoColor=61DAFB" />
<img src="https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white" />
<img src="https://img.shields.io/badge/TailwindCSS-38B2AC?style=flat&logo=tailwindcss&logoColor=white" />

<br/>

<!-- Backend logos -->
<img src="https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white" />
<img src="https://img.shields.io/badge/PostgreSQL-336791?style=flat&logo=postgresql&logoColor=white" />
<img src="https://img.shields.io/badge/FastAPI-009688?style=flat&logo=fastapi&logoColor=white" />

<br/>

<!-- ML Logos -->
<img src="https://img.shields.io/badge/Mask%20R--CNN-FF6F00?style=flat&logo=pytorch&logoColor=white" />
<img src="https://img.shields.io/badge/ResNet-EE4C2C?style=flat&logo=pytorch&logoColor=white" />
<img src="https://img.shields.io/badge/VGG-EE4C2C?style=flat&logo=pytorch&logoColor=white" />
<img src="https://img.shields.io/badge/MobileNet-0096D6?style=flat&logo=tensorflow&logoColor=white" />

<br/>

<!-- Infra -->
<img src="https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white" />
<img src="https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white" />

</p>

## Screenshots

<table>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/589c4d26-614b-44c2-a220-337804bb4871" width="100%"></td>
    <td><img src="https://github.com/user-attachments/assets/c8df4884-9000-4abe-9bff-f4a34d71eb0b" width="100%"></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/015cf9f1-56c8-415a-8611-2a49aac50084" width="100%"></td>
    <td><img src="https://github.com/user-attachments/assets/0b29cdab-abfc-439b-b7d9-73be2005c155" width="100%"></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/fa7f3396-aba6-4afa-a51b-e5d34b4e7a6c" width="100%"></td>
    <td><img src="https://github.com/user-attachments/assets/24d8bd73-e134-495c-b9bf-f1485609a8ca" width="100%"></td>
  </tr>
    <td><img src="https://github.com/user-attachments/assets/7a32a4ad-5403-4daf-b5bb-4b44c950c05a" width="100%"></td>
  <tr>
    
  </tr>
</table>

---


## Table of Contents

- [Overview](#overview)
- [Project Goals](#project-goals)
- [How the OdontoX Forensic Pipeline Works](#how-the-odontox-forensic-pipeline-works)
  - [1. Image Classification](#1-image-classification)
  - [2. Image Enhancement & Preprocessing](#2-image-enhancement--preprocessing)
  - [3. Segmentation & Masking](#3-segmentation--masking)
  - [4. Feature Extraction](#4-feature-extraction)
  - [5. AM–PM Comparison & Ranking](#5-am–pm-comparison--ranking)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)

---
## Overview

**OdontoX** is a full-stack AI-powered web application designed to support **Disaster Victim Identification (DVI)** using panoramic dental X-rays.

The system processes each uploaded X-ray through a structured AI pipeline involving:

- **MobileNet** for panoramic vs non-panoramic image classification  
- **UNet + VGG** for image enhancement and quality restoration  
- **Mask R-CNN** for tooth and jaw segmentation  
- **ResNet + VGG** for deep feature extraction and tooth-level embeddings  
- An automated **Top-3 AM–PM identification ranking** with visual overlays  

This pipeline improves the speed, objectivity, and reliability of forensic dental identification.

---

## Project Goals

- Provide a secure web portal for uploading and analyzing PM panoramic X-rays.
- Compare PM scans with a stored AM dental record database.
- Deliver clear **visual overlays**, **per-tooth similarity scores**, and **overall match suggestions**.
- Maintain case histories and generate verifiable forensic reports.
- Automatically rank the **Top 3** most likely AM candidates for each PM case.
- Reduce manual labor using automated segmentation, alignment, and feature analysis.

---

# How the OdontoX Forensic Pipeline Works

<img width="610" height="600" alt="Screenshot 2025-11-29 053405" src="https://github.com/user-attachments/assets/30ec8884-0dcf-4eae-8707-3afd93f32f2f" />

**1. Image Classification (Panoramic vs Non-Panoramic)**

- A lightweight CNN (MobileNet) classifies the uploaded X-ray.
- If the image is **non-panoramic**, processing stops and the user is notified.

---

**2. Image Enhancement & Preprocessing**

If blur, noise, or low contrast is detected, OdontoX applies:

- **UNet-based enhancement**
- **VGG-based contrast improvement**
- **Sharpness correction**
- **Intensity normalization**

This produces a consistent, high-quality input for segmentation.

---

**3. Segmentation & Masking of Panoramic X-rays**

- **Mask R-CNN (Detectron2)** segments:
  - Individual teeth  
  - Jaw structures  
- Outputs include:
  - Instance masks  
  - Bounding boxes  
  - Tooth indices  
  - Confidence scores  

---

**4. Feature Extraction**

Extracted AM/PM features include:

- Tooth count  
- Tooth shape & morphology  
- FDI numbering (quadrant + tooth index)  
- Tooth type (incisor, canine, premolar, molar)  
- Relative position & spacing  
- Geometric descriptors (contours, IoU, angles)
- Deep embedding vectors from **ResNet / VGG**

---

**5. AM–PM Comparison & Ranking System**

OdontoX uses a **hybrid two-stage matching pipeline** to identify the closest AM candidates for every PM case:

**Stage 1 — Global Image-Level Retrieval**
- Deep embeddings extracted using **ResNet/VGG** are compared using **cosine similarity**.
- This provides a fast, coarse global ranking of all AM records.
- Only the top candidates from this stage move to the next step.

**Stage 2 — Per-Tooth Structural Reranking (Hungarian Assignment)**
- For each shortlisted AM–PM pair, tooth-level features and geometric descriptors are matched.
- The system applies the **Hungarian algorithm** to compute the optimal assignment between PM teeth and AM teeth.
- Structural consistency is evaluated using:
  - Tooth morphology similarity  
  - Positional alignment  
  - Geometric overlap (IoU)  
  - Quadrant + FDI number coherence

**Final Output**
The combined score from Stage 1 + Stage 2 produces:

- A refined **Top-K ranked list** (default: Top 3)  
- Per-tooth matched-pair visualization  
- Structural alignment overlays  
- A final PM-to-AM identification score  


## Tech Stack
**Frontend**
- React  
- TypeScript  
- Tailwind CSS  
- react-dropzone  
- react-router  
- react-query / SWR  

**Backend**
- FastAPI / Python  
- PostgreSQL  
- Deep Learning Models (PyTorch / TensorFlow)  

---

## Prerequisites

Before installing or running OdontoX:

**System Requirements**
- Windows 10/11, Ubuntu 20.04+, or macOS  
- NVIDIA GPU with CUDA (optional but recommended)  
- RAM: 8 GB minimum (16 GB recommended)  
- Storage: 10 GB free  

**Software Requirements**
- Python 3.9+  
- Node.js 18+  
- Docker & Docker Compose (recommended)  
- PostgreSQL (if not using Docker)  
- Git  
- CUDA Toolkit (for GPU inference)

---

