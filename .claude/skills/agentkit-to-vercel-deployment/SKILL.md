---
name: AgentKit to Vercel Deployment
description: Deploys OpenAI AgentKit ChatKit apps (chatkit + managed-chatkit) to Vercel. Use when setting up local ChatKit backends, wiring Vite frontends, configuring env vars, domain allowlists, or troubleshooting Vercel deploys.
---

# AgentKit to Vercel Deployment

## Purpose

This skill captures a repeatable, reliable deployment process for OpenAI ChatKit starters, including:
- Local setup of chatkit (self-hosted FastAPI) and managed-chatkit (workflow sessions)
- Where to integrate custom agent code
- Vercel deployment with split frontend/backend projects
- Required env vars and domain allowlist steps
- Common failure modes and fixes

## Prerequisites

- GitHub repo with `chatkit/` and/or `managed-chatkit/`
- Vercel CLI installed (`npm i -g vercel`) or Vercel UI access
- OpenAI API key
- Domain allowlist key from OpenAI settings (domain_pk_...)

## Workflow

### 0) Read reference docs as needed

Use progressive disclosure. Start with [references/agentkit-overview.md](references/agentkit-overview.md), then open the specific section docs only when needed:
- [references/agent-builder.md](references/agent-builder.md)
- [references/chatkit.md](references/chatkit.md)
- [references/agents-sdk.md](references/agents-sdk.md)
- [references/domain-allowlist.md](references/domain-allowlist.md)
- [references/vercel-deploy.md](references/vercel-deploy.md)
- [references/vercel-env.md](references/vercel-env.md)
- [references/vercel-python.md](references/vercel-python.md)
- [references/vercel-monorepo.md](references/vercel-monorepo.md)
- [references/vercel-build.md](references/vercel-build.md)

### 1) Decide deployment topology (always split UI + backend)

Use separate Vercel projects for each app:
- chatkit backend (FastAPI)
- chatkit UI (Vite)
- managed-chatkit backend (FastAPI)
- managed-chatkit UI (Vite)

Reason: single-project static+python routing repeatedly caused 404s for the UI.

### 2) Backend wiring (FastAPI entrypoint)

In each backend repo root, ensure:
- `api/index.py`
- `requirements.txt`

Example `api/index.py`:
```py
import os
import sys

ROOT = os.path.dirname(os.path.dirname(__file__))
BACKEND_DIR = os.path.join(ROOT, "backend")
sys.path.insert(0, BACKEND_DIR)

from app.main import app  # noqa: E402
```

Backend deps (requirements.txt):
- chatkit:
  - fastapi, uvicorn, openai, openai-chatkit
- managed-chatkit:
  - fastapi, uvicorn, httpx

### 3) Frontend env wiring

chatkit UI:
- `VITE_CHATKIT_API_URL = https://<chatkit-backend>/chatkit`
- `VITE_CHATKIT_API_DOMAIN_KEY = domain_pk_...`

managed-chatkit UI:
- `VITE_API_URL = https://<managed-backend>`
- `VITE_CHATKIT_WORKFLOW_ID = wf_...`
- `VITE_CHATKIT_API_DOMAIN_KEY = domain_pk_...`

### 4) Managed UI must support VITE_API_URL

Edit:
`managed-chatkit/frontend/src/lib/chatkitSession.ts`

Add:
```ts
const apiBase = readEnvString(import.meta.env.VITE_API_URL);
const apiEndpoint = apiBase
  ? `${apiBase.replace(/\/$/, "")}/api/create-session`
  : "/api/create-session";
```

Then pass `endpoint = apiEndpoint` to `createClientSecretFetcher`.

### 5) Domain allowlist (required)

If UI shows blue overlay with `Domain verification failed`, add UI domains to:
`https://platform.openai.com/settings/organization/security/domain-allowlist`

Then set `VITE_CHATKIT_API_DOMAIN_KEY` in UI projects and redeploy.

### 6) Vercel project creation (CLI)

Backend project (repo root):
```bash
vercel --prod
```

UI project (frontend dir):
```bash
cd frontend
vercel --prod
```

Use `vercel env add` to set env vars for each project (prod, preview, dev).

### 7) Verify

- UI loads (HTTP 200)
- managed backend `POST /api/create-session` returns 200

Example:
```bash
curl -I https://<ui-domain>
curl -X POST https://<managed-backend>/api/create-session \
  -H "Content-Type: application/json" \
  -d '{"workflow":{"id":"wf_..."}}'
```

## Integration points (where to add your code)

chatkit (self-hosted):
- `chatkit/backend/app/server.py` -> `StarterChatServer.respond`
  - Add tools, retrieval, custom system prompts, model selection

managed-chatkit:
- Backend only creates ChatKit sessions via OpenAI
- Your workflow logic lives in Agent Builder, not in this repo

## Troubleshooting

- **Vercel UI 404**: do not deploy UI+backend in one project; split projects.
- **Blue overlay**: domain allowlist missing or domain key not set.
- **Managed UI fails**: missing `VITE_API_URL` or workflow ID.
- **Python mismatch**: use python3.11 locally; Vercel uses python3.12.
- **Vercel runtime error**: remove invalid `runtime` entries from vercel.json.

## Examples

### Example 1: Deploy chatkit (self-hosted)

User request:
```
Deploy chatkit to Vercel
```

You would:
1. Create backend project at repo root
2. Create UI project in `frontend/`
3. Set `VITE_CHATKIT_API_URL` and `VITE_CHATKIT_API_DOMAIN_KEY`
4. Add UI domains to OpenAI allowlist
5. Verify UI loads

### Example 2: Deploy managed-chatkit

User request:
```
Deploy managed-chatkit with workflow wf_... to Vercel
```

You would:
1. Ensure `VITE_API_URL` support in frontend
2. Create backend project (repo root)
3. Create UI project (`frontend/`)
4. Set env: `VITE_CHATKIT_WORKFLOW_ID`, `VITE_API_URL`, `VITE_CHATKIT_API_DOMAIN_KEY`
5. Add UI domain to allowlist
6. Verify `POST /api/create-session`
