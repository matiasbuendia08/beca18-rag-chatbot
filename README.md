# Beca 18 RAG Chatbot — PRONABEC 2026

> **Course:** Python Programming — Applied Data Science  
> **Author:** Matias Buendia  
> **Source:** Resolución Directoral Ejecutiva N.° 033-2026-MINEDU/VMGI-PRONABEC

---

## Purpose

This project builds an end-to-end Retrieval-Augmented Generation (RAG) pipeline 
that answers user questions about the official Beca 18 regulations by retrieving 
relevant fragments from the source PDF and passing them as context to a large 
language model. The system never relies on the model's parametric knowledge and 
declines to answer when the information is not present in the document.

## Source Document

**Resolución Directoral Ejecutiva N.° 033-2026-MINEDU/VMGI-PRONABEC**  
Available at: https://www.gob.pe/institucion/pronabec/normas-legales/7778068-033-2026-minedu-vmgi-pronabec

---

## Pipeline Summary

The pipeline extracts text from the 138-page PDF regulation document, inserts 
page markers, and splits the content into 400-token chunks with 60-token overlap 
using LangChain's RecursiveCharacterTextSplitter. Each chunk is embedded using 
gemini-embedding-001 (3072 dimensions) with separate task types for indexing 
and retrieval. Embeddings are stored in a persistent ChromaDB collection using 
cosine distance with idempotent indexing to avoid redundant API calls. At query 
time, the user's question is embedded and the top-k nearest chunks are retrieved 
and passed as context to gemini-2.5-flash, which generates a grounded answer 
citing page numbers and refusing to hallucinate when context is insufficient.

---

## Repository Structure

    beca18-rag-chatbot/
    ├── data/
    │   └── beca18_reglamento.pdf
    ├── notebooks/
    │   └── beca18_rag_chatbot.ipynb
    ├── .env.example
    ├── .gitignore
    ├── requirements.txt
    └── README.md
---

## Installation and Setup

**1. Install dependencies:**
```bash
pip install -r requirements.txt
```

**2. Configure API key:**
```bash
cp .env.example .env
# Edit .env and add your Gemini API key from https://aistudio.google.com/app/apikey
```

**3. Download the source PDF** from the link above and place it in `data/` as:

data/beca18_reglamento.pdf

---

## How to Run

1. Open the notebook in Google Colab:  
   **File → Open notebook → GitHub → paste repo URL**

2. Add your `GEMINI_API_KEY` to Colab Secrets (🔑 icon in left panel)

3. Run all cells top to bottom: **Runtime → Run all**

---

## How to Use the Chat Interface

The notebook includes an interactive chat interface built with ipywidgets:

- **Text box** — type your question about Beca 18
- **k slider** — control how many chunks are retrieved (1–10)
- **"Preguntar" button** — submit your question
- **"Limpiar" button** — clear the chat history
- **Expandable accordion** — shows the source fragments with page numbers

---

## Main Findings

The RAG pipeline successfully retrieves relevant fragments from the 138-page 
regulation and generates grounded answers citing specific page numbers. The 
system correctly refuses to answer off-topic questions, confirming the strict 
system prompt prevents hallucination. Due to free-tier API limits, 280 chunks 
from key pages were indexed, covering eligibility requirements, scholarship 
modalities, student obligations, and conditions for losing the scholarship.
