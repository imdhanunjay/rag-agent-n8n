# Setup Supabase for vector storage

This guide creates the `documents` table with a vector column suitable for the Supabase vector store used by this n8n workflow.

Prerequisites
- Supabase account: https://app.supabase.com
- Project created (free tier is fine)
- Access to the SQL editor and API keys in the project settings

1) Enable pgvector (SQL editor)
Run the following in Supabase SQL editor to ensure the vector extension is available:

```sql
create extension if not exists vector;
```

2) Create the `documents` table
Adjust the vector dimension to match your embedding model. The example below uses 1536 — update it if your embedding model uses another size.

```sql
create table if not exists documents (
  id uuid primary key default gen_random_uuid(),
  content text,
  metadata jsonb,
  embedding vector(1536), -- change 1536 to your model's dimension if needed
  created_at timestamptz default now()
);
```

3) Service role key
- In Supabase dashboard → Settings → API, copy the Service Role Key.
- Use this key in your n8n Supabase credential (do NOT commit it to GitHub).
- Ensure the key has insert/select permissions for the `documents` table.

4) Notes & troubleshooting
- The `embedding` vector dimension must match the embeddings returned by the model (Gemini embedding model). If dimensions mismatch, inserts will fail.
- Test inserting a row from the Supabase SQL editor first to confirm the table accepts vector data.
- For production, enable Row Level Security (RLS) and create a safe, limited-access key for app usage. Keep service keys in secure storage (n8n credentials or environment variables).
