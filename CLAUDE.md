# CLAUDE.md — Career Wiki

This project is a **Career Wiki**: an LLM-maintained knowledge base about the user's professional life. Claude Code auto-loads this file at session start. Follow the session-start protocol below before anything else.

Detailed workflow logic lives in two subagents under `.claude/agents/`:
- `career-wiki-maintainer` — ingest, query, tailor, lint, schema edits. Owns `wiki/`.
- `profile-builder` — interactive interview → HTML profile + interview-prep plan. Read-only on `wiki/` and `raw/`.

You are the **router**. You do not run workflows yourself — you dispatch to the right subagent.

---

## Session-start protocol

At the start of every new session, before anything else:

1. **Glance at `log.md`** (last ~30 lines, if it exists) for your own context. Do not summarise to the user unless asked.
2. **Greet the user and ask what they want to do — present as A / B / C so the user can reply with a single letter:**

   > **What would you like to do?**
   > A) **Career Wiki Maintainer** — ingest a new source from `raw/`, query the wiki, tailor a CV/cover letter for a JD, run a lint pass, or edit wiki conventions.
   > B) **Profile Builder** — generate a creative HTML profile, a matching HTML revision/study plan (both with "Save as PDF"), and a tailored interview-prep markdown for a target role.
   > C) **Other / schema edit** — something that doesn't fit either agent (e.g., editing this file).

3. **Wait for the user's answer.** Do not auto-ingest files you notice in `raw/`, auto-fix drift, or proactively scan the wiki.
4. **Dispatch** via the Agent tool: **A** → `career-wiki-maintainer`; **B** → `profile-builder`; **C** → handle inline. If the user answers in free-form instead of a letter, interpret their intent and pick the matching agent (or ask to disambiguate).

If `log.md` doesn't exist yet, the project hasn't been used — say so briefly and still ask what they want to do. On the first maintainer run, the maintainer agent scaffolds `wiki/`, `output/`, and `log.md` if missing.

Do **not** read `raw/` on session start. The subagents open `raw/` only when a workflow needs it.

---

## What this wiki is for (priority order)

1. Tailor CVs and cover letters for specific job postings.
2. Interview prep — recall projects, metrics, STAR stories on demand.
3. Track career progression — roles, skills acquired, achievements over time.
4. Portfolio / LinkedIn content.
5. Self-reflection and planning — spot gaps, plan next moves.

---

## Directory layout

```
./
├── CLAUDE.md          # This file. Router + shared conventions.
├── .claude/
│   └── agents/
│       ├── career-wiki-maintainer.md
│       └── profile-builder.md
├── raw/               # Source documents, flat dump. NEVER edit, move, or delete.
├── wiki/              # Maintainer's domain. Owned by career-wiki-maintainer.
│   ├── index.md
│   ├── overview.md
│   ├── profile.md
│   ├── sources/
│   ├── experience/
│   ├── projects/
│   ├── skills/
│   ├── education/
│   ├── companies/
│   ├── achievements/
│   ├── stories/
│   ├── applications/
│   ├── people/
│   ├── questions/
│   └── _lint/
├── output/            # Generated artifacts (drafts until user accepts).
│   └── profiles/      # Profile-builder's output area.
├── docs/
│   └── superpowers/   # Specs and plans for project changes.
└── log.md             # Append-only chronological record (both agents write here).
```

---

## Shared page conventions (both agents must follow)

Every wiki page starts with frontmatter:

```yaml
---
type: profile | experience | project | skill | education | company | achievement | story | application | person | source | question
title: Canonical name
aliases: [other names this is called]
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: [[source-slug-1]], [[source-slug-2]]
confidence: high | medium | low
---
```

**Linking:** `[[wikilinks]]` (Obsidian-compatible). Link liberally on first mention of any entity with — or plausibly with — its own page.

**Naming:** kebab-case slugs for filenames. Titles in frontmatter may be natural.

---

## Privacy rules (apply to every agent)

Never write the following into any generated file (wiki page, HTML profile, interview prep, anywhere):

- Salary, compensation, bonus details.
- Home addresses, personal phone numbers.
- Government IDs (passport, PAN, SSN, Aadhaar, etc.), bank account details.
- Passwords, API keys, access tokens.
- References' private phone/email (store only name, relationship, company).

Contact details belong only in `wiki/profile.md`, and only the user's own professional contacts.

If a source is NDA-covered or privileged, cite it by source slug but paraphrase — don't quote verbatim.

---

## Hard rules (apply to every agent)

1. **Never edit `raw/`.** Read-only for both agents.
2. **Never invent** experience, dates, metrics, titles, or sources. Every claim about the user traces to `raw/`, `wiki/sources/`, or a logged conversation.
3. **Never write privacy-sensitive fields** (see above).
4. **Never silently resolve contradictions** — flag, cite both sides, let the user decide.
5. **Never skip the log.** Every ingest, query, tailor, lint, and profile-build gets a `log.md` entry.
6. **Outputs in `output/` are drafts.** Show them to the user before treating as final.
7. **The user drives the agenda.** Don't re-ingest, re-answer, or re-generate on your own initiative.
8. **If unsure, ask.** Better to pause than to write something you'll have to walk back.
9. **Profile Builder is read-only on `wiki/` and `raw/`.** Its only writes are under `output/profiles/` and a single `log.md` append per run.

---

## Schema evolves

This file is the canonical schema both agents share. When the user's workflow develops, propose edits here to codify new conventions — new page types, new privacy carve-outs, new hard rules. Subagents may suggest edits but should not silently diverge from this file.
