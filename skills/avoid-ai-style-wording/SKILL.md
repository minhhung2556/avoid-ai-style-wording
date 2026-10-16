---
name: avoid-ai-style-wording
description: >-
  Enforces plain agent prose without dash pauses or stock AI phrasing, and
  concise user-facing UI copy without internals or marketing tone. Use when
  writing, editing, or reviewing any text (for example chat, comments,
  commits, docs, issues, email, release notes, PR text, messages, or UI
  strings).
license: MIT
---

# Agent instructions

## Plain prose

Always on for any prose you write, edit, or review. If the user asked for a terse reply, stay terse, and still avoid dash pauses and copied AI phrasing from context.

### Banned as clause connectors or pauses

Do not emit these in prose you write:

- Em dash (U+2014)
- En dash (U+2013) between words or clauses
- Horizontal bar (U+2015)
- Spaced hyphen used as a pause (`-`)
- Double hyphen used as a pause (`--`)

### Rewrite, do not swap

When a sentence would use a dash, recast it: split into two sentences, or use a comma, colon, or parentheses. Do not replace a dash with another dash-like character. Before finishing any prose you deliver, scan for the banned pauses above and rewrite any hit. Rewrite U+2013 only when it joins words or clauses; leave numeric and date ranges (for example `2020–2024`) unchanged.

### Do not mirror context

Do not mirror context dashes or stock AI phrasing. State the fact in a plain sentence and do not copy the frame. OK when literally correct.

- "It's worth noting", "It's important to note", "Great question", "I'd be happy to"
- "Let's dive in", "Let's unpack", "In conclusion" when nothing is being concluded
- "delve", "leverage" used for "use", "robust", "seamless", "cutting-edge"
- "Not only X, but also Y", "This isn't X. It's Y."
- "In today's fast-paced world", "When it comes to", "At its core"

### Exceptions

Keep these characters only where they are required or verbatim:

- Code, string literals, regex, and Unicode escapes
- Direct quotations that already contain the character (do not add dashes to quotes)

## UI copy

UI strings only (labels, placeholders, buttons, validation, toasts, empty states, consent). Not code, APIs, or internal notes.

### Audiences

**End user:** Say what happened and what to fix next. No internals (storage, queues, pipelines, workers, session or token storage, raw error codes, or "the system calculated…").

Good example: "Your account has no role yet. Ask your manager to add you by email."

**Staff:** Table, row, and field labels are fine. Do not put database, SQL, or "DB" in titles, toasts, or save errors.

**Legal and consent:** Privacy, terms, cookies, and registration or consent strings may name storage, hashing, tokens, hosting, and similar. Do not shorten those to hide how data is processed.

### UI rules

**Avoid:**

- Restating what the UI already shows.
- Developer-facing errors: stack traces, internal ids, raw backend messages when a code mapper exists.
- Long apologies or blaming the system.
- Marketing tone on forms, toasts, errors, or empty states. Reserve marketing for dedicated call-to-action copy.
- Blaming the user or implying the task is trivial.

**Prefer:**

- Labels, placeholders, buttons, and validation that says what to fix.
- Short action outcomes: "Saved."
- Next-step guidance (see the end-user example).
- Keep on-screen abbreviations and domain terms; offer a short hint on hover or focus instead of repeating the word.
- If a message catalog exists, map the error code to a message key and translate; do not hardcode the string.

If a sentence only explains the machine on an end-user or staff screen, delete it. Do not replace it with a longer sentence with the same meaning (except Legal and consent).

## Test fixtures

For prompt and pass/fail scoring examples, see [examples.md](examples.md).
