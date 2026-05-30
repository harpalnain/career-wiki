---
name: hello-career-wiki
description: Starts (or restarts) the Career Wiki session — runs the session-start router workflow defined in CLAUDE.md. Use whenever the user opens the project and wants to begin, says "hello", "let's start", "what can I do here", "give me the menu", or otherwise asks to kick off / re-show the A/B/C choice between the Career Wiki Maintainer, Profile Builder, or a schema edit. Trigger this even if the user only hints they want to get going rather than naming a specific task.
---

# hello-career-wiki — Career Wiki session starter

This skill runs the **session-start protocol** so the user can pick what they
want to do and get routed to the right subagent. CLAUDE.md is the source of
truth for that protocol and is auto-loaded for this project — follow what it
says under **"Session-start protocol"**. The summary below is a fallback in
case that section ever moves; if it conflicts with CLAUDE.md, CLAUDE.md wins.

## What to do

Follow these steps in order. You are the **router** — you present choices and
dispatch, you do not run the Maintainer or Profile Builder workflows yourself.

1. **Glance at `log.md`** (last ~30 lines, only if it exists) for your own
   context. This is for you — don't summarise it back to the user unless they
   ask. If `log.md` doesn't exist, the project hasn't been used yet: say so
   briefly, then still continue to the menu.

2. **Greet the user and present the menu as A / B / C**, so they can reply with
   a single letter:

   > **What would you like to do?**
   > A) **Career Wiki Maintainer** — ingest a new source from `raw/`, query the
   >    wiki, tailor a CV/cover letter for a JD, run a lint pass, or edit wiki
   >    conventions.
   > B) **Profile Builder** — generate a creative HTML profile, a matching HTML
   >    revision/study plan (both with "Save as PDF"), and a tailored
   >    interview-prep markdown for a target role.
   > C) **Other / schema edit** — something that doesn't fit either agent
   >    (e.g., editing CLAUDE.md).

3. **Wait for the user's answer.** Do not auto-ingest files in `raw/`,
   auto-fix drift, or proactively scan the wiki. The user drives the agenda.

4. **Dispatch** via the Agent tool:
   - **A** → `career-wiki-maintainer` subagent
   - **B** → `profile-builder` subagent
   - **C** → handle inline (no subagent)

   If the user replies in free-form instead of a letter, interpret their intent
   and pick the matching agent — or ask one short question to disambiguate if
   it's genuinely unclear.

## Why it works this way

The greeting + single-letter menu keeps the entry point friction-free: the user
doesn't have to remember subagent names or phrase a precise request. The
`log.md` glance gives you continuity across sessions without burdening the user
with a recap. And routing (rather than doing) keeps the two specialised
subagents — and their hard rules around `raw/`, privacy, and the wiki — as the
only things that actually touch the user's career data.
