# Agent Builder (OpenAI) quick reference

Use when working with managed-chatkit and workflow IDs.

Key points:
- Workflows live in Agent Builder and produce workflow IDs (wf_...).
- Managed-chatkit backend only exchanges workflow ID + API key for a client secret.
- Logic is configured in the workflow, not in this repo.

Operational notes:
- Keep workflow IDs in VITE_CHATKIT_WORKFLOW_ID (frontend env).
- Backend path is /api/create-session.

Source:
```
https://platform.openai.com/docs/agent-builder
```
