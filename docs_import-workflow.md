# Importing the workflow into n8n

Two methods: n8n UI (recommended) or n8n CLI.

1) UI (recommended)
- Log into your n8n instance (http://localhost:5678 or your hosted URL).
- Click "Workflows" → "Import".
- Upload `RAGAgent1-n8n-workflow.sanitized.json` or paste its JSON.
- After import, open the workflow and review every node.
- For each node that needs credentials (Google Drive, Google PaLM/Gemini, Supabase, Postgres), click the node and set the credential from the n8n credentials dropdown.
- Replace placeholder values:
  - Chat trigger: set a real webhookId by re-creating the webhook trigger or re-saving the node.
  - Download file node: set `fileId` to your Google Drive file id.
- Save and activate any nodes you want to run automatically.

2) CLI
- If you have n8n CLI installed:
  - n8n import:workflow --input=RAGAgent1-n8n-workflow.sanitized.json
- Then log into the UI to attach credentials.

Notes
- Credentials are stored by n8n separately from the workflow export — this is why exported workflows are safe to share when credentials are removed.
- If a node fails after import, open its execution log for details and ensure credentials are correct and network access is available.