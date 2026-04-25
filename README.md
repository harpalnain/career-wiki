# Career Wiki

An LLM-maintained knowledge base about your professional life — resumes, projects, skills, achievements, stories, applications, and the people you've worked with — kept in plain Markdown and curated by Claude Code.

> Inspired by [Andrej Karpathy's LLM Wiki idea](https://x.com/karpathy/status/1884252385253169969) — the notion that an LLM can act as a long-running personal librarian, ingesting raw artifacts over time and maintaining a clean, queryable, hand-editable knowledge base on your behalf.

## What it's for

In priority order:

1. **Tailor CVs and cover letters** for specific job postings.
2. **Interview prep** — recall projects, metrics, and STAR stories on demand.
3. **Track career progression** — roles, skills, achievements over time.
4. **Portfolio / LinkedIn content.**
5. **Self-reflection and planning** — spot gaps, plan next moves.

## How it works

You drop source documents (resumes, performance reviews, project write-ups, recommendation letters, transcripts, screenshots, etc.) into `raw/`. Claude Code ingests them on request, normalizes the contents into a wiki of cross-linked Markdown pages under `wiki/`, and keeps an append-only `log.md` of everything it has done.

`raw/` is **read-only** to the agents — your originals are never edited, moved, or deleted. The wiki is fully hand-editable; if you don't like how something was summarized, just edit the file.

## The two agents

This project ships with two specialized subagents under `.claude/agents/`. The root `CLAUDE.md` acts as a router that dispatches to the right one.

- **`career-wiki-maintainer`** — owns `wiki/`. Ingests new sources from `raw/`, answers questions about your career, tailors CVs and cover letters for a job description, runs lint passes for staleness and contradictions, and proposes schema edits.
- **`profile-builder`** — read-only on `wiki/` and `raw/`. Runs an interactive interview about a target role and produces a creative HTML profile, an HTML revision/study plan (both with "Save as PDF"), and a tailored interview-prep Markdown. All outputs go under `output/profiles/`.

At session start, Claude greets you with an **A / B / C** menu so you can pick which agent to invoke (or handle a one-off schema edit inline).

## Conventions

Every wiki page starts with YAML frontmatter declaring its `type`, `title`, `aliases`, `created` / `updated` dates, source citations, and a `confidence` rating. Pages cross-reference each other using `[[wikilinks]]` (Obsidian-compatible). Filenames are kebab-case slugs.

See `CLAUDE.md` for the directory layout and full page schema — that's the canonical source and where conventions evolve.

## Privacy

The agents will never write the following into any generated file:

- Salary, compensation, or bonus details
- Home addresses or personal phone numbers
- Government IDs (passport, PAN, SSN, Aadhaar, etc.) or bank details
- Passwords, API keys, or access tokens
- References' private phone numbers or emails (only name, relationship, and company)

Contact details live only in `wiki/profile.md`, and only your own. NDA-covered sources are cited by slug and paraphrased rather than quoted.

## Hard rules

1. `raw/` is read-only.
2. Nothing is invented — every claim about you traces to `raw/`, `wiki/sources/`, or a logged conversation.
3. No privacy-sensitive fields, ever.
4. Contradictions are flagged, never silently resolved.
5. Every ingest, query, tailor, lint, or profile build gets a `log.md` entry.
6. Outputs in `output/` are drafts until you accept them.
7. You drive the agenda — agents don't re-ingest, re-answer, or re-generate on their own initiative.

## Getting started

1. Open this directory in [Claude Code](https://claude.com/claude-code).
2. Drop a few documents into `raw/` — old resumes, project notes, anything career-relevant.
3. Start a session. Claude will greet you with the A / B / C menu.
4. Pick **A** and ask the maintainer to ingest what you just dropped in.
5. From then on, ask questions, tailor applications, or generate profiles whenever you need them.

The wiki gets richer the more you feed it. Treat `raw/` as your inbox.

---

> Generated from [career-wiki](https://github.com/harpalnain/career-wiki) — an LLM-maintained career wiki template.
