# Project Map

## Purpose
Personal data science portfolio website. It replaces the current site at 24natel.com.
Audience: recruiters, classmates, anyone who wants to learn about the learner.

## Requirements
Stated by the learner (2026-10-11):
- Show experiences, projects, a resume link and contact info.
- Data-science-heavy: data science should be part of how the site works, not only something it describes.
- Wants 3D (three.js; PLAN.md proposes React Three Fiber).
- Learning matters most. Wants to learn R3F, FastAPI, pgvector/RAG, a hand-built agent loop, evals and DuckDB-WASM well enough to explain them in interviews.
- Works on every device. Content loads first as plain HTML; 3D and interactive parts load on top.
- Free or very cheap hosting: Vercel (site), Railway/Fly/Modal (Python API), Supabase/Neon (Postgres). Model API has a hard monthly spending cap (amount not yet set; the pasted note says "$X").
- Easy to update: add projects and concepts by editing Markdown in `content/`, not code.
- Python for the data and AI parts; TypeScript/Next.js for the front end.

## Brainstorm plan (proposed, not confirmed in learning mode)
`docs/PLAN.md` was drafted in an earlier AI-assisted brainstorm. It proposes: a scroll-story "explorable explanation" with chapters 0–8, a 3D galaxy of the learner's work, an agent with tools and an MCP server, evals, a DuckDB-WASM SQL playground, a "60-second view" for recruiters, and phases starting with Foundation (schema, content, 60-second view, deploy). Treat every design choice in it as proposed until the learner reasons through it here.

## Components
- `content/coursework.md` — Markdown list of coursework topics (existing).
- `docs/PLAN.md` — brainstorm plan (existing, proposed).
- Everything else: not built.

## Main Flow
content/*.md --?--> website (Next.js, chosen as a requirement) --?--> visitors
(How content becomes pages, and where data lives, are both undecided.)

## Data and Trust Boundaries
Unknown. Content is currently local Markdown. PLAN.md flags private details to exclude from public content: phone, GPA, unconfirmed bullets, and internal eLab/Amazon specifics.

## Build and Deployment
None. Not a Git repository yet.

## Unknowns
- Scope of the first shippable version
- How Markdown content becomes pages (build time or request time; files or database)
- Content model (which kinds of content, which fields)
- Hosting setup and domain cutover from 24natel.com
- Model API budget cap amount
