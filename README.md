# Spring 2025 · CUS 635 Mini-Projects  
**Team 1 — Benjamin Hanim, Peter Roumeliotis, Giulio Bardelli**

| Notebook | Goal | Key artifact |
|-----------|------|--------------|
| **`mini_project_1.ipynb`** | News-feed scraping & basic EDA | Raw JSON in S3 |
| **`mini_project_2.ipynb`** | Vector-store builder (⇢ Pinecone) | `cus635 / Team_1` namespace, 1 024-dim vectors |
| **`mini_project_3.ipynb`** | RAG + LangGraph React agent | Colab demo that answers Finance-news questions |

---

## Quick-Start (Colab)

1. **Fork or clone** this repo and open the desired notebook in Google Colab (the “Open in Colab” badge works too).  
2. In Colab: **Runtime ▸ Change runtime type → GPU (optional)**.  
3. Click the pad-lock in the left sidebar and **add two secrets**  
   - `OPENAI_API_KEY`  
   - `PINECONE_API_KEY`  
4. **Runtime ▸ Run all**.  
   - *If you only need to test the agent*, you can skip Mini-Projects 1 & 2 and jump straight to **mini_project_3.ipynb** because the vectors are already hosted in our shared Pinecone index.

> **Note:** GitHub strips widget metadata, so demo-cells in *mini_project_3* look blank in the repo.  
> After you press **Run all** the answers will render live (see Cell 8).

---

## Project 2 · Vector Ingestion Pipeline  (`mini_project_2.ipynb`)
*Rebuild only if you want to regenerate vectors.*

| Step | Description |
|------|-------------|
| **1. Install deps** | `pinecone==6.0.2`, `boto3`, etc. |
| **2. Fetch JSON sources from S3** | Bucket: `cus635-spring2025`, Folder: `TEAM_1/sources/` |
| **3. Process & chunk** | Merge *title*, *description*, *content* → `text`. |
| **4. Create dummy embeddings** | For the class exercise we seeded 1 024-dim uniform vectors so they align with the professor’s index shape. |
| **5. Upsert** | Namespace `Team_1`, Index `cus635`. 137 articles in Finance uploaded successfully. |

Running the notebook will print upload progress and a few sample queries to confirm the namespace holds data.

---

## Project 3 · RAG Agent  (`mini_project_3.ipynb`)

| Layer | Tech | Notes |
|-------|------|-------|
| Embeddings | *thenlper/gte-large* (HF, 1 024-dim) | Matches index shape without extra cost. |
| VectorStore | `langchain_pinecone.PineconeVectorStore` | Re-uses `cus635` index & `Team_1` namespace. |
| RAG core | `ConversationalRetrievalChain` | Prompt injects Finance filter. |
| Agent | `LangGraph` ➜ ReAct DAG | One tool: **`search_finance_news`** (wraps `rag.query`). |
| LLM | `gpt-3.5-turbo` | temp 0.2 for answers, 0.1 for reasoning. |

### Demo (Cell 8)
Five canned questions illustrate retrieval + reasoning:

