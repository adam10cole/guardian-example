# 🛡️ Guardian: Local-First PII Detection

> **Secure your data before it leaves your machine.**  Guardian is a privacy-centric Chrome extension that uses local LLMs to identify Personally Identifiable Information (PII) in real time.

---

![Disclaimer: This image was AI generated](https://i.imgur.com/MHNcoEN.jpeg)

---

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Python: 3.10+](https://img.shields.io/badge/Python-3.10+-blue.svg)
![Framework: FastAPI](https://img.shields.io/badge/Framework-FastAPI-009688.svg)
![Model: Phi--3--Mini](https://img.shields.io/badge/Model-Phi--3--Mini-green.svg)
![Status: MVP](https://img.shields.io/badge/Status-Functional_MVP-orange.svg)

## Overview
**Guardian** acts as a local security layer for your browser. In an era of rampant data harvesting, Guardian ensures that sensitive information, everything from Social Security numbers to Bitcoin addresses to API keys, are flagged and replaced with context aware fake data to prevent any PPI from ever leaving your hardware and entering the servers of large AI companies such as OpenAI or Google.

### Why It Matters
- **100% Local:** No data is sent to the cloud. Inference happens on *your* GPU.
- **Advanced Extraction:** Uses a fine-tuned version of Microsoft's Phi-3 Mini optimized for 60+ PII entity types.
- **Developer-Ready:** Built with a modern FastAPI backend and a Manifest V3 Chrome extension.

---

## Table of Contents
- [Architecture](#-architecture)
- [Key Features](#-key-features)
- [Quickstart Guide](#-quickstart-guide)
- [Usage Examples](#-usage-examples)
- [FAQ](#-faq)
- [Tech Stack](#-tech-stack)

---

## Architecture

!["Architecture Diagram"](https://i.imgur.com/hdRtUq2.png)

Guardian utilizes a **Client-Server-Model** architecture:
1. **Frontend:** A Chrome Extension monitors DOM input events.
2. **Gateway:** A FastAPI server handles high-concurrency requests via Uvicorn.
3. **Inference:** The `ab-ai/PII-Model-Phi3-Mini` model is loaded in 4-bit precision using `bitsandbytes`.

---

## Key Features

- **Real-Time Input Monitoring:** Automatically highlights input fields in red when PII is detected.
- **Deep Entity Extraction:** Detects Names, Emails, SSNs, Street Addresses, and even Cryptocurrency Wallets.
- **Session History:** The extension popup maintains a history of detected threats (stored locally via `chrome.storage`).
- **Low-Latency Inference:** Optimized for NVIDIA RTX 40-Series GPUs using 4-bit quantization to maintain high-performance browsing.
- **Non Invasive:** Guardian removes any personal information from your AI inputs without removing any necessary context required to receive an adequate response from an online LLM. 
- **Context Retrieval:** Guardian reinserts your personal information from a secure database on your own hardware when AI servers send back your response. As a user, you notice no difference. 

**Will be optimized for other machines later.** 

---

## Quickstart Guide

### Prerequisites
- **Python 3.10+**
- **NVIDIA GPU** (RTX 30/40 Series recommended with 8GB VRAM)
- **CUDA Toolkit 12.x**

### 1. Backend Setup
```bash
# Clone the repository
git clone https://github.com/adam10cole/guardian.git]
cd guardian

# Create and activate environment
conda create -n guardian python=3.10 -y
conda activate guardian

# Install dependencies
pip install torch transformers fastapi uvicorn bitsandbytes accelerate

# Start the server
uvicorn backend.main:app --host 0.0.0.0 --port 8000
```
### 2. Extension installation
1. Open Chrome and go to `chrome://extensions`.
2. Toggle **Developer Mode** to ON. 
3. Click **Load Unpacked**. 
4. Select the `extension/` folder in your project directory. 

!["Install extension GIF"](https://i.imgur.com/rOCKzsh.gif)

---
## Usage Examples

When you type any sensitive information into a form, Guardian detects it by constantly sending your text through a local AI model which parses and saves data to a hashtable.

!["Example"](https://i.imgur.com/RIPeiCe.gif)

### API Response Format

The backend returns a standardized JSON structure for every scan:
```json
{
  "entities": [
    { "entity": "EMAIL", "text": "adam10cole@gmail.com" },
    { "entity": "PERSON", "text": "Adam Cole" }
  ],
  "status": "success"
}
```

---
## FAQ

**Q: Does Guardian store my text logs?** A: No. The backend processes the text in memory for inference and does not write the input text to disk.

**Q: Why does the first scan take a few seconds?** A: The first request triggers the model weights to "warm up" in your GPU VRAM. Subsequent scans are near-instant.

**Q: Can I use this on macOS?** A: Currently, Guardian requires an NVIDIA GPU for the 4-bit quantization logic. Support for Apple Silicon (MPS) is on the roadmap.

**Q: How completed is the project?*** A: The project is not complete. It was started this semester (Winter 26') and is not MVP ready. As such, the demo GIFs are just placeholders. 

---
## Tech Stack

- **Languages:** Python, JavaScript, CSS, HTML
- **AI/ML:** Hugging Face `transformers`, `bitsandbytes`, `Phi-3 Mini`
- **Backend:** FastAPI, Uvicorn, Pydantic
- **Frontend:** Chrome Extension API (Manifest V3)

---

This project is licensed under the MIT License 
