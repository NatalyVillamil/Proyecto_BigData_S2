# Copilot / AI Agent Instructions for Proyecto_BigData_S2

Purpose: Help AI coding agents become productive quickly by pointing to this repository's architecture, key files, developer workflows, and notable conventions discovered in the codebase.

**Big Picture**
- **Stack & intent:** This repo is a Python-based Big Data / NLP pipeline and web application (see `requirements.txs`). Key libraries: `Flask`, `gunicorn`, `pymongo`, `elasticsearch`, `spacy`, `transformers`, `sentence-transformers`, `pandas`, `numpy`, PDF/ocr tools (`PyPDF2`, `pytesseract`, `pdf2image`, `Pillow`), and scraping libs (`beautifulsoup4`, `lxml`). Treat it as a service that ingests documents, extracts text/data, and indexes or stores results.
- **Storage & search:** `pymongo` implies MongoDB used for structured storage; `elasticsearch` indicates a search/indexing component. Expect environment-provided connection strings.

**Where to start (discovery checklist)**
- Read `README.md` (project description and author info). It is minimal — expand if you find other docs.
- Inspect any file that creates a Flask app (search for `Flask(`, `app = Flask`, or `if __name__ == "__main__":`). That is likely the application entrypoint.
- Search the repo for `pymongo`, `elasticsearch`, `gunicorn`, `spacy`, `transformers` to find integration points and ingestion pipelines.

**Developer workflows & useful commands (Windows PowerShell)**
- Create and activate a virtual environment and install dependencies (note: dependency file is named `requirements.txs` in this repo):

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txs
```

- Run the app during development:
  - If there is an `app.py` or `main.py` with a Flask app, run `python app.py` or `python main.py`.
  - Otherwise set `FLASK_APP` to the application module and run the Flask CLI:

```powershell
$env:FLASK_APP = 'module_name'  # replace module_name
python -m flask run
```

- Production run (example using `gunicorn`, replace module:name):

```powershell
gunicorn -w 4 "module:app"
```

**Search / inspection shortcuts (useful for agents)**
- Quick PowerShell search for key imports and entrypoints:

```powershell
Select-String -Path * -Pattern "Flask\(|pymongo|elasticsearch|gunicorn|spacy|transformers|if __name__ == \"__main__\"" -SimpleMatch -List
```

**Conventions & gotchas discovered in this repo**
- The dependency file is named `requirements.txs` (not `.txt`). Do not assume `requirements.txt` exists; use the actual filename when installing or updating dependencies.
- The repo contains heavy NLP/ML packages. When iterating quickly, prefer editing non-ML code without installing large models; for model work, document required model downloads and GPU/CPU expectations.
- `python-dotenv` is present: expect a `.env` file for secrets/connection strings. Typical keys to look for: `MONGO_URI`, `ELASTIC_HOST`, `SECRET_KEY`, and any API keys used for scraping.

**Integration points to verify before changing code**
- Database connections: find the code that constructs `MongoClient(...)` or uses `pymongo` and confirm which env var it reads.
- Elasticsearch: locate `Elasticsearch(...)` usage and confirm client configuration (version-specific clients may need particular host/url formats).
- Background ingestion or worker scripts: search for scripts that call OCR (`pytesseract`), `pdf2image`, or `beautifulsoup4` to understand long-running tasks.

**If you change dependencies**
- Update `requirements.txs` and include rationale in the commit message. If you rename it to `requirements.txt`, note that CI or collaborators may expect the original filename — confirm with the team.

**When to ask the human**
- If an entrypoint (Flask app module) or `.env` keys are unclear, ask the repo owner for the expected app module name and environment variables.
- If refactoring touches Elasticsearch or Mongo schemas, ask for sample data or a dev instance to avoid breaking queries/index mappings.

Next step for agents: run the repository search commands above, then open the top files referencing `Flask`, `pymongo`, and `elasticsearch` to map the actual module paths for run commands and updates.

---
If anything in this guidance is unclear or you want me to include specific file references (for example, point to the exact Flask entrypoint once it's located), tell me and I will update this file.
