# 🚦 Smart Traffic Management System
> An AI-powered traffic management platform that dynamically optimizes signal timing based on real-time traffic density.

---

## 📌 Overview

The **Smart Traffic Management System** is a graduation project that addresses urban traffic congestion by combining computer vision, machine learning, and a real-time backend worker service. The system monitors live traffic feeds, analyzes vehicle density using AI, and automatically adjusts traffic signal durations to reduce waiting times and improve overall traffic flow.

---

## 🧠 How It Works

```
CCTV / Camera Feed
        ↓
  AI Module (Python)
  – Vehicle detection & counting
  – Traffic density estimation
        ↓
  Backend Worker (C# / TrafficWorker)
  – Processes AI output
  – Computes optimal signal timings
  – Sends control commands
        ↓
  Traffic Signals
  – Dynamic green/red durations applied
        ↓
  Web Dashboard (HTML)
  – Live monitoring & visualization
```

---

## ✨ Features

- **Real-time vehicle detection** using computer vision (Python/AI module)
- **Dynamic signal control** — green light durations adapt to current traffic density
- **Backend worker service** (C#) for processing and orchestrating signal updates
- **Web-based dashboard** for live traffic monitoring and system status
- **Scalable architecture** — designed to support multiple intersections

---

## 🗂️ Project Structure

```
graduation_project/
├── ai/                        # Python AI module
│   ├── detection/             # Vehicle detection & counting models
│   └── density/               # Traffic density analysis
│
├── backend/
│   └── TrafficWorker/         # C# background service
│       ├── Workers/           # Signal scheduling logic
│       ├── Models/            # Data models
│       └── Services/          # Core services & API communication
│
└── .gitignore
```

---

## 🛠️ Tech Stack

| Layer       | Technology           | Purpose                              |
|-------------|----------------------|--------------------------------------|
| AI / ML     | Python               | Vehicle detection, density analysis  |
| Backend     | C# (.NET)            | Traffic worker service, signal logic |
| Frontend    | HTML / CSS / JS      | Monitoring dashboard                 |

---

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- .NET 6+ SDK
- A webcam or RTSP camera stream (or test video files)

### 1. Clone the Repository

```bash
git clone https://github.com/Anasfassih8/graduation_project.git
cd graduation_project
```

### 2. Set Up the AI Module

```bash
cd ai
pip install -r requirements.txt
python main.py
```

### 3. Run the Backend Worker

```bash
cd backend/TrafficWorker
dotnet restore
dotnet run
```

### 4. Open the Dashboard

Open the HTML frontend in your browser to view the live traffic monitoring interface.

---

## 📊 Language Breakdown

![C#](https://img.shields.io/badge/C%23-52%25-239120?style=flat-square&logo=csharp)
![HTML](https://img.shields.io/badge/HTML-30%25-E34F26?style=flat-square&logo=html5)
![Python](https://img.shields.io/badge/Python-18%25-3776AB?style=flat-square&logo=python)

---

## 👥 Contributors

- [Anasfassih8](https://github.com/Anasfassih8) and collaborators

---

## 📄 License

This project was developed as a graduation project. Please contact the authors before reusing or redistributing.

---

## 🙏 Acknowledgments

Special thanks to our supervisors and university faculty for their guidance throughout the development of this project.
