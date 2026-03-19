# CLAUDE.md — GenAI Tool Selection Guide

## Project overview

A React single-page application that helps Mornington Peninsula Shire staff
select the right AI tool for their work tasks and build better prompts.
Built and maintained by Justin Daly, Emerging Technologies Lead.

**Live audience:** Non-technical council staff. All language, UI copy, and
recommendations should reflect that — plain English, no jargon, practical
and honest.

**Current version:** v3.0 prototype
**Status:** Functional and deployed. Active development towards v2 (production build).

---

## How to run it locally

No build step required in the current prototype:

```bash
# Option 1 — just open in browser
open index.html

# Option 2 — local dev server with hot reload
npx serve .
```

The app runs entirely in the browser. There are no environment variables,
no API keys, no backend, and no database.

---

## Architecture

**Single-file SPA** — the entire application lives in `index.html`:
- React 18.3.1 and ReactDOM loaded from unpkg CDN (pinned versions)
- Babel Standalone 7.24.0 transforms JSX in the browser at runtime
- All data (tools, tasks, questions, glossary) is embedded directly in the file
- All state managed via React `useState` — no external state library
- No routing library — view switching is a `view` state string

**Main views:** `home`, `flow`, `result`, `search`, `tool-profile`,
`reference`, `compare`, `builder`, `glossary`

**Key data structures (all in `index.html`):**
- `tools` — 5 AI tool profiles (ChatGPT, Claude, Perplexity, Gemini, Copilot Chat)
- `taskData` — 37 task-to-tool mappings
- `questions` — wizard decision tree
- `costarSteps` — CO-STAR prompt builder framework (6 steps)
- `comparisonFeatures` — 11-feature comparison table
- `glossaryData` — 5 categories, 20+ plain-language definitions

---

## Deployment

**Repository:** `github.com/justinpdaly/genai-tool-guide`
**Hosting:** Cloudflare Pages (static file, auto-deploys on push to `main`)

Pushing to `main` triggers an automatic Cloudflare Pages deployment.
No manual steps required once the GitHub connection is active.

**Current build settings in Cloudflare Pages:**
- Framework preset: None
- Build command: *(empty — will change to `npm run build` when Vite is added)*
- Build output directory: `/`

When Vite is added (v2), update Cloudflare Pages settings to:
- Build command: `npm run build`
- Build output directory: `dist`

**Cloudflare Web Analytics:** A placeholder script tag exists in `index.html`
(commented out). To activate, get the token from the Cloudflare Pages
dashboard → Analytics, and uncomment the line replacing `YOUR_TOKEN_HERE`.

---

## Critical design decisions — do not change without discussion

**1. Sensitive information must not go into any AI tool — including Copilot Chat.**
Enterprise protection (EDP via Copilot Chat) is a *data security* property —
it means data stays within Microsoft's enterprise boundary and isn't used for
model training. It does **not** define or satisfy privacy obligations.
Genuinely sensitive information (personal data about residents, confidential
legal or HR matters, etc.) should not be entered into any AI tool. The wizard's
first question must route these users away from AI entirely — to Data Security
or their relevant privacy obligations — not to Copilot Chat as a "safe"
alternative.

The current prototype incorrectly recommends Copilot Chat for sensitive tasks.
This is a known content error to be corrected — see the roadmap below.
Do not replicate or reinforce this pattern elsewhere in the app.

**2. The wizard's first question is always the sensitive-data gate.**
This must remain the very first question in the decision tree. The outcome
needs to be corrected (see above), but its position as the first gate is right.

**3. Tool profiles must be honest about limitations.**
Every tool profile includes a `limitations` array. These should not be softened
or removed — transparency about what tools can't do is a core feature for
a staff-facing governance tool.

---

## Working conventions

### Before making any code change
Explain what the change does and why in plain language before applying it.
Wait for confirmation before proceeding. Justin wants to understand every
change — this is a learning-oriented collaboration, not just output delivery.

### Commits
Use one commit per logical change. This project follows a clean, readable
git history. Commit messages should:
- Use an imperative subject line (`Add`, `Fix`, `Update`, not `Added` or `Adding`)
- Explain the *why* in the body if the change isn't self-evident
- Never bundle unrelated changes together

```bash
# Correct — one logical change per commit
git commit -m "Fix wizard sensitive-data routing to redirect away from AI tools"

# Wrong — bundled unrelated changes
git commit -m "Various improvements"
```

When changes are already mixed in a single file, revert to the last clean
commit and re-apply changes one at a time rather than using a single bulk commit.

### Edits
- Always read a file before editing it
- Explain what a change does before making it — wait for confirmation
- Prefer editing existing files over creating new ones
- Don't add comments, docstrings, or refactor code that wasn't part of the task
- Don't add error handling for scenarios that can't happen

### Keep it simple
This is a prototype serving a focused, practical purpose. Don't over-engineer.
The right amount of complexity is the minimum needed for the task at hand.

---

## Roadmap — v2 (production build)

These improvements are planned in priority order. Check git log for what's
already been completed before suggesting or starting any of these.

| # | Change | Complexity | Notes |
|---|--------|------------|-------|
| 1 | ~~Add error boundary~~ | Done | |
| 2 | ~~Pin CDN versions~~ | Done | |
| 3 | ~~Cloudflare Web Analytics placeholder~~ | Done | |
| 4 | ~~localStorage for prompt builder~~ | Done | |
| 5 | Fix sensitive-data wizard routing | Medium | **Priority** — current behaviour actively misleads staff; must redirect away from AI tools, not to Copilot Chat |
| 6 | Remove Babel / add Vite build step | Low–medium | |
| 7 | Separate data from UI (tools.js, tasks.js, etc.) | Medium | |
| 8 | Split monolith into sub-components | Medium | |
| 9 | Improve search with Fuse.js | Medium | |
| 10 | URL-based state sharing for results/prompts | Medium | |
| 11 | User feedback mechanism ("was this helpful?") | Medium | |
| 12 | CMS or JSON data source for tool info | High | |

---

## Things to watch out for

- **The sensitive-data wizard route is wrong.** The current app routes sensitive
  tasks to Copilot Chat. This is a content error — do not replicate this pattern.
  See Critical design decisions above.

- **Pricing and model info goes stale fast.** AI tools change their pricing and
  models frequently. The tool data in `index.html` reflects early 2026 — flag
  anything that looks outdated.

- **Babel is still in production.** Until Vite is added, Babel Standalone (~500KB)
  loads on every page visit. This is a known issue, not something to work around —
  it will be fixed properly with the Vite migration.

- **The file is ~4,000 lines.** Be precise with edits. A misplaced bracket or
  unclosed tag can silently break rendering with no visible error in the browser.

- **No tests exist.** Manual testing is required for any change. Open the app
  in a browser and walk through the affected feature before committing.

---

## Contact

Justin Daly — Emerging Technologies Lead, Mornington Peninsula Shire
