# Handoff Skill

Move a full conversation into a new chat with no re-explaining.

Type `/handoff` and Claude writes two files:

- `handoff.md`: goal, current state, active work, changes, failed attempts, next step.
- `handoff-check.md`: 5 validation questions with an answer key.

Works for any topic. Coding, writing, research, planning, design, business, ideas.

## Install

### Claude Code

```bash
git clone https://github.com/YOUR_USERNAME/handoff-skill.git
cp -r handoff-skill/handoff ~/.claude/skills/
```

Project-only install: copy to `.claude/skills/` inside your project instead.

Optional hard slash-only lock. Add this line to the frontmatter of `handoff/SKILL.md`:

```yaml
disable-model-invocation: true
```

### Claude.ai

1. Download this repo as a zip from GitHub.
2. Zip the `handoff` folder so the folder sits at the zip root.
3. Open Settings, then Capabilities, then Skills. Upload the zip.

Menu names may change. Check current Claude docs if needed.

Do not add `disable-model-invocation` on Claude.ai. The uploader rejects it.

## Use

1. In the old chat, type `/handoff`.
2. Claude saves `handoff.md` and `handoff-check.md`.
3. Open a new chat. Upload `handoff.md`.
4. Ask the 5 questions from `handoff-check.md`.
5. Compare answers with the answer key.

Answers match: handoff works.
Answers differ: handoff missed a detail. Re-run `/handoff` or edit `handoff.md` by hand.

## handoff.md structure

| Section | Purpose |
| --- | --- |
| Goal | What we are trying to achieve |
| Current State | Where things stand now |
| Active Work | Files, drafts, or items in progress |
| Changed | What was touched or decided this session |
| Failed Attempts | What did not work and why |
| Next Step | The next action to take |
| Open Questions | Optional. Unresolved items |

## Example (trip planning)

```markdown
## Active Work (files, drafts, items)
- Draft itinerary: Day 1-2 Tokyo, Day 3-5 Kyoto. Day 4 still empty.
- Shortlist: 3 hotels in Kyoto, none booked.

## Failed Attempts
- Hotel A: sold out for the dates.
- Rail pass: not worth it for this route.

## Next Step
Fill Day 4. Book one Kyoto hotel.
```

## Privacy

The skill is told to never write secrets into the files. API keys, passwords, and tokens become placeholders. Still review both files before sharing.

## License

MIT
