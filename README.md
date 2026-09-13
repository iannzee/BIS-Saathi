# 🇮🇳 BIS Saathi — AI-Powered Intelligent Assistant for Indian Standards & BIS Services

> **Smart India Hackathon 2025 | Problem Statement ID: 26107**
> Ministry of Consumer Affairs, Food & Public Distribution | Department of Consumer Affairs (DoCA)
> Category: Software | Theme: Smart Automation

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Problem Statement](#-problem-statement)
- [Our Solution](#-our-solution)
- [Key Features](#-key-features)
- [System Architecture](#-system-architecture)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Environment Variables](#-environment-variables)
- [Team](#-team)
- [Roadmap](#-roadmap)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🌟 Overview

**BIS Saathi** is an AI-powered conversational assistant that provides instant, accurate, and source-backed information related to Indian Standards (IS) and Bureau of Indian Standards (BIS) services. Built using Retrieval-Augmented Generation (RAG), every response is grounded in official BIS documents — no hallucinations, just verified answers with citations.

Whether you're an MSME trying to understand certification requirements, a consumer verifying a hallmark, or a student researching standards — BIS Saathi answers in plain language, in your language.

---

## ❗ Problem Statement

The Bureau of Indian Standards maintains **20,000+ Indian Standards** across sectors and offers services such as product certification, hallmarking, lab recognition, and consumer affairs. However:

- Information is scattered across **multiple PDFs, portals, and circulars**
- **MSMEs and startups** struggle to identify which IS standard applies to their product
- **Consumers** can't easily verify hallmarking or understand their rights
- **No multilingual access** — most small businesses aren't English-fluent
- Searching through documents is **time-consuming and technically demanding**

---

## 💡 Our Solution

BIS Saathi uses a **RAG (Retrieval-Augmented Generation)** pipeline to:

1. Accept user queries in **natural language** (text or voice)
2. **Detect intent** (standard lookup, certification guidance, hallmarking, lab finder, etc.)
3. **Retrieve** the most relevant chunks from our BIS knowledge base
4. **Generate** accurate, cited responses using an LLM
5. **Respond in the user's preferred Indian language**

```
User Query → Language Detection → Intent Classification
         → Vector Search (FAISS) → LLM Generation
         → Cited Response in User's Language
```

---

## ✨ Key Features

| Feature | Description |
|---|---|
| 📖 **Standard Lookup** | Find applicable Indian Standards by product description |
| 🏆 **Certification Guidance** | Step-by-step guidance on ISI, CRS, FMCS, and other BIS schemes |
| 🥇 **Hallmarking Info** | Explain BIS hallmarking process and how to verify jewellery |
| 🔬 **Lab Finder** | Locate BIS-recognized testing laboratories by state and sector |
| 📜 **Source Citations** | Every answer cites the specific IS standard, clause, and document |
| 🌐 **Multilingual Support** | Supports 10+ Indian languages via IndicTrans2 |
| 💬 **WhatsApp Bot** | Accessible via WhatsApp for MSME and rural outreach |
| 🛡️ **Zero Hallucination** | Responses generated only from verified BIS documents |

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────┐
│                   USER INTERFACES                   │
│       Web App  |  Mobile App  |  WhatsApp Bot       │
└──────────────────────┬──────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────┐
│               API GATEWAY (FastAPI)                 │
│        Auth | Rate Limiting | Session Mgmt          │
└──────────────────────┬──────────────────────────────┘
                       │
         ┌─────────────┼──────────────┐
         ▼             ▼              ▼
   ┌──────────┐  ┌──────────┐  ┌──────────┐
   │  Intent  │  │ Language │  │   Lab    │
   │Classifier│  │Translator│  │  Finder  │
   └────┬─────┘  └────┬─────┘  └──────────┘
        │              │
        ▼              ▼
┌────────────────────────────────────────────┐
│           RAG PIPELINE ENGINE              │
│   Vector Search → LLM → Citation Builder  │
└──────────────────┬─────────────────────────┘
                   │
       ┌───────────┼────────────┐
       ▼           ▼            ▼
  ┌─────────┐ ┌─────────┐ ┌──────────┐
  │  FAISS  │ │ BIS PDF │ │Standards │
  │ Vector  │ │ Corpus  │ │ Metadata │
  │   DB    │ │ Chunks  │ │    DB    │
  └─────────┘ └─────────┘ └──────────┘
```

---

## 🛠️ Tech Stack

### Backend
- **Python 3.11+**
- **FastAPI** — REST API framework
- **LangChain** — RAG pipeline orchestration
- **FAISS** — Vector similarity search
- **Sentence Transformers** — Text embeddings (`all-MiniLM-L6-v2`)
- **Gemini 1.5 Flash** — LLM for response generation (via Google AI API)
- **IndicTrans2** — Multilingual translation (22 Indian languages)

### Frontend
- **React.js** — Web application
- **Tailwind CSS** — Styling
- **Flutter** *(planned)* — Mobile application

### Data & Storage
- **PyMuPDF / pdfplumber** — PDF text extraction
- **ChromaDB** — Persistent vector store (alternative to FAISS)
- **PostgreSQL** — Structured metadata storage

### Infrastructure
- **Docker** — Containerization
- **GitHub Actions** — CI/CD
- **AWS / GCP Free Tier** — Deployment

### Integrations
- **Twilio / Meta Cloud API** — WhatsApp Bot
- **Google Maps API** — Lab Finder map view
- **BIS Official APIs** *(if available)* — Live data sync

---

## 📁 Project Structure

```
bis-saathi/
│
├── backend/                        # FastAPI backend
│   ├── main.py                     # Entry point
│   ├── routers/
│   │   ├── chat.py                 # /chat endpoint
│   │   ├── labs.py                 # /labs endpoint
│   │   └── standards.py            # /standards endpoint
│   ├── services/
│   │   ├── rag_pipeline.py         # Core RAG logic
│   │   ├── intent_classifier.py    # Query intent detection
│   │   ├── translator.py           # IndicTrans2 wrapper
│   │   └── citation_builder.py     # Source citation generator
│   ├── models/
│   │   └── schemas.py              # Pydantic models
│   └── requirements.txt
│
├── frontend/                       # React.js frontend
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   │   ├── ChatWindow.jsx
│   │   │   ├── MessageBubble.jsx
│   │   │   ├── LabFinder.jsx
│   │   │   └── LanguageSelector.jsx
│   │   ├── pages/
│   │   │   ├── Home.jsx
│   │   │   └── Admin.jsx
│   │   └── App.jsx
│   └── package.json
│
├── data/                           # BIS Knowledge Base
│   ├── raw_pdfs/                   # Downloaded BIS PDFs
│   ├── processed_text/             # Extracted text files
│   ├── chunks/                     # Chunked documents (JSON)
│   └── vector_store/               # FAISS index files
│
├── ml/                             # ML & Data Engineering
│   ├── data_pipeline/
│   │   ├── scraper.py              # BIS document downloader
│   │   ├── pdf_extractor.py        # PDF → text conversion
│   │   └── chunker.py              # Text chunking with metadata
│   ├── embeddings/
│   │   ├── embed_documents.py      # Generate & store embeddings
│   │   └── query_engine.py         # Vector search interface
│   └── notebooks/
│       └── rag_poc.ipynb           # Proof of concept notebook
│
├── whatsapp_bot/                   # WhatsApp integration
│   ├── bot.py
│   └── webhook.py
│
├── docs/                           # Documentation
│   ├── architecture.md
│   ├── api_reference.md
│   └── data_sources.md
│
├── .github/
│   └── workflows/
│       └── ci.yml                  # GitHub Actions CI/CD
│
├── docker-compose.yml
├── Dockerfile
├── .env.example
├── .gitignore
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.11+
- Node.js 18+
- Git
- Docker (optional)

### 1. Clone the Repository

```bash
git clone https://github.com/iannzee/bis-saathi.git
cd bis-saathi
```

### 2. Backend Setup

```bash
cd backend
python -m venv venv
source venv/bin/activate        # On Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 3. Build the Knowledge Base

```bash
cd ml/data_pipeline
python scraper.py               # Download BIS PDFs
python pdf_extractor.py         # Extract text from PDFs
python chunker.py               # Chunk text with metadata

cd ../embeddings
python embed_documents.py       # Generate FAISS vector index
```

### 4. Run the Backend

```bash
cd backend
uvicorn main:app --reload --port 8000
```

API will be live at: `http://localhost:8000`
Swagger docs at: `http://localhost:8000/docs`

### 5. Frontend Setup

```bash
cd frontend
npm install
npm start
```

Frontend will be live at: `http://localhost:3000`

### 6. Docker (Full Stack)

```bash
# From root directory
docker-compose up --build
```

---

## 🔐 Environment Variables

Create a `.env` file in the root directory. Refer to `.env.example`:

```env
# LLM API
GEMINI_API_KEY=your_gemini_api_key_here

# Database
POSTGRES_URL=postgresql://user:password@localhost:5432/bissaathi

# Translation
INDICTRANS_MODEL_PATH=./models/indictrans2

# WhatsApp
TWILIO_ACCOUNT_SID=your_twilio_sid
TWILIO_AUTH_TOKEN=your_twilio_auth_token
WHATSAPP_NUMBER=whatsapp:+14155238886

# Google Maps (Lab Finder)
GOOGLE_MAPS_API_KEY=your_maps_api_key

# App Config
DEBUG=True
ALLOWED_ORIGINS=http://localhost:3000
```

> ⚠️ **Never commit your `.env` file. It is listed in `.gitignore`.**

---

## 👥 Team

**Team Name:** *(Your Team Name)*
**SIH 2025 | PS ID26107**

| Name | Role | Responsibilities |
|---|---|---|
| **Iannzee** | 👑 Team Leader | Architecture, Coordination, Pitch |
| *(Member 2)* | 🤖 ML Engineer | RAG Pipeline, Embeddings, FAISS |
| *(Member 3)* | ⚙️ Backend Developer | FastAPI, Intent Classifier, API |
| *(Member 4)* | 🎨 Frontend Developer | React UI, Lab Finder, UX |
| *(Member 5)* | 📊 Data Engineer | BIS Scraping, PDF Processing, Chunking |
| *(Member 6)* | 🎤 Presenter | Pitch Deck, Demo Video, Documentation |

---

## 🗓️ Roadmap

### ✅ Phase 1 — Foundation (Day 1–2)
- [x] Project setup & repo structure
- [ ] BIS PDF collection (target: 50+ documents)
- [ ] PDF extraction pipeline
- [ ] Basic RAG proof of concept

### 🔄 Phase 2 — Core Engine (Day 2–3)
- [ ] FAISS vector store with metadata
- [ ] Intent classifier
- [ ] FastAPI `/chat` endpoint
- [ ] Source citation builder

### 🔄 Phase 3 — Frontend & Integration (Day 3–4)
- [ ] React chat UI
- [ ] Language selector (10 Indian languages)
- [ ] Lab Finder with map view
- [ ] Frontend ↔ Backend integration

### 🔄 Phase 4 — Polish & Demo (Day 4–5)
- [ ] Multilingual support via IndicTrans2
- [ ] WhatsApp bot (basic)
- [ ] Demo video recording
- [ ] Pitch deck finalization

---

## 🤝 Contributing

We follow **Git Flow** for this project.

```bash
# Always branch from dev
git checkout dev
git pull origin dev
git checkout -b feat/your-feature-name

# After work is done
git add .
git commit -m "feat: your descriptive commit message"
git push origin feat/your-feature-name

# Open a Pull Request → dev
# Never push directly to main
```

### Commit Message Convention

```
feat:     New feature
fix:      Bug fix
docs:     Documentation update
data:     Data pipeline changes
ml:       ML model or pipeline changes
refactor: Code refactoring
test:     Adding tests
```

---

## 📚 Data Sources

All knowledge base documents are sourced from official BIS channels:

- [BIS Official Website](https://www.bis.gov.in)
- [BISLINEt — Standards Database](https://www.bisnet.bis.gov.in)
- [MANAK Online Portal](https://www.manakonline.in)
- BIS Certification Scheme PDFs (ISI, CRS, FMCS, Hallmarking)
- Indian Standards (publicly available excerpts)
- BIS Consumer Affairs FAQs

---

## ⚖️ License

This project is built for **Smart India Hackathon 2025** under the problem statement by the **Ministry of Consumer Affairs, Food & Public Distribution**.

All BIS documents and Indian Standards referenced are the intellectual property of the **Bureau of Indian Standards**.

---

## 🙏 Acknowledgements

- **Bureau of Indian Standards (BIS)** for making standards accessible
- **IIT Madras** for the IndicTrans2 translation model
- **Smart India Hackathon 2025** organizing committee
- **Ministry of Consumer Affairs** for defining this critical problem

---

<div align="center">

**Built with ❤️ for India's 6.3 Crore MSMEs**

*Making Indian Standards accessible to every Indian*

</div>
