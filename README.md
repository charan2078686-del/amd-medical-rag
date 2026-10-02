# AMD Medical RAG & Prescription Scanner

An efficient, containerized Retrieval-Augmented Generation (RAG) system optimized for AMD ROCm environments. This system processes and queries structured clinical data, including diagnostic lab reports and prescription records, extracting medication dosages, frequencies, and patient metrics with source citations.

## Architecture

- **Client-Server Design:** Communication between the CLI and the inference engine occurs via UNIX domain sockets (`/tmp/rag_service.sock`) for low-latency IPC.
- **Server (`server.py`):** Acts as the background daemon, managing document indexing, entity extraction, and query handling.
- **Client (`app.py`):** Lightweight command-line interface providing `--index` and `--query` commands.
- **Data Corpus (`corpus/`):** Contains tabular medical records, including diagnostic reports and prescription details.
- **Evaluation Outputs (`output/`):** Generates structured JSON responses with explicit citations and confidence scores.

## Setup & Local Usage

1. **Start the background service:**
   ```bash
   python3 server.py
python3 app.py --index corpus
python3 app.py --corpus corpus --query-id rx1 --query "What is the dosage for Amoxicillin?"

docker build -t amd-medical-rag .
docker run -d --name medical-rag amd-medical-rag
