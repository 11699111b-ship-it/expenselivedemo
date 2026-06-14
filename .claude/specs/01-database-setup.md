# Spec: 01 — Database Setup

## 1. Overview

Replace the stub in `database/db.py` with a working SQLite implementation.

This step establishes the **data layer foundation** for the Spendly application.

All future features (authentication, profile, expense tracking) depend on this being correctly implemented.

---

## 2. Depends on

Nothing — this is the first step.

---

## 3. Routes

- No new routes
- Existing placeholder routes in `app.py` remain unchanged

---

## 4. Database Schema

### A. users

| Column | Type | Constraints |
| --- | --- | --- |
| id | INTEGER | Primary key, autoincrement |
| name | TEXT | Not null |
| email | TEXT | Unique, not null |
| password_hash | TEXT | Not null |
| created_at | TEXT | Default datetime('now') |

### B. expenses

| Column | Type | Constraints |
| --- | --- | --- |
| id | INTEGER | Primary key, autoincrement |
| user_id | INTEGER | Foreign key → users.id, not null |
| amount | REAL | Not null |
| category | TEXT | Not null |
| date | TEXT | Not null (YYYY-MM-DD format) |
| description | TEXT | Nullable |
| created_at | TEXT | Default datetime('now') |

---

## 5. Functions to Implement (`database/db.py`)

### A. `get_db()`

- Opens connection to `spendly.db` in project root
- Sets `row_factory = sqlite3.Row`
- Sets `PRAGMA foreign_keys = ON`
- Returns the connection

### B. `init_db()`

- Creates both tables using `CREATE TABLE IF NOT EXISTS`
- Safe to call multiple times

### C. `seed_db()`

- Checks if `users` table already has rows → if yes, return early
- Inserts one demo user: name=Demo User, email=demo@spendly.com, password=demo123 (hashed via werkzeug)
- Inserts 8 sample expenses linked to the demo user, spread across categories, dates in current month

---

## 6. Changes to `app.py`

- Import `get_db`, `init_db`, `seed_db` from `database.db`
- Call `init_db()` and `seed_db()` inside `with app.app_context():` block before `app.run()`

---

## 7. Files to Change

- `database/db.py` — implement all three functions
- `app.py` — add imports and startup calls

---

## 8. Files to Create

- None

---

## 9. Dependencies

- `sqlite3` (standard library)
- `werkzeug.security` (already in requirements.txt at 3.1.6)

---

## 10. Categories (Fixed List)

Food, Transport, Bills, Health, Entertainment, Shopping, Other

---

## 11. Rules

- No ORMs
- Parameterized queries only — no string formatting in SQL
- `PRAGMA foreign_keys = ON` on every connection
- `amount` stored as REAL
- Passwords hashed with `werkzeug.security.generate_password_hash`
- `seed_db()` must be idempotent (no duplicate inserts)
- Dates in YYYY-MM-DD format

---

## 12. Expected Behavior

- `get_db()` → connection with dict-like row access + FK enforcement
- `init_db()` → idempotent table creation
- `seed_db()` → idempotent seed; demo user + 8 expenses on first run, no-op thereafter
- UNIQUE constraint on email enforced
- FK constraint on expenses.user_id enforced

---

## 13. Error Handling

- Duplicate email insert → UNIQUE constraint error (expected, not swallowed)
- Invalid user_id on expense insert → FK constraint error (expected, not swallowed)
- Invalid queries → raise clearly for debugging

---

## 14. Definition of Done

- [ ] Database file created on app startup
- [ ] Both tables exist with correct schema and constraints
- [ ] Demo user exists with hashed password
- [ ] 8 sample expenses across categories
- [ ] No duplicate seed data on repeated runs
- [ ] App starts without errors
- [ ] Foreign key enforcement works
- [ ] All queries use parameterized form
