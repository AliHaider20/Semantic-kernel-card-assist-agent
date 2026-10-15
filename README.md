# CardAssist Agent

A small Streamlit-based assistant that helps with credit card tasks (activate/deactivate) and answers card-related questions using retrieval-augmented generation (RAG) over PDF guides.

## Quick highlights
- Activate / deactivate cards (demo functions via Semantic Kernel plugin).
- RAG QA: upload a PDF guide, it is chunked, embedded, and indexed with FAISS for contextual answers.
- Demo notebook (CardAssist.ipynb) shows embedding and indexing steps.

## Requirements
Install dependencies:
```bash
pip install -r requirements.txt
```

## Environment
Set these environment variables (see fake_env.env for examples):
- AI_FOUNDRY_MODEL_API — API key for LLM service
- LLM_ENDPOINT — Azure OpenAI chat endpoint for the deployment (e.g. gpt-4o-mini)
- EMBEDDING_ENDPOINT — optional if using remote embeddings
- AI_SEARCH_API — optional search service key

## Run (Streamlit)
```bash
# from repository root
export $(cat fake_env.env | xargs)   # or set vars manually
pip install -r requirements.txt
streamlit run CardAssist.py
```

Notes:
- Upload a PDF (e.g., global_card_access_user_guide.pdf) in the sidebar, click “Load PDF” to create embeddings and FAISS index.
- Card activation/deactivation are implemented as kernel functions (placeholders that return confirmation messages).

## System design (high level)
- UI: Streamlit app (CardAssist.py) — handles file upload, chat UI, and quick action buttons.
- Ingestion: PyPDFLoader → MarkdownTextSplitter (chunks) → SentenceTransformers (all-MiniLM-L6-v2) → embeddings matrix.
- Indexing: FAISS IndexFlatL2 holds embeddings for nearest-neighbor retrieval.
- Kernel & LLM: semantic_kernel Kernel with AzureChatCompletion (LLM) uses FunctionChoiceBehavior to call plugin methods or respond using RAG context.
- Plugin: CreditCardPlugin exposes async kernel functions: activate_card, deactivate_card, rag_query (returns concatenated relevant chunks).
- Notebook: CardAssist.ipynb demonstrates embedding, indexing, and sample flows.

## File map
- CardAssist.py — main Streamlit app and core logic
- CardAssist.ipynb — interactive demo and tests
- requirements.txt — Python deps
- global_card_access_user_guide.pdf — sample PDF used for RAG
- fake_env.env — example environment variables

## License
MIT

## Contact
Owner: AliHaider20
