# Fall Detection System using GRU on Multi-Threaded Server

This project implements a **real-time fall detection system** for elderly care scenarios using **smartphone-based accelerometer and barometer data**, processed on a **multi-threaded Flask server** and classified with a **GRU-based recurrent neural network**.

---

## Overview

- **Data Source:** Smartphone sensors (accelerometer, barometer, GPS, battery)
- **Sampling Rate:** ~100 Hz
- **Model:** Gated Recurrent Unit (GRU)
- **Inference:** Server-side, real-time
- **Deployment:** Dockerized Flask + Flask-SocketIO server
- **Goal:** Reliable fall detection with low false negatives

---

## System Architecture

1. **Sensor Data Collection**
   - Smartphone collects 3-axis accelerometer data + barometer
   - Data streamed as JSON via HTTP `/stream` endpoint

2. **Server-Side Processing**
   - Multi-threaded Flask server handles multiple clients
   - Sliding window segmentation (7-second windows, 57% overlap)
   - Data stored temporarily as CSV for inference

3. **Machine Learning**
   - Acceleration magnitude computed from x, y, z
   - Butterworth low-pass filtering applied
   - GRU-based binary classifier (Fall / Non-Fall)

4. **Verification Logic**
   - Post-fall inactivity check (standard deviation)
   - Altitude change verification to avoid false positives (e.g. phone drop)

5. **Real-Time Updates**
   - Predictions and user status broadcast via WebSocket
   - Dashboard displays live user state and fall alerts

---

## Model Details

- **Input:** 700-length magnitude time series (~7 seconds)
- **Architecture:** GRU layers + Fully Connected layers
- **Loss:** Binary Cross Entropy
- **Class Ratio:** 1:3 (Fall : Non-Fall)
- **Test Accuracy:** 1.0 on smartphone-based dataset

---

## Latency & Performance

- **Detection Delay:** ~3–9 seconds (sliding window confirmation)
- **Scalability:**  
  - CPU suitable for small-scale deployments  
  - GPU recommended for monitoring 100+ users concurrently

---
