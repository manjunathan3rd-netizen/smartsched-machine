# SmartSched AI — Deployable Demo Build

A fully functional, zero-build, single-file web app implementing the SmartSched AI
concept: live Gantt scheduling, priority scoring, disruption simulation with
real-time replanning, a data-driven AI assistant, and a what-if simulator —
all running client-side in `index.html`.

## What's real vs. what's simulated in this build

This is the **demo/front-end layer** of the full blueprint. It is 100% functional —
not mocked buttons — but the scheduling engine is a JavaScript greedy/priority-based
optimizer, not the production Google OR-Tools + trained ML stack described in the
architecture doc. It shares the same UI, data model, and decision logic shape, so it
demos convincingly and the real backend can be swapped in behind the same interface
later (see "Upgrading to the full stack" below).

Fully real in this build:
- Priority scoring formula (tier + deadline urgency + order size)
- Greedy constrained scheduling (machine type match, worker skill match, material stock)
- Live re-optimization on disruption (machine breakdown, rush order, material shortage)
- Rule-based explainability assistant that reads the actual computed schedule
- What-if simulator (add machine/worker, cut material stock)

## Deploy this in under 2 minutes

**Option A — Vercel / Netlify (drag and drop)**
1. Go to [vercel.com/new](https://vercel.com/new) or [app.netlify.com/drop](https://app.netlify.com/drop)
2. Drag the `smartsched-ai` folder onto the page
3. Done — you get a live public URL instantly (no build step, no config needed)

**Option B — GitHub Pages**
```bash
git init
git add .
git commit -m "SmartSched AI demo"
git branch -M main
git remote add origin <your-repo-url>
git push -u origin main
```
Then in your repo: **Settings → Pages → Deploy from branch → main → / (root)**

**Option C — Run locally**
Just open `index.html` directly in any browser — no server required.

## Upgrading to the full production stack

Replace the `runScheduler()` function's logic with a call to your backend:
```js
async function runScheduler(){
  const res = await fetch('/api/schedule/generate', { method:'POST', body: JSON.stringify({orders, machines, workers, materials}) });
  schedule = await res.json();
}
```
Point that endpoint at the FastAPI + OR-Tools + XGBoost service described in the
architecture document, and swap the rule-based `explainOrder()` responses for calls
to `/api/ai/explain` backed by an LLM (Groq/Gemini free tier). The UI, data shapes,
and demo flow stay identical.

## File structure
```
smartsched-ai/
├── index.html   # the entire app — HTML, CSS, and JS in one file
└── README.md
```
