# Test examples for avoid-ai-style-wording

Use these fixtures in a chat where [SKILL.md](SKILL.md) is already loaded. Paste the **Prompt**, read the reply, then score it against **Check**. **Fail** and **Pass** are reference samples; wording may differ if meaning matches **Pass**.

## How to score

1. Paste the prompt only (not the fail or pass blocks).
2. Scan the reply for every item in **Check**.
3. **Pass** if the reply matches **Pass** in meaning and violates none of the checks.
4. **Fail** if the reply matches **Fail** on any check or copies bait from the prompt (dashes, stock phrases, bad UI copy).

---

## Prose

### P1 Em dash (U+2014)

**Prompt**

Write one sentence telling the user the build failed and they should check the logs.

**Fail**

```
The build failed — check the logs.
```

**Pass**

```
The build failed. Check the logs.
```

**Check**

- No em dash (U+2014) in agent prose.
- Two ideas may be two sentences, or joined with comma, colon, or parentheses.

---

### P2 En dash between words (U+2013)

**Prompt**

In one sentence, describe how the app talks to the backend (client and server).

**Fail**

```
Traffic uses a client–server model over HTTPS.
```

**Pass**

```
Traffic uses a client and server model over HTTPS.
```

**Check**

- No en dash (U+2013) joining words or clauses in agent prose.
- Compound recast (for example "client and server") is fine.

---

### P3 Date range exception (U+2013)

**Prompt**

One sentence: the API was stable from 2020 through 2024.

**Fail**

```
The API used a client–server design and was stable from 2020–2024.
```

**Pass**

```
The API was stable from 2020–2024.
```

**Check**

- Numeric or date range may keep en dash (U+2013), for example `2020–2024`.
- No en dash between words or clauses (for example `client–server`) in agent prose.

---

### P4 Horizontal bar (U+2015)

**Prompt**

Tell the user to wait, then retry the upload.

**Fail**

```
Wait ― then retry the upload.
```

**Pass**

```
Wait, then retry the upload.
```

**Check**

- No horizontal bar (U+2015) in agent prose.

---

### P5 Spaced hyphen pause

**Prompt**

The config file is missing. Tell the user to add it before continuing.

**Fail**

```
The file is missing - add it first.
```

**Pass**

```
The file is missing. Add it first.
```

**Check**

- No spaced hyphen used as a pause (`word - word`) in agent prose.
- Hyphens inside compound words (for example `read-only`) in prose are fine when not used as a pause.

---

### P6 Double hyphen pause

**Prompt**

Tell the user to open the file; the path is in the error message.

**Fail**

```
Open the file -- the path is in the error.
```

**Pass**

```
Open the file. The path is in the error.
```

**Check**

- No `--` used as a pause in agent prose.
- `--` inside a command example (for example `git commit --amend`) in code blocks is fine.

---

### P7 Rewrite, do not swap

**Prompt**

Improve this line for a PR comment: `We should merge soon — tests are green.`

**Fail**

```
We should merge soon – tests are green.
```

```
We should merge soon ― tests are green.
```

```
We should merge soon -- tests are green.
```

**Pass**

```
We should merge soon. Tests are green.
```

```
We should merge soon (tests are green).
```

**Check**

- Does not swap em dash for en dash, horizontal bar, or `--`.
- Recasts with split sentence, comma, colon, or parentheses.

---

### P8 Do not mirror context (dashes and stock phrasing)

**Prompt**

It's worth noting that caching helps — let's dive in and unpack how we leverage Redis for a robust, seamless setup. Great question! When it comes to performance, at its core this isn't slow. It's fast. Not only is it fast, but also it's cutting-edge in today's fast-paced world. I'd be happy to delve into details. In conclusion, we should ship.

Explain in two short sentences whether we should enable the cache for this endpoint.

**Fail**

```
It's worth noting that caching helps — let's dive in.
```

```
When it comes to caching, at its core you should enable it for this endpoint.
```

**Pass**

```
Enable the cache for this endpoint. It cuts load on the database for read-heavy traffic.
```

**Check**

- No banned pause characters from the prompt echoed in agent prose.
- None of the listed stock phrases unless literally required by the user question.
- States the recommendation in plain sentences.

---

### P9 Terse reply

**Prompt**

terse: The deploy failed — what should I do first?

**Fail**

```
The deploy failed — check the pipeline logs and fix the first error.
```

**Pass**

```
Check the pipeline logs. Fix the first error.
```

**Check**

- Reply stays short.
- No dash used as a pause, even though the prompt contained one.

---

### P10 Exceptions (code, regex, escape, quote, range)

**Prompt**

Reply with: (1) one line of prose saying the separator is documented below; (2) a JavaScript string literal that contains an em dash between "wait" and "retry"; (3) a regex that matches an em dash; (4) the Unicode escape `\u2014` in a code comment; (5) quote this exactly: `She said "stop — now"`; (6) mention the years 2019–2021 in prose.

**Fail**

```
Wait — see below.
```

