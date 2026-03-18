# finpersoai-backend

Backend scaffold for FinPersoAI based on FastAPI.

## Project structure

- `app/`: API, core config, security, database, services, repositories, workers
- `ai/`: ML and scoring modules
- `rag/`: ingestion, retrieval, vector store, assistant flows
- `data/`: raw, processed, external, sample datasets
- `artifacts/`: trained models and generated outputs
- `scripts/`: automation scripts
- `tests/`: test suite
- `docs/`: technical documentation

## Key documentation

- `CAHIER_DES_CHARGES_IA.md`: operational AI requirements and governance reference for the team

## Quick start

```bash
python -m venv .venv
.venv\\Scripts\\activate
pip install -r requirements.txt
uvicorn app.main:app --reload
```

## First endpoint

- `GET /health`
