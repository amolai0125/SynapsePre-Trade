# DealAI

AI-powered deal evaluation platform: a React frontend, a FastAPI backend that fans a deal out to
four AI "teams" (credit/legal/risk/compliance) over Vertex AI (Gemini) with a RAG layer over ISDA
/ credit / compliance docs, and a GitHub Actions pipeline that deploys both to Cloud Run.

## Layout

- `Frontend/` — React + Vite + TypeScript + Tailwind. Owned by Sonali, Mansi, Shruti. See
  `Frontend/README.md`.
- `Backend/` — FastAPI. See below and `DEPLOYMENT.md`.
- `.github/workflows/` — `ci.yml` (tests/build on PRs), `deploy.yml` (deploy to Cloud Run on
  push to `main`).

| Path | Owner |
| --- | --- |
| `Backend/routes` (evaluate_deal, chat) | Aina / Amol |
| `Backend/agent`, `Backend/vertex_ai`, `Backend/sys_prompt` | Vaibhav |
| `Backend/teams` (legal, risk, credit, compliance) | Aina |
| `Backend/rag` (embedding, vector_search) | Adhika |
| `Backend/data/docs` (isda, credit_policy, compliance_policy) | Amol |

## Backend quickstart

```bash
cd Backend
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
copy .env.example .env   # fill in your GCP project, or leave blank to run in mock mode
uvicorn main:app --reload
```

Without `GOOGLE_CLOUD_PROJECT` set (or with `VERTEX_AI_MOCK=true`), `Backend/vertex_ai/client.py`
returns deterministic mock responses instead of calling Vertex AI — useful for local dev and CI
without GCP credentials.

## Frontend quickstart

```bash
cd Frontend
npm install
copy .env.example .env   # VITE_API_BASE_URL, defaults to http://localhost:8000
npm run dev
```

## Deploying to GCP

See `DEPLOYMENT.md` for the one-time GCP setup (Artifact Registry, service account, IAM roles)
and the GitHub secrets the `deploy.yml` workflow needs. Once configured, pushing to `main`
builds and deploys both services to Cloud Run automatically.
