# RAG Pipeline (n8n)
A modular Retrieval-Augmented Generation (RAG) pipeline built with n8n, designed for document-grounded chat, automated knowledge updates, and vector store hygiene.

## Tech Stack

    - n8n – orchestration
    - Gemini – chat model + embeddings
    - PostgreSQL – chat memory
    - Supabase – vector store
    - Google Drive – document source

## Workflows

1. Main RAG Chat Workflow
Triggered via chat. Uses Gemini for response generation, Gemini embeddings for retrieval, PostgreSQL for conversation memory, and Supabase as the vector store.

2. Vector Update Workflow
Triggered on Google Drive file or folder uploads/changes. Processes documents, generates embeddings, and upserts them into the vector store.

3. Vector Cleanup Workflow
Scheduled workflow that removes vectors for documents no longer present in Google Drive, keeping the knowledge base consistent and up to date.