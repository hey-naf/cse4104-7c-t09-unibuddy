# UniBuddy

**An AI study companion that answers questions from a student's own lecture material — and cites the page it came from.**

CSE 4104 · Software Development III · Section 7C · Fall 2026
Northern University of Business and Technology, Khulna

![status](https://img.shields.io/badge/status-week%2001%20%E2%80%94%20planning-17365D)
![course](https://img.shields.io/badge/CSE%204104-Software%20Development%20III-2E7D6B)

> **Status:** Week 1 — team formation and idea selection. There is no application code yet.
> This repository holds the project definition, research, and documentation. The build
> starts in Week 6, following the course schedule.

---

## Team

| Name | Student ID | Role |
|---|---|---|
| Md Nafew Yeasin | 11230321355 | Team Leader |
| Mujibur Rahman Fahad | 11230321568 | AI Integration Lead |
| Abid Hasan | 11230321553 | Frontend Developer |
| M R Russell | 11230321545 | Backend Developer |

**Team:** `CSE4104-7C-T09` · **Section:** 7C · **Institution:** NUBT Khulna
**Course instructor:** Md. Riaz Mahmud, Assistant Professor

---

## The problem

A university student carries five or six courses at once. Each has its own handout, its
own deadline pattern, and its own exam date — spread across printed sheets, photographed
whiteboards, and PDFs in a group chat. Nothing is in one place and nothing is searchable.

The harder half arrives at exam time. Four hundred pages of lecture PDFs, and no
efficient way to find the paragraph that answers the question you are stuck on. Re-reading
everything is too slow. Asking a general chatbot does not work either: it has never seen
the handout, so it answers confidently and wrongly.

## What UniBuddy does

A student registers, adds the courses they are taking, and uploads the PDFs handed out in
class. UniBuddy indexes those documents and answers questions about them in plain
language — **using only the uploaded material, and returning the page number the answer
came from** so it can be checked against the original.

Alongside that, it tracks what each course requires: assignment deadlines, quiz and exam
dates, and their status. The dashboard shows what is due this week, so the student reaches
the tutor already knowing which course needs attention.

**One-line pitch:** *your own lecture PDFs, turned into a tutor that cites its sources.*

## Why the AI is load-bearing

Three things here cannot be done without it:

1. **Semantic search.** A question rarely shares words with its answer. "Why does TCP need
   a three-way handshake" does not keyword-match "both hosts must agree on initial
   sequence numbers". Meaning-matching requires embeddings.
2. **Grounding in a closed document set.** A general model has not read the student's
   course pack. Retrieval-augmented generation restricts the answer to retrieved passages,
   which is what makes citations possible at all.
3. **Explanation at the right level.** The same passage must serve a first-year and a
   final-year student; adapting the explanation is a language task.

The course organizer requires none of this — it is ordinary CRUD. If the AI were removed,
UniBuddy would be a to-do list. That is the test we used to decide what this project is.

---

## Features

### First working release (V1)

- [ ] Registration, login, and logout with private per-user data
- [ ] Student profile — university, department, semester
- [ ] Course management (add / edit / delete)
- [ ] Assignment, quiz, and exam tracking with deadlines and status
- [ ] Dashboard showing what is due this week
- [ ] Upload lecture PDFs, organised by course
- [ ] **AI question answering grounded in uploaded documents, with page references**
- [ ] Responsive layout for desktop, tablet, and mobile
- [ ] Persistent storage of all data

### Deliberately deferred

Scope is cut on purpose so the core can be made reliable first. These remain part of the
project's direction:

- [ ] Quiz and flashcard generation from uploaded material
- [ ] GPA / CGPA and attendance tracking
- [ ] Helpdesk over institutional documents (handbooks, notices)
- [ ] Group project collaboration
- [ ] Bangla-language explanations

---

## Technology stack

| Layer | Choice | Why |
|---|---|---|
| Frontend | React 18 + Vite, Tailwind CSS | Component model suits a dashboard; fast builds; responsive without hand-written CSS |
| Backend | Node.js + Express | One language across the stack; direct access to AI SDKs |
| Database | PostgreSQL on Supabase | Relational fits courses → assignments → documents; `pgvector` stores embeddings in the same database |
| AI | Google Gemini API (chat + embeddings) | Multimodal, usable free tier, generous context window; OpenAI kept as a fallback behind one interface |
| Retrieval | Chunking + `pgvector` similarity search | Grounds answers in the student's own material and yields page numbers for citation |
| Auth | JWT + bcrypt | Named in the course handbook; stateless sessions, unrecoverable passwords |
| File storage | Supabase Storage (private bucket) | Uploaded handouts are personal; per-user access policies |
| Deployment | Vercel · Render · Supabase | Free tiers sufficient for a semester; secrets via environment variables |
| Version control | Git + GitHub | Mandatory for the course |

---

## Repository structure

```
.
├── frontend/          React application
├── backend/           Express API server
├── database/          Schema, migrations, seed data
├── documentation/     Project vision, SRS, design, final report
├── testing/           Test plans, test cases, bug reports
├── screenshots/       UI captures for the README and submissions
├── presentation/      Slide decks and demo assets
└── README.md
```

---

## Getting started

> Not yet runnable — the application is built from Week 6. This section is filled in as
> each part lands.

Planned setup once the backend exists:

```bash
git clone https://github.com/<org>/cse4104-7c-t09-unibuddy.git
cd cse4104-7c-t09-unibuddy

# backend
cd backend && npm install && cp .env.example .env   # then fill in .env
npm run dev

# frontend (separate terminal)
cd frontend && npm install && npm run dev
```

### Environment variables

Copy `.env.example` to `.env` and fill in real values. **Never commit `.env`.**

| Variable | Purpose |
|---|---|
| `DATABASE_URL` | PostgreSQL connection string |
| `JWT_SECRET` | Signing key for auth tokens |
| `GEMINI_API_KEY` | AI provider key |
| `SUPABASE_URL` / `SUPABASE_SERVICE_KEY` | File storage |

---

## Branch strategy

| Branch | Purpose |
|---|---|
| `main` | Stable, always deployable |
| `development` | Integration branch; feature work merges here first |
| `frontend` | UI work |
| `backend` | API and database work |
| `ai-integration` | Retrieval and AI feature work |

Work lands on a feature branch, merges to `development` after review, and reaches `main`
only when it is deployable.

### Commit messages

Describe what changed, in the imperative. The course handbook marks commit quality
directly.

```
Added JWT authentication
Implemented PDF chunking and embedding pipeline
Fixed dashboard responsiveness below 480px
```

Not `update`, `done`, `work`, or `final`.

---

## Roadmap

| Weeks | Milestone |
|---|---|
| 1–2 | Idea, research, proposal, repository setup |
| 3–4 | SRS, architecture, ER diagram, API contract |
| 5 | Figma wireframes for the four core screens |
| 6 | Backend — auth, schema, CRUD APIs |
| 7 | Frontend — React app on live APIs |
| 8 | AI — ingestion, chunking, embeddings, retrieval, cited answers |
| 9 | Feature completion against the course checklist |
| 10 | Testing — functional, API, UI, AI-response quality |
| 11 | Deployment |
| 12–14 | Documentation, presentation, viva |

---

## Deployment

_No live deployment yet._ Link will be added in Week 11.

## Screenshots

_No interface yet._ Captures will be added as screens are built.

---

## Academic integrity

This project is built by the team named above for CSE 4104. AI assistance is used as a
development tool; every member understands and can explain the code they submit. No code
or text has been copied from the comparable products reviewed in our research — those are
cited as prior art only.
