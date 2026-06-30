# Agentic Honeypot API 👮🏻‍♂️
AI-Powered Scam Engagement & Intelligence Extraction System


## Demo 🎥

![Watch Demo Video here](https://github.com/user-attachments/assets/fc57eb8b-32d1-4b48-b59f-82dc8478155d)

---

## Presentation 📊

Hackathon Presentation:  
[The Defenders PPT – India AI Impact Buildathon](https://github.com/user-attachments/files/25664547/The.Defenders.PPT.-.India.AI.Impact.Buildathon.1.pdf)

---

## Overview

Agentic Honeypot is an autonomous multi-agent system that detects scams, engages scammers like a real human victim, extracts forensic intelligence in real time, and submits a structured report via webhook.

Instead of blocking threats, the system wastes scammer time while collecting actionable intelligence.

---

## System Flow 🔄

Incoming Message
→ Express API (Authentication + Validation)
→ Lookup Agent (Scam Detection)
→ Handler Agent (Human-like Engagement)
→ Intelligence Extraction Engine
→ In-Memory Session Store
→ Stop Condition Check
→ Final Webhook Callback

---

## Architecture Components 🧠

### 1. Lookup Agent (Detection)

GPT-4o-mini classifies scam types using indicator categories such as:

- Urgency and threat tactics
- OTP / Aadhaar / PAN requests
- Payment pressure
- Suspicious links
- Authority impersonation
- Job / investment offers
- Lottery or reward claims
- Government scheme misuse
- Courier / customs threats

Output:

```json
{
  "scamDetected": true,
  "scamType": "bank_fraud",
  "confidence": 0.95
}
```

---

### 2. Handler Agent (Engagement)

- Maintains natural emotional tone
- Asks investigative follow-ups
- Avoids redundant intel collection
- Adapts strategy by turn count
- Explicitly identifies red flags

Up to 10 turns per session.

---

### 3. Intelligence Extraction Engine

Regex and pattern-based detection captures:

- Phone numbers
- Bank accounts
- UPI IDs
- Phishing links
- Email addresses
- Case / employee / reference IDs

All data is deduplicated and stored in session memory.

---

### 4. Reporting (Structured Webhook)

Session stops when:

- 10 turns reached
- 5 intelligence items collected
- 20 seconds inactivity

Final payload:

```json
{
  "sessionId": "ht-bank_fraud-1700000000000",
  "scamDetected": true,
  "scamType": "bank_fraud",
  "confidenceLevel": 0.97,
  "extractedIntelligence": {
    "phoneNumbers": [],
    "bankAccounts": [],
    "upiIds": [],
    "phishingLinks": [],
    "emailAddresses": []
  },
  "engagementMetrics": {
    "totalMessagesExchanged": 10,
    "engagementDurationSeconds": 75
  },
  "agentNotes": "Bank impersonation scam using urgency tactics."
}
```

---

## Tech Stack ⚙️

- Node.js 18+
- Express.js
- OpenAI GPT-4o-mini
- Zod (schema validation)
- dotenv (environment configuration)

---

## Setup

This API lives in the `Backend/` folder of the monorepo. Use the [Tester-HoneyPot](../Tester-HoneyPot/) app to run scenarios and receive callback payloads locally.

### Prerequisites

- Node.js 18+
- npm
- OpenAI API key

### 1. Clone the repository

```bash
git clone https://github.com/madhav-kr-singh/Agentic-HoneyPot.git
cd Agentic-HoneyPot/Backend
```

### 2. Install dependencies

```bash
npm install
```

### 3. Configure environment

```bash
cp example.env .env
```

Edit `.env`:

| Variable | Required | Description |
|----------|----------|-------------|
| `PORT` | No | HTTP port (default: `3000`) |
| `API_KEY` | Yes | Secret for `x-api-key` header |
| `OPENAI_API_KEY` | Yes | OpenAI key for Lookup/Handler agents |
| `FINAL_CALLBACK_URL` | Yes* | Webhook URL for final session payload |

\* Required for scoring/callback tests. For local dev with the tester, use the URL printed when you start the tester (e.g. `http://localhost:4000/callback`).

Example `.env`:

```env
PORT=3000
API_KEY=secret_api_key
OPENAI_API_KEY=sk-your-openai-key
FINAL_CALLBACK_URL=http://localhost:4000/callback
```

### 4. Start the tester (recommended, separate terminal)

```bash
cd ../Tester-HoneyPot
npm install
npm start
```

Use the printed `FINAL_CALLBACK_URL` in Backend `.env` if you have not set it yet.

### 5. Run the API

```bash
cd ../Backend
npm start
```

Server: **http://localhost:3000**  
Health check: **GET /health**

### 6. Verify with the tester UI

1. Open **http://localhost:4000**
2. Honeypot URL: `http://localhost:3000/honeypot`
3. API Key: value of `API_KEY` from `.env`
4. Run a scenario

### CLI-only testing (without web UI)

```bash
cd Tester-HoneyPot
node tester.js --url http://localhost:3000/honeypot --key your_api_key
```

Set Backend `FINAL_CALLBACK_URL` to the callback URL printed by the CLI (default: `http://<your-lan-ip>:3333/callback`).

For architecture, agents, stop conditions, and deployment details, see [docs/architecture.md](docs/architecture.md).

---

## API Endpoint 🔗

**POST** `/honeypot`
Header: `x-api-key`

Request:

```json
{
  "sessionId": "ht-bank_fraud-1700000000000",
  "message": {
    "sender": "scammer",
    "text": "Your SBI account will be blocked. Share OTP.",
    "timestamp": "2026-03-01T10:00:00.000Z"
  },
  "metadata": {}
}
```

Response:

```json
{
  "status": "success",
  "reply": "I'm worried. Can you share your phone number for one to one conversation?"
}
```

---

## Performance Metrics 📈

| Metric                | Value   |
| --------------------- | ------- |
| Detection Precision   | 99.3%   |
| Detection Recall      | 97.2%   |
| Avg Intel per Session | 2.7     |
| Avg Response Time     | 1.2s    |
| Callback Success Rate | 99.2%   |
| Cost per Conversation | < $0.01 |

---

## Supported Scam Types 🕵️

Bank fraud, UPI fraud, KYC fraud, job scams, lottery scams, utility fraud, government scheme fraud, courier/customs scams, crypto/investment fraud, tech support scams, loan scams, tax/refund fraud, insurance fraud, including multilingual variants.

---

## Security Considerations 🔐

- `x-api-key` authentication
- Environment-based secret storage
- No persistent PII storage
- In-memory session lifecycle
- Structured schema validation

---

## Recognition 🏆

Grand Finalists — Top 2% in India
AI Impact Buildathon — HCL & GUVI
India AI Summit 2026 — Bharat Mandapam

---

## Post-Hackathon Disclaimer

This repository has been updated after the conclusion of the India AI Impact Hackathon 2026.

The current version reflects architectural and structural changes made based on independent design decisions and optimization goals. As a result, this implementation may no longer strictly adhere to all original hackathon submission constraints, evaluation rubrics, or capped conditions imposed during the competition.

The system has been refined beyond hackathon limitations to better represent long-term scalability and technical standards.

---

BUILT WITH ❤️ BY THE DEFENDERS
