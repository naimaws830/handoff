---
name: handoff
description: Slash-command only skill. Run ONLY when the user explicitly types /handoff. Never trigger automatically, never suggest it unprompted, never run it because the conversation seems long or the topic matches. Writes a handoff.md file that captures the full state of the current conversation so a brand-new chat can continue the work with zero re-explaining. Also produces 5 validation questions with answer key so the user can test the handoff in the new chat. Works for any topic including coding, writing, research, planning, design, business, learning, and ideas.
---

# Handoff

Produce two things.

1. `handoff.md`. Lets a fresh chat pick up exactly where this one stopped. The reader is a new Claude instance with no memory of this conversation. Write for that reader.
2. Five validation questions with an answer key. Lets the user test whether the handoff worked.

## Trigger

Run only on explicit `/handoff`. If the user did not type it, do nothing and do not mention this skill.

## Process

1. Re-read the whole conversation, including pasted content, uploaded files, and tool results.
2. Fill the handoff template below. Be specific. Use exact names, paths, commands, numbers, links, and wording where they matter.
3. Save as `handoff.md`.
   - Claude.ai: `/mnt/user-data/outputs/handoff.md`
   - Claude Code or local: project root (current working directory).
4. Write the validation questions (see below). Save as `handoff-check.md` in the same folder as `handoff.md`.
5. Present both files. Then show the five questions and answers in the reply.
6. Tell the user in one line how to test. See "Validation questions".

## Handoff template

Use this structure exactly. Keep all headings, even if a section is empty (write "None").

```markdown
# Handoff

> Resume prompt: Read this file fully. Continue the work described below. Start with the Next Step. Ask me only if something is marked Unknown.

## Goal
What we are trying to achieve. One to three sentences. Include the success condition if known (what "done" looks like).

## Current State
Where things stand right now. What is finished, what is in progress, what is untouched. Include key decisions made and constraints or preferences the user stated (tone, tools, deadlines, audience, style, tech stack, budget).

## Active Work (files, drafts, items)
Things being worked on right now. For each, give the name or path and one line on status.
Items can be files, documents, drafts, spreadsheets, designs, links, datasets, shortlists, plans, or any concrete thing. If the chat produced nothing concrete, write "None".

## Changed
What was touched or decided this session. Created, edited, deleted, renamed, rewritten, dropped, chosen. Prefer concrete decisions over topics. Short list, newest last.

## Failed Attempts
Everything tried that did not work. For each give what was tried, what happened, and why it failed (or "cause unknown"). Include dead ends, rejected ideas, and errors. This stops the new chat from repeating them.

## Next Step
The single next action to take. Concrete and immediately doable. Add one or two follow-ups after it if useful.

## Open Questions (optional)
Unresolved questions or missing info. Mark anything unverified as Unknown. Omit this section if there are none.
```

## Adapting to the topic

The headings stay the same. The content changes with the domain.

- Coding: files and functions touched, commands run, error messages, branch, test status.
- Writing: draft versions, tone, audience, outline position, sections done.
- Research: question, sources used, findings so far, gaps, rejected sources.
- Planning or business: options considered, decisions, owners, dates, numbers.
- Ideas or brainstorming: concepts explored, ones dropped and why, the leading direction.

Do not force code language onto non-code work.

### Example: non-code (trip planning)

```markdown
## Active Work (files, drafts, items)
- Draft itinerary: Day 1-2 Tokyo, Day 3-5 Kyoto. Day 4 still empty.
- Shortlist: 3 hotels in Kyoto, none booked.

## Changed
- Dropped Osaka. Too much transit time.
- Budget raised from $2,000 to $2,500.
- Switched from rail pass to single tickets.

## Failed Attempts
- Hotel A: sold out for the dates.
- Rail pass: not worth it for this route.
```

### Example: coding

```markdown
## Active Work (files, drafts, items)
- src/auth.py: login fix, half done.
- tests/test_auth.py: 2 cases failing.

## Failed Attempts
- Increased token expiry: no effect, the bug is in session refresh.
```

## Validation questions

Purpose: the user opens a new chat, uploads `handoff.md`, asks the same five questions, and compares the new answers to the answer key. Matches mean the handoff works. Mismatches show what the handoff missed.

How to write them:

- Base the questions on the old conversation, not on the handoff text. The point is to test whether the handoff kept what mattered.
- Pick the five most important facts a new chat must get right. Cover a spread, for example goal or success condition, a key decision or constraint, current state, a failed attempt and why, and the next step.
- Make each question specific, with one clear correct answer. Avoid vague questions like "What is the project about?".
- Write answers from the old conversation. Keep each answer to one to three sentences.
- Do not copy question wording from the handoff headings. Ask about substance.
- Never put the answer key inside `handoff.md`. The new chat must not see it.

Format for `handoff-check.md` and for the reply:

```markdown
# Handoff Check

How to test: Open a new chat. Upload handoff.md. Ask these five questions. Compare answers with the key below.

## Questions
1. ...
2. ...
3. ...
4. ...
5. ...

## Answer Key (from the original chat)
1. ...
2. ...
3. ...
4. ...
5. ...
```

After the questions, add one line. If answers differ, tell the user to run `/handoff` again in the old chat, or add the missing detail to `handoff.md` by hand.

## Rules

- Facts only. Do not invent details. If unsure, write Unknown.
- Self-contained. Never write "as discussed above". The new chat cannot see this one.
- Short and dense. Bullets over paragraphs. Cut filler.
- Failed attempts must include the reason. A bare "didn't work" is useless.
- Never include secrets such as API keys, passwords, tokens, private keys, or sensitive personal data. Use a placeholder like `<API_KEY>` and note where it is stored. This applies to the answer key too.
- Do not include the full chat transcript. Summarize.
- Keep the user's language. If the conversation was in another language, write both files in that language.
