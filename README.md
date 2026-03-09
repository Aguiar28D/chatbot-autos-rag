# 🚗 Used Car Search Chatbot with RAG

A conversational AI chatbot that enables semantic search over a dataset of **38,531 used vehicle listings** using Retrieval Augmented Generation (RAG). Built as a final project for a Deep Learning postgraduate course (UTEC + MIT).

**Team:** Nicolas Fripp · Diego Aguiar · Bruno Moraes

---

## 🧠 What It Does

Traditional car search relies on exact keyword matching — if you type "family car" you might miss relevant SUVs just because you didn't type the right word. This chatbot understands natural language intent:

> *"Show me affordable SUVs under $10,000"*
> *"Compare Honda vs Mazda options"*
> *"Which of those has the best mileage?"* ← remembers previous context

---

## 🏗️ Architecture

```
User Query
    │
    ▼
OpenAI Embeddings (text-embedding-3-small)
    │
    ▼
FAISS Vector Store ──► Top 5 similar vehicles
    │
    ▼
GPT-4o-mini + Conversation History
    │
    ▼
Natural Language Response
```

Each vehicle is converted to a natural-language description before embedding:
> *"Toyota RAV4 2015. Price: $14,500 USD. Engine: gasoline 2.5L. Transmission: automatic. Mileage: 85,000 km. Body type: SUV. Color: white. Drivetrain: all."*

---

## ⚙️ Tech Stack

| Component | Technology |
|-----------|-----------|
| Embeddings | OpenAI `text-embedding-3-small` |
| Vector Store | FAISS (CPU, persisted to disk) |
| LLM | GPT-4o-mini |
| Orchestration | LangChain |
| Data | Pandas |
| Environment | Google Colab / Python |

---

## 📁 Files

| File | Description |
|------|-------------|
| `chatbot_autos_rag.ipynb` | Main notebook — full RAG pipeline |
| `Deep_Learning_Final_Pitch.pptx` | Final project presentation |

---

## 🚀 Getting Started

### 1. Install dependencies
```bash
pip install langchain langchain-openai langchain-community langchain-core faiss-cpu pandas
```

### 2. Set your OpenAI API key
```bash
export OPENAI_API_KEY="your-key-here"
```
In Google Colab, use the **Secrets** panel (key icon in sidebar).

### 3. Add your dataset
The notebook expects a CSV with used vehicle listings. Update `DATASET_PATH` in the notebook to point to your file. Required columns:
`manufacturer_name`, `model_name`, `year_produced`, `price_usd`, `engine_fuel`, `engine_capacity`, `transmission`, `odometer_value`, `body_type`, `color`, `drivetrain`, `state`

### 4. Run the notebook
The FAISS index is built on first run and saved to disk — subsequent loads are instant.

---

## 💬 Example Queries

```
"Show me Toyota SUVs"
"I need a family car under $10,000"
"Compare Honda and Mazda options"
"Which of those is the cheapest?"   ← follows conversation context
"Does it have a warranty?"          ← remembers previous answer
```

---

## ✅ Key Features

- **Semantic search** — finds relevant results without exact keyword matches
- **Conversational memory** — follow-up questions work naturally
- **Persistent vector store** — FAISS index saved to disk, no re-embedding needed
- **Cost-efficient** — one embedding + one LLM call per query
- **Direct similarity search** — inspect raw FAISS results without LLM

---

## 📊 Why RAG?

| Approach | Training needed | Knowledge updatable | Cost |
|----------|----------------|---------------------|------|
| Fine-tuning | ✅ Yes | ❌ No (retrain) | High |
| **RAG** | ❌ No | ✅ Yes (update store) | Low |
| Prompt stuffing | ❌ No | ✅ Yes | Very high per query |

RAG gave us the best tradeoff: no training required, easy to update the vehicle catalog, and low cost per query.

---

## 📄 Presentation

View the full project pitch: [`Deep_Learning_Final_Pitch.pptx`](./Deep_Learning_Final_Pitch.pptx)
