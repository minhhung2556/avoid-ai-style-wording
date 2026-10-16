# avoid-ai-style-wording

Agent skill that keeps model-written prose plain (no dash pauses, no stock AI phrasing) and keeps UI copy short, actionable, and audience-appropriate.

## Install

```bash
npx skills add minhhung2556/avoid-ai-style-wording
```

With the [GitHub CLI](https://cli.github.com/manual/gh_skill):

```bash
gh skill install minhhung2556/avoid-ai-style-wording
```

## What it covers

- **Prose:** chat, comments, commits, docs, and PR text
- **UI copy:** labels, toasts, validation, empty states, and consent strings (with separate rules for end users, staff, and legal/consent)

Rules live in [skills/avoid-ai-style-wording/SKILL.md](skills/avoid-ai-style-wording/SKILL.md). [examples.md](skills/avoid-ai-style-wording/examples.md) has scoring fixtures.

## License

MIT. See [LICENSE](LICENSE).
