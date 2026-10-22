# Job Scout — 90-second demo script

Two n8n workflows that find jobs and prepare honest applications.
**Job Scout** finds and scores jobs. **Application Package** tailors my resume for one job, without letting the AI invent experience.

Every claim below matches what the workflows actually do. No invented numbers.

---

## The script (about 90 seconds)

### 0:00 – 0:08 · Hook
**Screen:** inbox full of job alert emails → quick cut to a finished resume PDF. On-screen text: *"My AI writes my resume. It can't lie on it."*
**Say:**
> "I built an AI that tailors my resume for every job, and it's not allowed to invent a single skill. Here's how it works."

### 0:08 – 0:28 · Workflow 1: Job Scout
**Screen:** Job Scout canvas → Google Sheet with scores and reasons.
**Say:**
> "The first one is Job Scout. It reads my job alerts from Indeed, LinkedIn and Rozee. An LLM pulls out every job, scores it from 1 to 10 against my profile, and logs it here with a reason. Every job is checked against the original email, so nothing made-up gets in."

### 0:28 – 0:42 · From email to form
**Screen:** Gmail alert for an 8+ job → click "Prepare application" → form opens already filled in → paste the job description → Submit.
**Say:**
> "Jobs scoring 8 or more are emailed to me with one link. It opens a form that's already filled in. I paste the full job description and submit."

### 0:42 – 1:05 · Workflow 2: Application Package
**Screen:** Application Package canvas → Package Doc → resume PDF.
**Say:**
> "The second workflow rewrites my resume bullets for that job, builds a PDF from my own template, drafts a cover letter and a LinkedIn message, finds the recruiter search, and logs it all in my tracker."

### 1:05 – 1:20 · The guardrail
**Screen:** the Guardrail node → its output with flags.
**Say:**
> "AI likes to stretch the truth on a resume. So every bullet is checked against my real resume. If it adds a skill or a claim I don't have, it's swapped back to my original."

### 1:20 – 1:30 · Close
**Screen:** the "Before you apply" checklist in the Package Doc.
**Say:**
> "It never applies for me. It prepares, I check, I decide. The full build, including the mistakes I made along the way, is on my GitHub."

---

## Shot list (prepare before recording)

1. Gmail inbox with several job alert emails
2. Job Scout canvas, zoomed so node names are readable
3. Google Sheet with title, company, score and reason columns
4. A Job Scout email with the "Prepare application" link
5. The pre-filled form
6. Application Package canvas
7. A finished Package Doc and resume PDF
8. Guardrail output showing its flags

## Before you record

- Blur or crop: phone number, email address, Sheet and Doc IDs in URLs, credential screens.
- Browser zoom 125%, bookmarks bar hidden, other tabs closed.
- Record with Win + Alt + R (Xbox Game Bar) or the Snipping Tool's record mode.
- Speak slowly and pause between scenes, so cuts are easy.
- Add captions (CapCut can do it automatically). Most people watch on mute.

---

## Mistakes I made while building it, so you can avoid them

| Problem | Cause | Fix |
|---|---|---|
| Fake jobs appeared in the sheet | The LLM filled in jobs when the email had none | Empty-list rule in the prompt, plus a filter that keeps a job only if its title appears in the source email |
| Groq rate-limit errors (429) | Each email was about 5,700 tokens, mostly tracking links | Cleaned links before the LLM, capped the input, batched requests, and used a smaller model for scoring |
| Tailored resume claimed work I never did | The guardrail only checked tool names and numbers | Novelty check: a bullet with more than 3 words not found anywhere in my real resume is replaced with the original |
| A check passed "end-to-end" when it should have flagged it | The LLM used Unicode non-breaking hyphens, so the text didn't match | Normalize dashes and spaces before every comparison |
| Fixes didn't show up in real runs | The Production URL runs the last *published* version | Save → Publish after every change |
| Google Docs API error: "must not be an Office file" | The resume template was a .docx, not a Google Doc | Convert the template to a native Google Doc and check its mimeType |

## Limits (honest)

- Runs on my laptop: when n8n is closed, nothing runs and the form link doesn't open.
- The guardrail catches added words, not every subtle change of meaning. I still read every bullet before sending.
- The cover letter is a draft. I write the final version myself.

## Stack

n8n · Groq (GPT-OSS 120B and 20B) · Gmail API · Google Sheets, Docs and Drive APIs · OAuth 2.0
