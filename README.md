## Model‑Aware Tokenizer (The Tokenator)

### A multi‑format text extractor, model‑aware tokenizer, and chunk‑analysis engine.

---

This project is provided for portfolio review only. No permission is granted for reuse, modification, or redistribution.

---

Overview

Tokenator is a full‑stack tool for analyzing token counts across multiple LLM models. It accepts a wide range of file formats, extracts text using best‑effort converters, and evaluates token usage using:

- tiktoken (preferred, exact)
- gpt‑3‑encoder (fallback for older GPT‑2/Davinci models)
- heuristic estimation (when no encoder is available)

It includes:

- a browser UI
- a Node.js backend
- a worker‑thread engine for large files
- an Electron wrapper for desktop use
- a local runner with automatic browser lifecycle
- a test harness for validation

This project demonstrates systems thinking, multi‑format parsing, concurrency, and model‑aware tokenization logic.

---

Features

🧩 Multi‑Format Extraction

Tokenator extracts text from:

- TXT / MD
- HTML / XHTML
- PDF
- DOCX
- PPTX
- EPUB / ZIP archives
- CSV / JSON
- Images (PNG/JPG/WebP) via optional OCR
- Binary files (base64 fallback)

Extraction is best‑effort and capped for safety.

🧠 Model‑Aware Tokenization

Supports:

- GPT‑4 / GPT‑4o
- GPT‑3.5 variants
- Davinci / GPT‑2 encodings
- Gemini models
- Gemma models
- Custom model specs (model:encoding:context or model|encoding|context)

Tokenization uses:

1. Exact tiktoken encoding when available
2. Exact GPT‑3‑encoder for older models
3. Heuristic fallback using chars‑per‑token ratios

📦 Chunking & Context Analysis

For each model:

- token count
- context window
- chunks needed
- optional per‑chunk previews

⚡ Worker‑Thread Acceleration

Large files (>2MB) automatically route through a worker thread:

- file written to disk
- worker processes text
- results returned asynchronously
- file cleaned up

🖥️ Electron Desktop App

A lightweight GUI wrapper:

- starts the server
- opens a dedicated browser window
- shuts down the server when the window closes

🧪 Test Harness

A simple test script validates:

- upload endpoint
- JSON structure
- snippet token count
- model count entries

---

Architecture

    [Upload]
        ↓
    [Extraction Pipeline]
        ↓
    [Tokenizer Layer]
        ↓
    [Chunker]
        ↓
    [Response JSON]

Extraction Pipeline

- Detects file type
- Routes to appropriate extractor
- Applies OCR if enabled
- Caps extracted text to 5MB

Tokenizer Layer

- Parses model specs
- Selects encoder
- Computes token counts
- Computes chunks needed

Worker Path

Used for large files:

    [Disk Write] → [Worker Thread] → [Token Counts] → [Cleanup]

---

Repository Structure

    tokenator/
        server.js
        tokenWorker.js
        electron-main.js
        run_local.sh
        public/
            index.html
            script.js
        test/
            run_tests.sh
        uploads/            (ignored)
        server.out          (ignored)
        server.pid          (ignored)
        package.json
        INSTALL.md
        README.md

---

Usage

Install
`
npm install
`

Optional:
`
npm install tiktoken
npm install gpt-3-encoder
npm install tesseract.js
`

Run the server
`
npm start
`
Open:  
http://localhost:3000

Run with Electron
`
npm run run:gui
`

Run locally with auto‑shutdown
`
npm run run:local
`

---

API

POST /upload
Multipart form‑data:

- file — uploaded file
- models — comma‑separated model specs
- enableOcr=true
- expandArchives=true
- chunkSize=2000
- useWorker=true

Returns JSON:

- filename
- size
- ext
- derivedFrom
- snippet
- snippetTokens
- tokenCounts (per model)

---

Security Notes

- No authentication — do not deploy publicly
- Upload limit: 50MB
- Extracted text capped at 5MB
- Files processed in memory or temporary disk
- No persistent storage

---

Why This Project Matters

The Tokenator demonstrates:

- full‑stack engineering
- multi‑format parsing
- concurrency and worker threads
- model‑aware logic
- Electron desktop integration
- robust error handling
- developer‑friendly UX
- testability and reproducibility

It’s a practical tool with real engineering depth — the kind of project that shows you can design, build, and ship systems that solve real problems.
