# Closira — Engineering Internship Assignment

Full-stack implementation of Closira's customer enquiry-handling platform.

## Structure

```
closira/
├── backend/   # FastAPI + Celery + PostgreSQL
├── frontend/  # React Native (Expo) + NativeWind
└── docker-compose.yml
```

## Quick Start

### Backend (Docker)
```bash
cp backend/.env.example backend/.env
docker-compose up --build
```
- API docs: http://localhost:8000/docs
- Flower (Celery monitor): http://localhost:5555

### Frontend
```bash
cd frontend
npm install
npx expo start
```
Scan the QR with the Expo Go app (iOS / Android).

## Tests
```bash
cd backend
pip install -r requirements.txt
pytest -v
```

## Architecture

See `Closira_Architecture.docx` for the full design document.

### Backend
- **FastAPI** REST API (non-blocking, async-dispatched)
- **Celery** background worker with Redis broker
- **PostgreSQL** with SQLAlchemy ORM + Alembic migrations
- **Structured JSON logging** via structlog
- **SOP Matching**: keyword-based rules for 5 scenarios

### Frontend
- **React Native** (Expo SDK 51)
- **NativeWind** (Tailwind CSS for RN) for consistent styling
- **React Navigation** v6 — Bottom Tabs + Stack
- **React Context + useReducer** for global state
- All data mocked from `/mock/*.json`

## API Endpoints

| Method | Endpoint                      | Description                     |
|--------|-------------------------------|---------------------------------|
| POST   | /enquiry                      | Create inbound enquiry          |
| POST   | /enquiry/{id}/followup        | Schedule follow-up              |
| POST   | /enquiry/{id}/escalate        | Escalate to human agent         |
| GET    | /enquiry/{id}/history         | Full history + event timeline   |
| GET    | /health                       | API + DB + worker health check  |

## Trade-offs

| Decision | Why | Limitation |
|---|---|---|
| Celery over BackgroundTasks | Process isolation, retries, monitoring | Requires Redis |
| PostgreSQL over SQLite | Production-grade, concurrent writes | Heavier local setup |
| Keyword SOP matching | Simple, deterministic, no AI cost | Low recall on complex messages |
| NativeWind | Consistent tokens, fast iteration | Babel plugin required |
| Mock data | Evaluable without backend | No live updates |
