# 🎙️ Voice2Query: AI-Powered Speech-to-SQL

> **Interactive Database Exploration through Voice Commands**
>
> A cascaded pipeline that transcribes speech → text → SQL → executes on a database → visualizes results.

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python)
![Streamlit](https://img.shields.io/badge/Streamlit-1.30+-red?logo=streamlit)
![SQLite](https://img.shields.io/badge/SQLite-3-green?logo=sqlite)
![Groq](https://img.shields.io/badge/Groq-Llama--3.3--70B-purple?logo=groq)
![Whisper](https://img.shields.io/badge/Whisper-Local-orange)

---

## 🏗️ Architecture

```
┌──────────┐     ┌─────────────┐     ┌──────────────┐     ┌───────────┐     ┌─────────────┐
│  🎤 Audio │ ──▸ │ ASR Module  │ ──▸ │   Error      │ ──▸ │ Text-to-  │ ──▸ │  Execute &  │
│  Input    │     │ (Whisper)   │     │   Correction │     │SQL (Groq) │     │  Visualize  │
└──────────┘     └─────────────┘     └──────────────┘     └───────────┘     └─────────────┘
                                            ▲                    ▲
                                            │                    │
                                      ┌─────┴────────────────────┴─────┐
                                      │     📋 Database Schema         │
                                      │     (SQLite University DB)     │
                                      └────────────────────────────────┘
```

## 📁 Project Structure

```
voice2query/
├── config.py                     # Central configuration
├── requirements.txt              # Dependencies
├── .env.example                  # API key template
├── README.md                     # This file
│
├── database/
│   ├── schema.sql                # DDL: 6 tables
│   ├── seed_data.sql             # 250+ rows of mock data
│   ├── setup_db.py               # Initialize the database
│   └── connection.py             # SQLAlchemy + schema introspection
│
├── modules/
│   ├── asr/
│   │   └── transcriber.py        # Whisper speech-to-text
│   ├── text_to_sql/
│   │   ├── schema_prompt.py      # Schema-aware prompt builder
│   │   └── generator.py          # LLM-based NL → SQL
│   ├── error_correction/
│   │   └── corrector.py          # DB-aware fuzzy correction
│   └── executor/
│       └── query_runner.py       # Safe SQL execution
│
├── dashboard/
│   └── app.py                    # Streamlit UI
│
├── audio_samples/                # Sample audio files
│
└── tests/                        # Pytest test suite
    ├── test_db.py
    ├── test_asr.py
    ├── test_text_to_sql.py
    ├── test_executor.py
    └── test_correction.py
```

## 🚀 Quick Start

### 1. Clone & Install

```bash
cd voice2query
pip install -r requirements.txt
```

### 2. Configure API Key

```bash
cp .env.example .env
# Edit .env and add your OpenAI API key
```

### 3. Initialize Database

```bash
python database/setup_db.py
```

### 4. Launch Dashboard

```bash
streamlit run dashboard/app.py
```

## 🧪 Run Tests

```bash
python -m pytest tests/ -v
```

## 📊 Database Schema

**University Database** — 6 tables, 250+ rows:

| Table | Rows | Description |
|-------|------|-------------|
| `departments` | 8 | Academic departments |
| `professors` | 20 | Faculty members |
| `students` | 50 | Enrolled students |
| `courses` | 30 | Course catalog |
| `enrollments` | 150 | Student-course enrollments |
| `scholarships` | 20 | Financial awards |

**Supported query patterns**: JOINs, aggregations, subqueries, GROUP BY + HAVING, multi-table joins.

## 🔧 Module Details

### Task 1: ASR Module (`modules/asr/`)
- Uses OpenAI Whisper (local, `base` model)
- Supports .wav, .mp3, .m4a, .webm, .flac
- Confidence scoring & silence detection
- No API key needed — runs entirely on CPU

### Task 2: Text-to-SQL Module (`modules/text_to_sql/`)
- Uses Groq Llama 3.3 70B (free tier, OpenAI-compatible API) with schema-aware prompting
- Dynamic DDL injection + few-shot examples
- Safety validation: SELECT-only enforcement
- SQL cleaning (removes markdown fences)

### Task 3: Dashboard (`dashboard/`)
- 3 input modes: voice recording, file upload, text input
- Step-by-step pipeline visualization
- Auto-chart generation with Plotly
- Query history tracking

### Task 4: Error Correction (`modules/error_correction/`)
- Domain dictionary for common ASR errors
- Fuzzy matching against DB terms (table/column names)
- Multi-word value matching (department names, etc.)

## 👥 Team

| Member | Task |
|--------|------|
| Member 1 | Database Schema & Setup |
| Member 2 | ASR Module (Whisper) |
| Member 3 | Text-to-SQL (LLM) |
| Member 4 | Dashboard & Error Correction |

## 📄 License

Academic project — Data Management, Spring 2026.
