# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Activate virtualenv (required before any python/flask/pytest commands)
source venv/bin/activate

# Run the dev server (port 5001)
python app.py

# Run all tests
pytest

# Run a single test file
pytest tests/test_db.py

# Install dependencies
pip install -r requirements.txt
```

## Architecture

**Spendly** is a Flask expense-tracker app built as a step-by-step teaching project. Many routes are stubbed placeholders that students implement incrementally.

**Entry point:** `app.py` — all routes are registered here directly (no blueprints). The app runs on port 5001.

**Database layer:** `database/db.py` is the only database file. It must export:
- `get_db()` — returns a SQLite connection with `row_factory = sqlite3.Row` and `PRAGMA foreign_keys = ON`
- `init_db()` — creates all tables using `CREATE TABLE IF NOT EXISTS`
- `seed_db()` — inserts sample development data

The database file is SQLite (no ORM). Call `get_db()` per request; close the connection in a teardown.

**Templates:** Jinja2, all extending `templates/base.html`. The base provides the navbar (brand + sign in / get started links) and footer (terms/privacy links). Page-specific CSS is injected via `{% block head %}`. Page-specific scripts via `{% block scripts %}`.

**Static assets:** `static/css/style.css` is the global stylesheet (DM Sans + DM Serif Display fonts). `static/css/landing.css` is landing-page-only. `static/js/main.js` is loaded globally.

**Stub routes** (not yet implemented — students add these in later steps):
- `/logout` — Step 3
- `/profile` — Step 4
- `/expenses/add` — Step 7
- `/expenses/<id>/edit` — Step 8
- `/expenses/<id>/delete` — Step 9
