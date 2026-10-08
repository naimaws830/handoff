---
name: handoff
description: Slash-command only skill. Run ONLY when the user explicitly types /handoff. Never trigger automatically, never suggest it unprompted, never run it because the conversation seems long or the topic matches. Writes a handoff.md file that captures the full state of the current conversation so a brand-new chat can continue the work with zero re-explaining. Also produces 5 validation questions with answer key so the user can test the handoff in the new chat. Works for any topic including coding, writing, research, planning, design, business, learning, and ideas.
---

# Handoff

Produce one file and one chat message.

1. `handoff.md`. A file that lets a fresh chat pick up exactly where this one stopped. The reader is a new Claude instance with no memory of this conversation. Write for that reader.
2. Five validation questions with answers, shown in the chat reply only. Do not save them to any file. They let the user test whether the handoff worked.

## Trigger

Run only on explicit `/handoff`. If the user did not type it, do nothing and do not mention this skill.

## Process

1. Re-read the whole conversation, including pasted content, uploaded files, and tool results.
2. Fill the handoff template below.
3. Save as `handoff.md`.
   - Claude.ai: `/mnt/user-data/outputs/handoff.md`
   - Claude Code or local: project root (current working directory).
4. Present `handoff.md`.
5. In the same reply, write the five validation questions and answers (see below). Chat only. No second file.
6. Add one line telling the user how to test.

## Writing style for handoff.md

Write in full sentences. Use short paragraphs. Do not use bullet fragments or note-style shorthand.

Why: a new chat reads sentences more reliably and reproduces them in its own answers. Fragments lose the reasoning that connects facts, such as why something failed or why a decision was made.

A list is acceptable only for items that are naturally a list, such as file paths or commands. Even then, give each item a sentence of context.

Be specific. Use exact names, paths, commands, numbers, links, and wording where they matter.

## Handoff template

Use this structure exactly. Keep all headings, even if a section is empty (write "None").

```markdown
# Handoff

> Resume prompt: Read this file fully. Continue the work described below. Start with the Next Step. Ask me only if something is marked Unknown.

## Goal
One to three sentences on what we are trying to achieve. Include the success condition if known, meaning what "done" looks like.

## Current State
A short paragraph or two on where things stand right now. Say what is finished, what is in progress, and what is untouched. Include key decisions made and any constraints or preferences the user stated, such as tone, tools, deadlines, audience, style, tech stack, or budget.

## Active Work (files, drafts, items)
Describe the things being worked on right now. For each one, give its name or path and a sentence on its status. Items can be files, documents, drafts, spreadsheets, designs, links, datasets, shortlists, plans, or any concrete thing. State where each item lives, for example in a file, only in the chat, or in both. If the chat produced nothing concrete, write "None".

## Changed
Describe what was touched or decided this session. Cover what was created, edited, deleted, renamed, rewritten, dropped, or chosen. Prefer concrete decisions over topics. Put the newest change last.

## Failed Attempts
Describe everything tried that did not work. For each attempt, say what was tried, what happened, and why it failed. Write "cause unknown" only if the conversation truly never found a cause. Include dead ends, rejected ideas, and errors, so the new chat does not repeat them.

## Next Step
State the single next action to take, concrete and immediately doable. Add one or two follow-up actions after it if useful. Include every specific instruction the user gave about upcoming work.

## Open Questions (optional)
List unresolved questions or missing information. Mark anything unverified as Unknown. Omit this section if there are none.
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
The draft itinerary lives only in the chat. It covers Tokyo on days 1 and 2 and Kyoto on days 3 to 5, and day 4 is still empty. A shortlist of three Kyoto hotels also exists in the chat, and none of them is booked.

## Changed
Osaka was dropped because the transit time was too long. The budget was raised from $2,000 to $2,500. The plan switched from a rail pass to single tickets.

## Failed Attempts
Hotel A was sold out for the travel dates. The rail pass was rejected because it did not pay off on this route.
```

### Example: coding

```markdown
## Active Work (files, drafts, items)
The file src/auth.py holds the login fix, which is about half done. The file tests/test_auth.py has two failing cases that cover session refresh.

## Failed Attempts
Increasing the token expiry had no effect, because the real bug is in session refresh and not in expiry.
```

## Accuracy rules

These come from real handoff tests, where the new chat contradicted the original on small details.

- Location. Before writing that something exists only in the chat, or only in a file, check the whole conversation. If it appears in both, say both. A false "not in any file" is a contradiction, not a harmless simplification.
- Versions. When one idea exists in several forms, such as a full version, a preview, and a simplified demo, name each form and say where it lives. Do not merge them into one.
- Origin. When a claim came from a specific source, record the source. For example, say that a fact came from a store listing or a user message.
- No filler causes. Do not write "cause unknown" when the conversation gave the cause.
- Instructions. Copy every concrete instruction the user gave about next work, including small renames and fixes.

## Validation questions

Purpose: the user opens a new chat, uploads `handoff.md`, asks the same five questions, and compares the new answers to the answers given in this chat. Matching facts mean the handoff works. A contradiction or a missing fact shows what the handoff missed. Extra detail is fine if it does not contradict the original.

Show the questions in the chat reply only. Never save them or their answers to a file. Keeping them out of files means the new chat cannot see the answers by accident.

### How to write the questions

- Base them on the old conversation, not on the handoff text. The point is to test whether the handoff kept what mattered.
- Pick the five most important facts a new chat must get right. Cover a spread, for example the goal or success condition, a key decision or constraint, a fact whose location or origin matters, a failed attempt and why, and the next step.
- Make each question open and reasoning-based. Ask what something means, what is deliberately not claimed, or why a choice was made. End with "Give the reasons." when a rationale matters.
- Avoid vague questions like "What is the project about?". Each question needs one clear correct answer.
- Do not copy wording from the handoff headings. Ask about substance.

Example question: "If a user sees a positive impact score for a REIT, what does it mean, and what is the system deliberately not claiming? Give the reasons."

### How to write the answers

- Write answers from the old conversation, not from `handoff.md`.
- Use full sentences, one to four sentences each. No bullet points.

### Format in the chat reply

Put each answer directly after its question. Do not group all questions first and all answers later.

```markdown
**Question 1**
...

**Answer 1**
...

**Question 2**
...

**Answer 2**
...

(continue to Question 5 and Answer 5)
```

After the five pairs, give a test prompt the user can copy into the new chat. It holds the questions only, so the answers stay hidden:

```markdown
Read handoff.md. Answer the five questions below. For each one, write the question first, then the answer in full sentences. Do not use bullet points.

1. (question 1 only)
2. (question 2 only)
3. (question 3 only)
4. (question 4 only)
5. (question 5 only)
```

Then add one line. Open a new chat, upload `handoff.md`, paste the test prompt, and compare. If answers contradict or miss facts, run `/handoff` again in the old chat, or add the missing detail to `handoff.md` by hand.

## Rules

- Facts only. Do not invent details. If unsure, write Unknown.
- Self-contained. Never write "as discussed above". The new chat cannot see this one.
- Dense but readable. Short sentences, no filler.
- Failed attempts must include the reason. A bare "didn't work" is useless.
- Never include secrets such as API keys, passwords, tokens, private keys, or sensitive personal data. Use a placeholder like `<API_KEY>` and note where it is stored. This applies to the answers too.
- Do not include the full chat transcript. Summarize.
- Keep the user's language. If the conversation was in another language, write the file and the chat questions in that language.