# ListMates – project rules for Claude Code

ListMates is a social notes and to-do website ("socializing through productivity").
Team: Group 7 (Emin Ismayilzade, Suleyman Abiyev, Ali Garashli), CSCI 3509, Fall 2026.
The full feature list is in the GitHub issues (FR-01 to FR-24, from our project proposal).

## Repository layout
- `backend/`  Python 3.11+, FastAPI, Beanie (MongoDB), APScheduler. Run from `backend/`.
- `frontend/` React + TypeScript + Vite, Tailwind CSS, shadcn/ui, TanStack Query, React Router, BlockNote.
- `k8s/`      Kubernetes YAML: MongoDB (MongoDB Controllers for Kubernetes operator), backend, frontend, Uptime Kuma.
- `docs/`     Documentation. `docs/assignment2/` is the graded API prototype; do not change it.

## API rules
- Base path `/api/v1`. Resource names are plural nouns (`/lists`, `/users/{id}/public-lists`).
- Success: `{success: true, message, data, meta?}`. Errors: `{success: false, error: {code, message, details?}}`.
  Always build responses with `backend/app/envelope.py`; add a new shortcut there for each new error code.
- Order of checks: route/method -> auth (401) -> ID/query format (400) -> ownership/visibility (404)
  -> role (403) -> body validation (400) -> business rules (409).
- Another user's private resource returns 404, not 403 (do not reveal that it exists).
- Visibility is checked on the server for every request. A list can be seen if the viewer owns it,
  it is public, it is network and the viewer is an accepted follower, or it is a group list and the
  viewer is a member. Never rely on hiding things in the UI.
- At most 4 public lists per user (409 `PUBLIC_LIST_LIMIT_REACHED`).

## Data
- MongoDB through Beanie models in `backend/app/models/`. Data access stays in `backend/app/store.py`
  (or `backend/app/store/` modules), never directly in routes.
- Collections: users, follows, lists, items, comments, reactions, nudges, notifications, groups,
  puzzles, puzzle_results, reports, activity_events. Files (images, PDFs) go in GridFS, max 10 MB.
- Log an `activity_events` document for: task created, task completed, task missed, nudge received,
  game played. The AI monthly report is built only from these events.

## AI, integrations, monitoring
- All AI calls go through `backend/app/ai.py` (OpenAI-compatible client; Groq by default, Gemini as backup).
  Send numbers and task titles only, never note contents. AI never invents numbers: code computes stats.
- Google Calendar, Meet and Gmail use the user's stored Google refresh token (scopes requested at sign-in).
- Weather uses Open-Meteo (no key). Monitoring alerts go to Telegram through `backend/app/monitoring.py`.

## Secrets
- Secrets live only in `.env` files (git-ignored). Every new variable goes into `backend/.env.example`
  with an empty value. Never print, log or commit tokens.

## Tests and workflow
- Every new endpoint gets pytest tests in `backend/tests/`. Run `python -m pytest` from `backend/` before pushing.
- Tests must not need internet, Google or a real AI key: mock external calls.
- One GitHub issue = one branch (`feature/FR-07-follows`, `fix/...`, `docs/...`) = one pull request.
- Small commits with imperative messages ("Add follow request endpoints").
- PRs need 1 approval from another team member and are merged with "Create a merge commit".
  The author of the PR clicks Merge.
