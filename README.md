# 💊 Pharmacie AI Agent - WhatsApp Automation Workflow (n8n)

An enterprise-grade automation architecture built with **n8n** for handling pharmacy customer inquiries via the **WhatsApp Cloud API**.

The system dynamically processes text messages and images (such as handwritten medical prescriptions) using multi-stage OCR and LLM Vision models, handles live database stock queries against **PostgreSQL**, routes general information queries through a dedicated Mini Agent, and implements dynamic Human Handover protection with conversational state tracking.

---

## 🛠️ Stack & Key Components

* **Automation Engine:** n8n Workflow Automation Engine.
* **AI & LLM Orchestration:** LangChain Agent powered by **Google Gemini**.
* **Vision & Multi-Stage OCR:** Tesseract.js (initial fast pass) + Gemini Vision (`AI Image Analyzer`) for complex or handwritten prescription fallback.
* **Database & Search:** PostgreSQL database integration (`search_stock_pharmacie`) for real-time inventory, pricing, and healthcare discount lookup.
* **Messaging Channel:** WhatsApp Cloud API (Webhook triggers, template routing, and response dispatch).
* **State Management & Interception:** Window Buffer Memory, custom session dynamic locks (`info_waiting_human`, `last_human_warning`), and interactive session reset (`/reset`).

---

## ⚙️ Workflow Architecture

1. **Multi-Modal Ingestion & Routing:**
   - Dual entry points via Webhook and native WhatsApp Trigger.
   - Intelligent payload normalization and media filtering for text and image paths.

2. **Smart Prescription Vision (OCR + LLM):**
   - High-speed pre-processing via Tesseract OCR.
   - Automatic confidence scoring (< 65% triggers Gemini Vision analysis for prescription extraction).

3. **Multi-Agent & Tool Routing Architecture:**
   - **Main Agent Orchestrator:** Evaluates customer intent, manages conversation flow, and queries the PostgreSQL database for product stock/pricing.
   - **General Info Mini Agent:** Handles operational questions (payment methods, business hours, locations) efficiently without polluting main tool contexts.

4. **Human Handover & Anti-Spam Safeguards:**
   - **Dynamic Human Interception:** Automatically pauses AI automated responses when a human agent is requested or active (`info_waiting_human`).
   - **Anti-Spam Notification Throttling:** Smart warning tracker (`last_human_warning`) prevents flooding users with repeated human takeover signatures during ongoing conversations.

---

## 📋 Changelog

### [v2.0.0] - 2026-08-10

#### 🌟 Added
* **General Info Mini Agent:** Isolated handling of general inquiries (payment methods, schedules, insurance coverage) using a secondary Gemini model to reduce latency and token overhead.
* **Smart Intent Router:** Automated categorization of user messages (`INFO` vs `WAITING`) during human takeover states.

#### 🛡️ Enhanced
* **Anti-Spam Human Handover:** Integrated dynamic state verification (`last_human_warning` in PostgreSQL) with a 10-minute cooldown to prevent repeating waiting notifications.
* **Memory & State Interception:** Improved conversation locking during active human intervention.

---

### [v1.0.0] - 2026-08-01

#### 🌟 Initial Release
* Core Gemini LLM Agent integration with PostgreSQL database tool binding.
* Dual-stage OCR and Gemini Vision for reading handwritten medical prescriptions.
* Basic WhatsApp Cloud API webhook ingestion and response formatting.

---

## 🚀 How to Import & Run

1. **Clone the repository:**
   ```bash
   git clone https://github.com/Niquitof/pharmacie-ai-agent.git

   