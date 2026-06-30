# 🛡️ Agentic Honeypot Tester

**Scam API Evaluation System v2.0**

A production-ready framework to benchmark and evaluate honeypot APIs against real-world scam scenarios. It simulates attacker agents, streams live interactions, scores detection capabilities, and validates callback payloads.

Built for **India AI Impact Hackathon 2026** (HCL / GUVI) — Grand Finale.

---

## 🚀 Features

* 🔟 Pre-built scam scenarios (bank fraud, job scams, crypto, UPI phishing, etc.)
* 📡 Real-time terminal streaming via SSE with ANSI color classification
* 🧠 Intelligent agent simulation with multi-turn conversations
* 📊 Multi-tab dashboard:

  * Live Output
  * Scores
  * Intelligence Extraction
  * Schema Validation
  * Payload Inspector
* 🔁 Automatic callback injection for honeypot payload delivery
* 🧪 Hackathon scoring system (out of 100)
* 🌐 Production deployment ready (Render support)

---

## 🏗️ Architecture

```
server.js (Node/Express) ←→ tester.js (Agent CLI) ←→ Honeypot API
        ↓                         ↓
      UI (SSE) ← Payload Callback → Intelligence Extraction
```

### Components

* **server.js**

  * HTTP server
  * SSE streaming
  * Child process management
  * In-memory state (clients, sessions, payloads)

* **tester.js**

  * Simulates scam agents
  * Multi-turn conversations (max 15 turns)
  * Extracts PII (phone, UPI, bank, email)
  * Sends callback payloads

* **ui.html**

  * Responsive dark UI
  * Scenario selector
  * Live logs + scoring visualization

---

## ⚙️ Quick Start

### Prerequisites

* Node.js 18+
* Running honeypot API (see [Backend README](../Backend/README.md))

This repo folder is `Tester-HoneyPot/`.

---

### Local setup

**Step 1 — Install and start the tester**

```bash
git clone https://github.com/madhav-kr-singh/Agentic-HoneyPot.git
cd Agentic-HoneyPot/Tester-HoneyPot
npm install
npm start
```

Default UI: **http://localhost:4000**

On startup, note the callback URL (example):

```
FINAL_CALLBACK_URL=http://localhost:4000/callback
```

**Step 2 — Configure the honeypot Backend**

In `Backend/.env`, set:

```env
FINAL_CALLBACK_URL=http://localhost:4000/callback
API_KEY=your_secret_api_key
OPENAI_API_KEY=sk-...
```

Start the Backend (`cd Backend && npm start` → port **3000**).

**Step 3 — Run scenarios in the UI**

| Field | Local value |
|-------|-------------|
| Honeypot URL | `http://localhost:3000/honeypot` |
| API Key | Same as Backend `API_KEY` |

Select a scenario → **Run Selected** or **Run All**.

---

### CLI mode (no web UI)

```bash
node tester.js --url http://localhost:3000/honeypot --key your_api_key
```

The CLI starts a callback listener on port **3333** by default. Set Backend:

```env
FINAL_CALLBACK_URL=http://<your-ip>:3333/callback
```

---

### Deploy to Render (production)

1. Deploy this service as a **Web Service**
2. Start command: `node server.js` (or use the included `Procfile`)
3. Optional env vars:

| Variable | Purpose |
|----------|---------|
| `PORT` | Server port (Render sets this) |
| `RENDER_EXTERNAL_URL` | Set automatically on Render |
| `PUBLIC_URL` | Override public base URL if needed |

4. Set on the **Backend** deployment:

```env
FINAL_CALLBACK_URL=https://your-tester.onrender.com/callback
```

The tester injects `CALLBACK_URL` into child processes when runs are started from the UI; you still need `FINAL_CALLBACK_URL` on the honeypot so it knows where to POST the final payload.

---

## 🔧 Configuration

| Field        | Description                 |
| ------------ | --------------------------- |
| Honeypot URL | Endpoint to test            |
| API Key      | Sent via `x-api-key` header |

* Config is saved in `localStorage` (dev only)

---

## 🧪 Usage

1. Enter **Honeypot URL + API Key**
2. Select a scenario
3. Click:

   * `Run Selected` or
   * `Run All`
4. Monitor:

   * Live terminal output
   * Extracted intelligence
   * Score breakdown

---

## 🔁 Callback Flow

1. The tester server exposes **POST `/callback`** to receive final payloads.
2. When a run starts, `server.js` injects `CALLBACK_URL` into `tester.js` (e.g. `http://localhost:4000/callback`).
3. Set the same URL on the honeypot as **`FINAL_CALLBACK_URL`** in `Backend/.env`.
4. When a session ends, the honeypot POSTs JSON to that `/callback` endpoint.

---

## 📦 Example Payload

```json
{
  "sessionId": "bankkycfreeze-123",
  "scamDetected": true,
  "extractedIntelligence": {
    "phoneNumbers": ["91-9821034567"]
  },
  "engagementMetrics": {
    "totalMessagesExchanged": 12
  }
}
```

---

## 📊 Scoring System (100 Points)

| Category     | Weight |
| ------------ | ------ |
| Detection    | 20     |
| Intelligence | 40     |
| Engagement   | 20     |
| Structure    | 20     |

### Criteria

* **Detection** → Scam classification accuracy
* **Intelligence** → PII extraction quality
* **Engagement** → Handles multi-turn conversations efficiently
* **Structure** → Valid schema + callback delivery

---

## 🎭 Scenarios

| ID               | Name                 | Channel  | Type       |
| ---------------- | -------------------- | -------- | ---------- |
| bankkycfreeze    | SBI Suspicious Login | SMS      | Bank Fraud |
| wfhdataentry     | WFH Data Entry       | WhatsApp | Job Scam   |
| cryptoinvestment | CryptoPro Returns    | Telegram | Investment |
| ...              | +7 more              | Various  | Various    |

---

## ✅ Requirements Checklist

* Endpoint returns HTTP 200
* Responds within 30 seconds
* Handles max conversation turns
* Callback successfully received
* Payload matches schema

---

## 🛠️ Troubleshooting

| Issue       | Fix                                 |
| ----------- | ----------------------------------- |
| No callback | Set `FINAL_CALLBACK_URL` on the honeypot to the tester `/callback` URL |
| SSE drops   | Keepalive runs every 20s            |
| Low score   | Improve PII extraction              |
| Debugging   | Check browser console + server logs |

---


## 🤝 Contributing

PRs welcome. Just don’t break the scoring system and pretend it was a feature.

---

## 💡 Notes

* Designed for **agentic evaluation**, not static testing
* Works best with adaptive or LLM-powered honeypots
* Optimized for Indian scam ecosystems

---


**Built by The Defenders**