(Em dash in the agent's own explanatory prose outside code and the required quote.)

**Pass**

```
The separator is documented below.

const hint = "wait—retry";
const EM_DASH = /\u2014/;
// U+2014 em dash

She said "stop — now"

Rollout covered 2019–2021.
```

**Check**

- Em dash and en dash appear only inside code, string literals, regex, escapes, the verbatim quote, or the date range.
- Surrounding agent prose has no banned pause characters.

---

### P11 Release note (scope beyond chat or PR)

**Prompt**

Write two sentences for a product release note: version 2.4 adds scheduled exports. Do not use marketing filler.

**Fail**

```
In today's fast-paced world, we're thrilled to unveil seamless scheduled exports — let's dive in!
```

**Pass**

```
Version 2.4 adds scheduled exports. Set a cadence in Project settings and downloads run automatically.
```

**Check**

- Plain prose rules apply to release notes and similar product text.
- No banned pause characters or stock phrases from the skill list.

---

## UI copy

For UI cases, the prompt asks for a specific string (toast, label, error, etc.). Score only that string unless **Check** says otherwise.

### U1 End user (no internals)

**Prompt**

Write toast copy for an end user: their account has no role assigned. Audience: end user.

**Fail**

```
E_ROLE_MISSING: worker could not resolve role from session token in Redis.
```

**Pass**

```
Your account has no role yet. Ask your manager to add you by email.
```

**Check**

- Says what happened and what to do next.
- No storage, queues, pipelines, workers, session or token storage, raw error codes, or "the system calculated…".

---

### U2 Staff save error (no DB in toast)

**Prompt**

Write a staff-facing toast when saving a customer record fails. Audience: staff.

**Fail**

```
Save failed: SQL error 23505 on table customers.
```

**Pass**

```
Couldn't save this record. Check the highlighted fields and try again.
```

**Check**

- No "DB", "SQL", or database jargon in the toast.
- Field labels elsewhere in the UI may still say `Customer ID`.

---

### U3 Staff short outcome

**Prompt**

Write the toast after a staff user successfully saves a form. Audience: staff.

**Fail**

```
Your changes have been successfully persisted to the database!
```

**Pass**

```
Saved.
```

**Check**

- Short action outcome; no database mention.

---

### U4 Legal and consent (may name processing)

**Prompt**

Write one sentence for a cookie consent banner. Audience: legal/consent.

**Fail**

```
We store your data securely.
```

**Pass**

```
We use cookies and local storage to keep you signed in; session tokens are hashed and hosted in the EU.
```

**Check**

- May name storage, hashing, tokens, hosting, and similar.
- Does not hide how data is processed with vague marketing.

---

### U5 Do not restate the UI

**Prompt**

Button label is "Export CSV". Write a toast that fires right after the user clicks it. Audience: end user.

**Fail**

```
You clicked Export CSV.
```

**Pass**

```
Export started. Download will begin when the file is ready.
```

**Check**

- Does not restate the button label or obvious action.

---

### U6 No developer-facing error

**Prompt**

Write an end-user error when checkout fails. A message catalog maps `E_CHECKOUT_FAILED`. Audience: end user.

**Fail**

```
Error: NullPointerException at com.example.CheckoutService.charge:142
```

**Pass**

```
Payment didn't go through. Try another card or contact support.
```

**Check**

- No stack trace, internal id, or raw backend message.

---

### U7 No long apology or system blame

**Prompt**

Write an end-user message when the site is down. Audience: end user.

**Fail**

```
We're terribly sorry — our infrastructure team is working around the clock because our servers failed catastrophically due to a bug in the deployment pipeline.
```

**Pass**

```
This page isn't available right now. Try again in a few minutes.
```

**Check**

- No long apology or blame-the-system narrative.
- No em dash or other banned pause in the string.

---

### U8 No marketing on empty state

**Prompt**

Write empty-state copy when the user has no projects yet. Audience: end user.

**Fail**

```
Unlock seamless productivity — create your first project today!
```

**Pass**

```
No projects yet. Create one to get started.
```

**Check**

- No marketing tone; gives a clear next step.
- No banned pause or stock marketing words from the skill list.

---

### U9 Do not blame the user

**Prompt**

Write validation under a password field: minimum length not met. Audience: end user.

**Fail**

```
Obviously you need at least 12 characters.
```

**Pass**

```
Use at least 12 characters.
```

**Check**

- Does not blame the user or imply the mistake is trivial.

---

### U10 Catalog mapping (no hardcoded string)

**Prompt**

Return the user-visible error for code `E_ROLE_MISSING`. A message catalog exists; use key `errors.role_missing`. Audience: end user.

**Fail**

```
Your account has no role yet. Ask your manager to add you by email.
```

(Hardcoded English in code or API response instead of the key.)

**Pass**

```
errors.role_missing
```

Or pseudocode:

```
showMessage(t('errors.role_missing'));
```

**Check**

- Maps code to catalog key; does not hardcode the final string when a catalog exists.

---

### U11 Delete machine-only sentence

**Prompt**

Write staff table footer copy: "Showing 24 of 100 records." The total is computed by the server. Audience: staff.

**Fail**

```
Showing 24 of 100 records. The total was calculated by aggregating all rows in the database.
```

**Pass**

```
Showing 24 of 100 records.
```

**Check**

- Drops the sentence that only explains the machine.
- Does not replace it with a longer sentence with the same meaning.

---

### U12 Abbreviation with hint

**Prompt**

Write a column header for stock keeping unit. Staff see the table daily. Audience: staff.

**Fail**

```
Stock keeping unit (SKU)
```

**Pass**

```
SKU
```

With hint (tooltip or `aria-describedby`): `Stock keeping unit`

**Check**

- Keeps `SKU` on screen.
- Full expansion in hover or focus hint, not repeated in the header.

---
