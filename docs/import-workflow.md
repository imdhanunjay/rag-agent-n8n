# Importing the workflow into n8n

Purpose
This document explains how to import the sanitized workflow `RAGAgent1-n8n-workflow.json` into an n8n instance and attach credentials (Google Drive, Google Gemini / PaLM, Supabase, Postgres) after import.

Prerequisites
- A running n8n instance (local or hosted). See `docs/run-n8n-local.md` for a Docker Compose option.
- n8n credentials created for:
  - Google Drive (OAuth2)
  - Google PaLM / Gemini (API key)
  - Supabase (URL + service role key)
  - Postgres (user/password)
- The sanitized workflow file present in the repository root: `RAGAgent1-n8n-workflow.json`.

Import using the n8n UI (recommended)
1. Open your n8n instance in a browser (e.g., http://localhost:5678).
2. Go to "Workflows" → "Import".
3. Upload `RAGAgent1-n8n-workflow.json` (choose file) or paste its JSON contents.
4. Click "Import". The workflow will open in the editor.

Attach credentials and review nodes
1. Open each node that needs credentials (Google Drive, Google Gemini model nodes, Supabase vector store, Postgres memory).
2. For each node, choose the appropriate credential from the node's "Credentials" dropdown. If you haven’t created them yet:
   - Create credentials via the n8n sidebar → Credentials → New credential (select type and fill in values).
   - Save credentials, then return to the node and select them.
3. Replace placeholder values in node parameters:
   - Download file node: set `fileId` to your Google Drive file ID (or use a publicly accessible test file).
   - Chat trigger node: confirm webhook is created / re-save the chat trigger node to register the webhook URL.
   - Supabase nodes: verify `tableName` (default `documents`) matches the Supabase table you set up.

Test the ingestion flow
1. In the workflow editor, select the manual trigger "When clicking ‘Execute workflow’" and click "Execute workflow".
2. Watch the node executions in the right-side execution log. The Download file node should fetch your file, the loader should pass the document to embeddings, and the Supabase node should attempt to insert embeddings.
3. If any node fails, open its execution log to see the error and verify credentials, network access, and Supabase table schema (pgvector column type and vector dimension).

Test the chat flow
1. With the workflow open, create or re-check the webhook for the chat trigger node (n8n must register the webhook).
2. Use the chat input (or a test request to the webhook URL) to send a message. The AI Agent node uses the connected Google Gemini chat model and the Postgres chat memory to respond.
3. Debug failures by checking the node execution output — credential errors and webhook permission issues are the most common.

Notes & troubleshooting
- Credentials are not stored in the workflow file. You must attach credentials in the n8n UI after import.
- If Supabase insertion fails, confirm:
  - pgvector extension exists in Supabase (see `docs/setup-supabase.md`).
  - `embedding` column vector size matches the embedding model dimension.
  - Supabase key has necessary insert/select privileges.
- Webhook issues: when n8n runs behind a proxy or uses a different host, the webhook URL may differ. Use the registered webhook URL displayed in the chat trigger node to test externally.

Where to look for more help
- n8n docs: https://docs.n8n.io
- Supabase docs about pgvector and SQL editor
- Google PaLM / Gemini API docs for embedding & chat model usage
