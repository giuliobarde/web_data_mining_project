# Spring 2025 · CUS 635 Mini-Projects  
**Team 1 — Benjamin Hanim, Peter Roumeliotis, Giulio Bardelli**

| Notebook | Goal | Key artifact |
|-----------|------|--------------|
| **`mini_project_1.ipynb`** | Scrape last-month news via News API & upload raw JSON to S3 | 137 source files in `s3://cus635-spring2025/TEAM_1/sources/` |
| **`mini_project_2.ipynb`** | Build 1 024-dim vectors & upsert to Pinecone | Namespace `Team_1` in index `cus635` |
| **`mini_project_3.ipynb`** | LangChain / LangGraph RAG agent over Finance news | Colab demo that answers live queries |

---

## Quick-Start (Colab)

1. **Open any notebook** with the *Open in Colab* badge.  
2. In Colab sidebar ▸ **Secrets**, add  
   * `OPENAI_API_KEY`  
   * `PINECONE_API_KEY`  
3. **Runtime ▸ Run all**.  
   *If you only want to see the agent*, jump straight to **Mini-Project 3**—vectors are already hosted.

> ℹ️ **Why do demo cells look blank on GitHub?**  
> We cleared all widget outputs before committing to avoid  
> `There was an error rendering your Notebook: the 'state' key is missing from 'metadata.widgets'`.  
> After you press **Run all**, Cell 8 in *mini_project_3* will display full answers.

---

## Project 1 – News Scraper (`mini_project_1.ipynb`)

| Step | Details |
|------|---------|
| API | News API (`sport`, `bitcoin`, `government` queries; 30-day window) |
| Output | 137 articles, grouped by **source**; each saved as `<Source>.json` |
| Storage | Uploaded via unsigned Boto3 to `cus635-spring2025/TEAM_1/sources/` |

> **Rerun guide:** set your own `news_key`, change `TEAM`, run the notebook—files will appear in your S3 folder.

---

## Project 2 – Vector Ingestion (`mini_project_2.ipynb`)

1. **Fetch** the JSON files from S3.  
2. **Process** each article → single `text` field (`title + description + content`).  
3. **Embed** with dummy seeded 1 024-dim vectors (class exercise alignment).  
4. **Upsert** to Pinecone → 137 vectors in `cus635 / Team_1`.  
5. **Sanity check** sample queries (`stock market`, `investment trends`, …).

---

## Project 3 – RAG Agent (`mini_project_3.ipynb`)

| Layer | Tech | Notes |
|-------|------|-------|
| Embeddings | `thenlper/gte-large` (HF, 1 024-dim) | matches index dims |
| VectorStore | `langchain_pinecone.PineconeVectorStore` | Finance-only filter |
| RAG Core | `ConversationalRetrievalChain` | simple prompt template |
| Agent | LangGraph ReAct | single tool `search_finance_news` |
| LLM | GPT-3.5-Turbo | temp 0.2 answers, 0.1 reasoning |

### Demo Queries (Cell 8)

