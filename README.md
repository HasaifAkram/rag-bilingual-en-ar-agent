# 🌐 Bilingual (English/Arabic) RAG Chat Agent

A Retrieval-Augmented Generation (RAG) chatbot built in n8n that automatically detects whether an incoming message is in English or Arabic, then routes it to a language-specific AI agent backed by its own vector-indexed knowledge base.

## 🎯 Problem It Solves
Serving customers in multiple languages usually means maintaining separate bots or manually translating knowledge bases. This workflow detects the input language automatically and answers using documents retrieved in the matching language, so one workflow can serve a bilingual audience naturally.

## ⚙️ Tech Stack
- **n8n** (workflow orchestration)
- **LangChain AI Agents** (`@n8n/n8n-nodes-langchain.agent`) — one per language
- **Pinecone** (vector store, multiple namespaces/indexes for EN and AR content)
- **Ollama Embeddings** (local embedding generation)
- **Google Gemini** (LLM for chat responses)
- **Google Drive** (source document ingestion)
- **Chat Trigger** (conversational entry point)

## 🔄 How It Works
1. **Trigger** — A chat message comes in via the n8n Chat Trigger (or a Manual Trigger for testing).
2. **Language Detection** — A Code node inspects the incoming text and detects whether it's English or Arabic.
3. **Routing** — An If node branches the flow based on detected language.
4. **Retrieval** — Each branch's AI Agent queries its own Pinecone vector store (populated from documents downloaded via Google Drive and embedded with Ollama) to fetch relevant context.
5. **Response Generation** — The matched-language AI Agent (powered by Google Gemini) generates a grounded response using the retrieved context.
6. **Document Ingestion** (separate branch) — Source files are downloaded from Google Drive, text is extracted, and content is embedded and upserted into Pinecone for future retrieval.

## 📦 Setup
1. Import `rag_bilingual_agent.json` into your n8n instance.
2. Connect credentials for:
   - Pinecone
   - Google Gemini API
   - Google Drive OAuth2
   - Ollama (local or hosted instance)
3. Create separate Pinecone namespaces/indexes for English and Arabic content, and update the vector store nodes accordingly.
4. Replace placeholder Google Drive file IDs with your own source documents.
5. Run the ingestion branch once to populate both vector stores, then activate the chat-facing branch.

> Replace all placeholder values (file IDs, index names) with your own before running.

## 📈 Impact
Enables a single support/chat workflow to serve English and Arabic-speaking users with contextually accurate, document-grounded answers — no duplicate bots required.

## 📄 License
MIT — see [LICENSE](./LICENSE)
