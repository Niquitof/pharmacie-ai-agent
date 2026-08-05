# 💊 Pharmacie AI Agent - WhatsApp Automation Workflow (n8n)

An intelligent low-code automation agent built with **n8n** for handling pharmacy customer inquiries via the **WhatsApp Cloud API**.

The workflow automatically processes text messages and images (such as medical prescriptions) using OCR and LLM Vision models, maintains multi-turn conversational memory, and queries stock and pricing in real time against a **PostgreSQL** database.

---

## 🛠️ Stack & Key Components

* **Automation Engine:** n8n Workflow Automation Engine.
* **AI Agent & LLM:** LangChain Agent powered by **Google Gemini**.
* **Vision & OCR Engine:** Tesseract.js (initial OCR pass) + Gemini Vision (`AI Image Analyzer`) for fallback/complex prescription parsing.
* **Database & Search:** PostgreSQL database tool (`search_stock_pharmacie`) to query product inventory, public prices, and Healthcare discounts.
* **Messaging Channel:** WhatsApp Cloud API (Triggers and response dispatch).
* **State Management:** Window Buffer Memory with custom session keys and interactive reset capability (`/reset`).

---

## ⚙️ Workflow Architecture

1. **Trigger & Routing:** Inbound messages received via Webhook or WhatsApp Trigger are split into text and image execution paths using a Switch node.
2. **Media Download & OCR Processing:** Images are downloaded securely, processed via Tesseract OCR, and evaluated for confidence level. If confidence is insufficient (< 65%), the image is escalated to AI Vision.
3. **Payload Normalization:** Data from text inputs or vision models is unified into a clean, standardized JSON schema (`Unificate JSON`).
4. **Agent Orchestration:** The LangChain agent (`Orchestra`) evaluates customer intent and invokes the PostgreSQL database tool (`search_stock_pharmacie`) when product availability or pricing is requested.
5. **Formated Response:** Returns a standardized WhatsApp response template featuring pricing and stock status.

---

## 🚀 How to Import and Run

1. Clone this repository:
   ```bash
   git clone https://github.com/Niquitof/pharmacie-ai-agent.git