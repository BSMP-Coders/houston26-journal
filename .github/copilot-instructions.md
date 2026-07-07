---
description: "Copilot context for the BSMP Dev Journal — a beginner-friendly Flask portfolio app students use on day one to set up their agent, brainstorm their final project, and log what they build each class."
applyTo: '**'
---
# Dev Journal — Copilot Context

Context for GitHub Copilot (and other agents) while a student works in this repo.
This file is always-on for the whole repo.

## What this repo is
A high-school student's personal **Dev Journal / portfolio app**, built with **Python + Flask**.
It's the student's very first hands-on repo in the program. Two things happen here:
1. **Day-one warm-up:** the student sets up their Copilot agent (BYOK model), then uses Copilot to
   brainstorm a final-project idea and save it as `my_project_idea.md` (see `Brainstorm_activity.md`).
2. **Ongoing:** at the end of each class the student adds a journal entry about what they built.

Assume the student is a **beginner** with little prior coding experience.

## Teaching style — Learn, Build, Create (not vibe coding)
- After each change, **proactively explain — without being asked — what you did AND why**, in a few
  plain-language sentences a high-schooler gets. Teach the concept, not just "I added a route."
- Treat Copilot as a **thinking partner**: when brainstorming, ask 2–4 clarifying questions before
  suggesting ideas; when the student seems stuck, offer to explain rather than dumping a wall of code.
- The student should come away **understanding the idea**, not just with running code. Offer to go deeper.

## Running the app — let the student run it
- Run with **`python app.py`** — it starts on **http://localhost:5001** in debug mode and
  auto-reloads on save.
- **Don't start the server yourself as a follow-up step.** After you edit, tell the student which
  command to run and which URL to open, so they learn the terminal workflow. If they explicitly ask
  you to run or restart it, that's fine.

## How this app works (so you edit it correctly)
- **Pure Flask + a JSON file. No database, no AI calls.** The app runs with **no keys**. Don't add
  Azure/OpenAI code to this app unless the student is deliberately extending it.
- Entries live in `data/entries.json` as a list of objects. Each entry has exactly these keys:
  `title`, `date`, `summary`, `learned`, `coolest`, `screenshot`. Keep this shape.
- Routes (one route = one concept):
  - `/` → `home()` lists all entries (`templates/index.html`)
  - `/entry/<int:entry_id>` → shows one entry by its position in the list (`templates/entry.html`)
  - `/add` → GET shows the form, POST validates + saves a new entry (`templates/add.html`), then redirects home
- Reuse the existing `load_entries()` / `save_entries()` helpers for file I/O — don't reinvent it.

### Keep form ↔ route ↔ template field names in sync
The `/add` form inputs, the keys `request.form.get(...)` reads, and the keys the templates render
**must match exactly** (`title`, `date`, `summary`, `learned`, `coolest`, `screenshot`). A mismatch
shows a **blank value with no error** — very hard for a beginner to debug. If you add a field, update
the form, the route, and the templates together.

## The `.env` / BYOK setup
- `.env-sample` and the `.env` step (see the README link) are for **setting up the Copilot agent**
  (Bring Your Own Key) — **not** for running this journal app. `app.py` never reads those variables.
- Never hardcode keys; never commit `.env` (it's gitignored).

## If the student extends this with AI (optional, later)
This app has no AI by default. If a student adds an AI feature (e.g., for their final project), point
them to the BYOK setup page and use the **same known-good shape as the lessons**:
- Use `from openai import AzureOpenAI` — **never** plain `OpenAI` (this program uses Azure).
- `load_dotenv(override=True)` so `.env` wins over stale Codespaces/shell variables.
- Read `AZURE_OPENAI_ENDPOINT`, `AZURE_OPENAI_API_KEY`, and `AZURE_OPENAI_DEPLOYMENT` (the deployment
  name, not a raw model id) from `.env`.
- In any `try/except`, **`print(e)`** so the real error (e.g. `401 - invalid subscription key`) shows
  in the Flask console instead of being swallowed.

## Code style
- Simple variable names; a short comment above each function saying what it does; f-strings for formatting.
- Default to **simple Python** — no list comprehensions, decorators, or classes.
- Keep HTML/templates simple and readable.

## Environment
- Students work in **GitHub Codespaces**; Python 3 with Flask preinstalled (`pip install -r requirements.txt`).
- GitHub Copilot is active in **agent mode** — explain what changed after each edit.
