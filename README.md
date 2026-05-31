# ESG AI HUB

A modern, single-file web application that serves as the central home for all AI agents built within the organization. Built with vanilla HTML/CSS/JS — no build step, no dependencies.

![status](https://img.shields.io/badge/status-live-3ddc97) ![build](https://img.shields.io/badge/build-none%20needed-2bb6ff)

## Features

- **Agent Hub home page** — every AI agent shown as a card (name, description, status, open action). Add new agents by editing one array.
- **Recruitment Agent** with two tabs:
  - **Job Poster** — embeds the n8n job-posting form via iframe with a loading state.
  - **Job Dashboard** — live KPIs (Total / Active / Closed / Draft), a recent-jobs table with search, status filter, sortable columns, pagination, manual refresh, and auto-refresh (polls every 30s).
- **Live data** from an n8n webhook exposing the "Job Status" data table.
- Responsive, dark, professional UI.

## Project structure

```
.
├── index.html      # the entire app (HTML + CSS + JS)
├── vercel.json     # static hosting config
└── README.md
```

## Run locally

It must be served over http(s) (not opened as a file://) so the browser allows the fetch to n8n:

```bash
# Python
python -m http.server 8000
# then open http://localhost:8000

# or Node
npx serve .
```

## Deploy to Vercel

**Git integration (recommended):**
1. Push this repo to GitHub.
2. In Vercel: **Add New → Project → Import** this repo.
3. Framework preset: **Other**. Leave build/output empty.
4. **Deploy.** Every push auto-deploys after that.

**CLI:**
```bash
npm i -g vercel
vercel login
vercel --prod
```

## Configuration

The dashboard reads live data from this endpoint (edit the line near the top of the
`<script>` in `index.html` to change it):

```js
var JOBS_WEBHOOK_URL = "https://nisar-ai-agents.app.n8n.cloud/webhook/job-status";
```

The n8n workflow must be **Active** and its **Respond to Webhook** node must send the
header `Access-Control-Allow-Origin: *` so the browser can read the response.

## Adding a new agent

In `index.html`, add an object to the `AGENTS` array and a matching page builder.
The home card and routing wire up automatically.

```js
var AGENTS = [
  { id:"recruitment", name:"Recruitment Agent", description:"…",
    status:"active", icon:"users", page:"recruitment" }
  // add new agents here
];
```
