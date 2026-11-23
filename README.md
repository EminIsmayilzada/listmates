# ListMates

ListMates is a website for notes and to-do lists, built around the idea of **socializing through productivity**.
Users build lists with text, checklists, images, PDFs and links, and choose who sees each list:
**Personal** (only them), **Network** (approved followers) or **Public** (everyone, at most 4, shown on their profile).
Followers comment, react and nudge friends who miss their daily tasks; group leaders assign tasks;
a daily word game and Sudoku keep streaks going; and AI writes a monthly productivity report and breaks
big tasks into steps.

**Team:** Group 7, CSCI 3509 Intro to Software Engineering, Fall 2026

| Member | Owns |
|---|---|
| Emin Ismayilzade | AI features, games, Google integrations, monitoring, deployment |
| Suleyman Abiyev | Backend API and database (MongoDB) |
| Ali Garashli | Frontend and Google sign-in |

## Repository layout

```
backend/    FastAPI REST API (Python 3.11+)
frontend/   React + TypeScript web app (Vite)
k8s/        Kubernetes files: MongoDB, backend, frontend, monitoring
docs/       Documentation (docs/assignment2/ is the graded API prototype)
CLAUDE.md   Project rules for Claude Code
```

## Tech stack

| Part | Choice |
|---|---|
| Frontend | React + TypeScript (Vite), Tailwind CSS, shadcn/ui, TanStack Query, BlockNote editor |
| Backend | Python, FastAPI, Beanie, APScheduler |
| Database | MongoDB + GridFS on Kubernetes (MongoDB Controllers for Kubernetes); MongoDB Atlas free tier for development |
| Login | Google Sign-In (OAuth) + JWT in an httpOnly cookie |
| AI | Groq API (OpenAI-compatible), Gemini as backup |
| Integrations | Google Calendar, Google Meet (via Calendar), Gmail, Open-Meteo weather |
| Monitoring | Uptime Kuma, Python logging, Telegram Bot API |
| Hosting | minikube locally, k3s on Azure for Students online |

## Run the backend

```bash
cd backend
python -m venv .venv
source .venv/bin/activate          # Windows PowerShell: .venv\Scripts\Activate.ps1
pip install -r requirements.txt
cp .env.example .env               # then fill in the values (ask the team privately)
uvicorn app.main:app --reload
```

- API docs: http://127.0.0.1:8000/docs
- Tests: `python -m pytest`

The backend currently runs on the in-memory mock data from Assignment 2. MongoDB replaces it in issue FR-02.

## Run the frontend

Coming in the frontend setup issue (`npm install`, `npm run dev`, then http://localhost:5173).

## How we work

- Every feature is a GitHub issue (FR-01 to FR-24 from the proposal) on the project board, with an owner and a week.
- One issue = one branch = one pull request. A teammate approves, the author merges with **Create a merge commit**.
- CI runs the backend tests (and the frontend build) on every pull request.
- Secrets live in `.env` files only, never in Git.
- See `CLAUDE.md` for the coding rules all of us (and Claude Code) follow.
