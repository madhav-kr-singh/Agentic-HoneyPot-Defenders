# Agentic HoneyPot Defenders

Autonomous honeypot API and evaluation tester for scam engagement and intelligence extraction.

## Repository layout

| Directory | Purpose | Default port |
|-----------|---------|--------------|
| [`Backend/`](Backend/) | Honeypot API (Express + OpenAI agents) | 3000 |
| [`Tester-HoneyPot/`](Tester-HoneyPot/) | Web UI + scenario runner + callback receiver | 4000 |

## Prerequisites

- **Node.js 18+** (`node -v`)
- **npm**
- **OpenAI API key** (Backend only)

## Quick start (local)

### Terminal 1 — Tester

```bash
cd Tester-HoneyPot
npm install
npm start
```

Open **http://localhost:4000**. On startup, the server prints:

```
FINAL_CALLBACK_URL=http://localhost:4000/callback
```

Copy that URL for the Backend `.env`.

### Terminal 2 — Backend

```bash
cd Backend
npm install
cp example.env .env
```

Edit `.env`:

```env
PORT=3000
API_KEY=your_secret_api_key
OPENAI_API_KEY=sk-...
FINAL_CALLBACK_URL=http://localhost:4000/callback
```

Start the API:

```bash
npm start
```

Verify: **http://localhost:3000/health**

### Run a test

1. In the tester UI (**http://localhost:4000**), set:
   - **Honeypot URL:** `http://localhost:3000/honeypot`
   - **API Key:** same value as `API_KEY` in Backend `.env`
2. Pick a scenario → **Run Selected** or **Run All**
3. Watch live output, scores, and callback payload tabs

## Documentation

- [Backend README](Backend/README.md) — API overview, endpoints, setup
- [Tester README](Tester-HoneyPot/README.md) — Scenarios, scoring, UI usage
- [Architecture](Backend/docs/architecture.md) — System design, agents, deployment

## Production

Deploy **Backend** and **Tester** as separate services (e.g. Render).

1. Deploy the tester and note its public URL.
2. Set on the Backend deployment:

   ```env
   FINAL_CALLBACK_URL=https://your-tester.onrender.com/callback
   ```

3. On Render, the tester uses `RENDER_EXTERNAL_URL` automatically for callback injection.

## Troubleshooting

| Problem | Fix |
|---------|-----|
| 401 Unauthorized | Match `x-api-key` in UI with Backend `API_KEY` |
| No callback / low Structure score | Set `FINAL_CALLBACK_URL` to tester `/callback` URL |
| OpenAI errors | Check `OPENAI_API_KEY` and billing |
| Backend won't start | Run `npm install` in `Backend/`; ensure `.env` exists |

---

**Built with ❤️ by The Defenders**
