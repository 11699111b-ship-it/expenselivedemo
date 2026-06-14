---
name: project-spendly
description: "Overview of the Spendly expense tracker project — stack, structure, and implementation status"
metadata: 
  node_type: memory
  type: project
  originSessionId: 971e1bef-b8a1-414e-93e7-12e6f1d5fe4c
---

Spendly is a Flask-based expense tracker built as a step-by-step teaching project. Students implement functionality incrementally across defined steps.

**Why:** Teaching scaffold — frontend shell is complete, students build out backend features step by step.

**How to apply:** When suggesting changes, respect the step-based structure. Don't implement stub routes ahead of the student's current step unless asked.

## Stack
- Python + Flask, SQLite (no ORM), Jinja2 templates
- Runs on port 5001
- Fonts: DM Sans + DM Serif Display

## Key Files
- `app.py` — all routes registered directly (no blueprints)
- `database/db.py` — SQLite layer: `get_db()`, `init_db()`, `seed_db()`
- `templates/` — Jinja2 templates extending `base.html`
- `static/css/style.css` — global styles; `static/css/landing.css` — landing only
- `static/js/main.js` — loaded globally

## Current State (as of 2026-06-14)
Working routes: `/`, `/register`, `/login`, `/terms`, `/privacy`

Stub routes (not yet implemented):
- Step 3: `/logout`
- Step 4: `/profile`
- Step 7: `/expenses/add`
- Step 8: `/expenses/<id>/edit`
- Step 9: `/expenses/<id>/delete`
