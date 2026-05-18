# RAG Agent1 — n8n Workflow (Supabase + Google Gemini)

A small RAG (Retrieval-Augmented Generation) agent built in n8n that:
- Downloads a document (Google Drive).
- Embeds documents using Google Gemini (PaLM) embeddings.
- Stores embeddings in Supabase (vector store).
- Exposes a chat-triggered AI Agent using Google Gemini with Postgres chat memory.

This repository contains a sanitized n8n workflow export you can import into your n8n instance. No credentials or secrets are included.

Quick links
- Workflow (sanitized): `RAGAgent1-n8n-workflow.json`
- Docs: `docs/` (import, run locally, Supabase setup)
- Demo assets: `assets/images/` (add screenshots & GIFs)

Demo screenshot
- Add your workflow screenshot to '<img width="1530" height="1010" alt="Image" src="https://github.com/user-attachments/assets/eae07fcd-64a2-4f03-baa3-e115ca5c4bc3" />' '<img width="2880" height="1497" alt="Image" src="https://github.com/user-attachments/assets/446f4b70-72b7-49d6-a80f-2692904102f8" />'and a short GIF '<img width="2880" height="1474" alt="Image" src="https://github.com/user-attachments/assets/9dfe188c-2f84-4124-b2a0-a7d5aef3b41c" />'.

Tech stack
- n8n (workflow orchestration)
- Google Gemini / PaLM (embeddings + chat)
- Supabase (pgvector vector DB)
- Postgres (chat memory)
- Google Drive (document source)

Quickstart (high level)
1. Create a Supabase project and enable pgvector.
2. Create a Postgres DB (local or managed) for n8n memory.
3. Obtain Google PaLM / Gemini API key.
4. Run n8n locally (Docker Compose recommended).
5. Import `RAGAgent1-n8n-workflow.json` into n8n (see `docs/import-workflow.md`).
6. In the n8n UI, add credentials for Google Drive, Google PaLM, Supabase, and Postgres.
7. Update the Download file node with your Drive fileId and run the ingestion flow.

Security note
- This repo contains no secrets. Do not commit API keys, service-role keys, or `.env` with real values.
- Use n8n credentials to store integration secrets on the n8n instance.
- For CI use GitHub Secrets.

Files in this repo
- `RAGAgent1-n8n-workflow.json` — sanitized workflow export (import into n8n)
- `README.md` - project overview & quickstart
- `.env.example` - example environment variables
- `.gitignore`
- `LICENSE` - MIT
- `docs/` - setup and run instructions
- `assets/` - screenshots and demo assets

Contact
imdhanunjay - https://github.com/imdhanunjay
