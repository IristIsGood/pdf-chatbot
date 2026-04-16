# Chat with PDF: Agentic RAG Application 📄🤖
An end-to-end Retrieval-Augmented Generation (RAG) application that allows users to upload PDF documents and engage in context-aware conversations. Built with **LangChain**, **OpenAI**, and **Streamlit**, this project transforms static documents into interactive knowledge bases.

## 🎯 Problem & Solution
* **Problem:** Large PDF documents (manuals, books, reports) are difficult to navigate and search for specific information. Standard LLMs cannot "see" your local files, leading to generic or hallucinated answers.
* **Solution:** A **Context-Injected Chat Interface**. This app reads your PDF, splits it into semantic chunks, and stores it in a high-speed vector database (**FAISS**). When you ask a question, the app retrieves only the most relevant sections to "ground" the LLM's response in reality.



## 🏗️ Technical Architecture & Pipeline
This project implements a sophisticated RAG lifecycle optimized for web performance:

1. **Document Loading:** Uses `PyPDFLoader` to parse raw binary data into standard LangChain Document objects, retaining page-level metadata for potential citations.
2. **Intelligent Chunking:** Implements `RecursiveCharacterTextSplitter` ($500$ chars, $50$ overlap) which respects natural language boundaries (paragraphs/sentences) to prevent semantic fragmentation.
3. **Semantic Vector Space:** Utilizes `OpenAIEmbeddings` to project text into $1536$-dimensional vector space.
4. **Local Vector Storage:** Leverages **FAISS** (Facebook AI Similarity Search) for in-memory vector storage, providing millisecond-level similarity search without the need for an external database server.
5. **Stateful UI (Streamlit):**
    * **Execution Model:** Optimized using `@st.cache_resource` to ensure the PDF is only embedded once per upload.
    * **Session Management:** Uses `st.session_state` to maintain a persistent chat history, enabling a multi-turn conversational experience.

## ✨ Key Features
* **Bilingual Documentation:** Integrated English/Chinese technical notes and interview preparation questions directly in the codebase.
* **Streamlit Web UI:** A clean, professional interface with file uploaders, loading spinners, and styled chat bubbles.
* **Deterministic Intelligence:** Configured with `temperature=0` to ensure factual, non-creative accuracy essential for document analysis.
* **Zero-Footprint Storage:** Designed for privacy; the document is processed in memory/temp files and is not permanently stored.

## 🛠️ Tech Stack
* **Framework:** LangChain (`RetrievalQA`)
* **Frontend:** Streamlit
* **Intelligence:** OpenAI (`gpt-4o-mini`)
* **Vector DB:** FAISS (Local)
* **Environment:** Python 3.10+, `python-dotenv`

## 🚀 Getting Started

### 1. Installation
```bash
pip install streamlit langchain langchain-openai faiss-cpu pypdf python-dotenv
```

### 2. Configuration
Create a `.env` file in the root directory:
```env
OPENAI_API_KEY=sk-your-key-here
```

### 3. Usage
**Run the Web App:**
```bash
streamlit run app.py
```
*Upload a PDF in the sidebar and start chatting in the main window.*

## 🔑 Key Technical Decisions
* **Caching Strategy:** Used `@st.cache_resource` to store the entire LangChain pipeline. This prevents expensive API calls to OpenAI on every UI interaction, making the app feel "instant" for the user.
* **Recursive Splitting:** Chose `RecursiveCharacterTextSplitter` over simple character splitting to ensure that context (like "risk management" or "trading game") is not cut in half, preserving the semantic integrity of the vector search.
* **Local Temp Handling:** Implemented `tempfile.NamedTemporaryFile` to bridge the gap between Streamlit's in-memory file bytes and LangChain's requirement for a file path.

## 🛡️ License
MIT

## 👤 Developer
**Irist** – Building bridges between static data and AI intelligence.
