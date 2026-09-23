# Architecture City

Static 3D architecture map for a software repository: districts, towers, and evidence-labelled traces rendered in the browser with Three.js. This repository ships a committed sample graph (derived from `cheffer0723/asymmetry`) plus a Railway-ready Caddy service so deployers get a working city with **no secrets**.

## Deploy on Railway

Marketplace overview: [`TEMPLATE.md`](TEMPLATE.md)  
Publisher checklist: [`RAILWAY_PUBLISH.md`](RAILWAY_PUBLISH.md)

What Railway runs:

- One service from the repo root
- `Dockerfile` → Caddy 2 on `$PORT`
- Healthcheck: `/health`
- Variables: none required
- Assets: `index.html`, vendored Three.js (`three.core.js` included), summary + full graph JSON

After you publish, paste the Deploy button here:

```md
[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/deploy/<your-template-code>)
```

## What visitors get

- Fast first paint from `architecture-city-summary.json` (CITY mode)
- Optional **LOAD FULL DETAILS** from `architecture-map.json`
- Sample attribution from `source-manifest.json` / graph `repository` field
- Read-only, safety-filtered orientation — not source contents, not live traffic

## Customize the sample

1. Replace `architecture-city-summary.json` and `architecture-map.json` with your own graph (same schema).
2. Update `source-manifest.json` so snapshot attribution stays honest.
3. Edit focus buttons / district labels in `index.html` if your domains differ from the Asymmetry sample.
4. Redeploy. No build step.

## Local run

### Option A — Caddy (same config as Railway)

```bash
SITE_ROOT=. PORT=8080 caddy run --config Caddyfile --adapter caddyfile
```

Open `http://127.0.0.1:8080/`

### Option B — any static server from the repo root

```bash
python3 -m http.server 8080
```

### Smoke check

```bash
./scripts/smoke.sh http://127.0.0.1:8080
```

## GitHub Pages

`.github/workflows/deploy.yml` still publishes the repository root to Pages. Railway and Pages are independent hosts of the same static viewer.

## Boundaries

- `cheffer0723/asymmetry` remains the product source of truth when this sample graph is used.
- This repository is a read-only snapshot viewer; it does not write back to the product repo.
- Public posture: orientation and evidence boundaries only — no private internals, production traffic claims, or live system guarantees.

## Layout

| Path | Role |
|---|---|
| `index.html` | 3D viewer |
| `architecture-city-summary.json` | Lightweight first-load graph |
| `architecture-map.json` | Full safety-filtered graph |
| `source-manifest.json` | Snapshot provenance |
| `vendor/three/` | Vendored Three.js (`three.core.js`, module, OrbitControls) |
| `Dockerfile` / `Caddyfile` / `railway.toml` | Railway static host |
| `scripts/smoke.sh` | Deploy verification |
| `TEMPLATE.md` | Railway marketplace overview |
| `legacy/` | Earlier 2D explorer (not served by Railway) |
