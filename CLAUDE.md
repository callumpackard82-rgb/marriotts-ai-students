# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

A self-paced **student AI-literacy website** for Marriotts School (the student-facing sibling of the
staff site at `../AI training` / https://marriotts-ai-training.netlify.app). **Static HTML only** —
no framework, no build step, no dependencies. Every page is a self-contained `.html` file with all
CSS and JS inline.

**Crucial context:** the school's AI policy **bans students from using AI on the school network and
school devices**. This site does not undermine that — it exists because students use AI *outside*
school where the school has no control. Every module is honest about the ban, never sets a task that
requires an AI tool (all interactives are in-browser JS), and teaches the school's core principle:
**AI is the copilot, never the pilot — you never outsource your thinking.**

- `index.html` — the hub / dashboard (pathway picker, progress, links to every module)
- `essentials-*-module.html` etc. — the training modules

## Architecture

**Three theme pathways, 12 planned modules**, one audience (Y7–13). Same 7/8-section module anatomy
as the staff site (why it matters → the idea → see it in action → your turn → 🛡️ safety checkpoint →
self-check → where next). Pathway triad reuses the Marriotts staff palette:
**Essentials = coral-red `#d0452e`**, **Thinking = slate-teal `#3f6e72`**, **Creating = charcoal
`#565b5e`**.

Module plan (built ✅ / planned ⏳):
- **Essentials**: E1 what-is-ai ✅ → E2 rules-and-risks ✅ → E3 spot-the-fake ✅ → E4 copilot-not-pilot ✅
- **Thinking**: T1 study-smarter ⏳ → T2 homework-and-honesty ⏳ → T3 question-everything ⏳ → T4 ai-and-you (wellbeing) ⏳
- **Creating**: C1 create-not-copy ⏳ → C2 build-something ⏳ → C3 share-with-care ⏳ → C4 ai-and-your-future ⏳

The hub lists all 12; unbuilt ones render as "coming soon" (no links). `TOTAL` in `index.html` counts
**available** modules only — update it when adding modules.

**Branding.** Identical to the staff site: coral `#d0452e` + grey `#73797c` from `logo.svg` (repo
root); logo in every header in a white rounded tile beside the eyebrow **"Marriotts School · Student
AI Guide"**; `<link rel="icon" href="logo.svg">`. New pages must include both.

**Progress tracking.** Same pattern as the staff site but a **different storage key** so the two
sites never collide: `localStorage['saig_done']` = JSON array of completed module ids (e.g.
`["E1","E3"]`). There is no tool-preference key (students don't pick a tool — the site never asks
them to use one). The hub re-reads progress on `pageshow`, `focus`, `visibilitychange` and `storage`
events, shows a red banner if localStorage is blocked, and has a progress-code export/import panel
(base64 of `{d:[ids]}`).

## CRITICAL: Netlify corrupts inline `onclick` on `<a>` tags

Inherited, live-verified lesson from the staff site: Netlify's "Pretty URLs" optimisation (forced on
in the UI, `netlify.toml` skip_processing does NOT reliably override it) rewrites `<a href>` tags and
invalidly escapes single quotes inside inline `onclick`, silently breaking handlers on the live site
only. **Never put inline `onclick` (or any quote-containing inline JS) on an `<a href>` tag.**
Save links use `data-done="<ID>"` + a script-block listener that writes to `saig_done`. Inline
`onclick` on a `<button>` is safe — only anchors are rewritten.

## Local development

No build/lint/test. Serve over HTTP (localStorage needs a real origin):

```bash
python -m http.server 8766      # then open http://localhost:8766/
```

(Port 8766 — the staff site uses 8765, so both can run at once.)

## Deployment

GitHub → Netlify continuous deploy, same workflow as the staff site: edit → commit → push `main` →
live in ~1 min.

| Thing | Value |
|---|---|
| GitHub repo | https://github.com/callumpackard82-rgb/marriotts-ai-students |
| Deploy branch | `main` |
| Live URL | (Netlify site — see repo's Netlify dashboard) |
| Build command | *(none — static site)* |
| Publish directory | repository root |

After any change affecting the live site, verify by fetching the deployed page (Netlify can transform
HTML).

## Content conventions — student-facing (different from the staff site!)

- **Audience is students, Y7–13.** Tone: direct, respectful, never preachy, no forced slang. Treat
  them as capable of judgement — the site's whole premise is "we can't control what you do outside
  school, so we'll be honest with you instead."
- **Never set a task that requires using an AI tool.** Students can't use AI at school and many are
  under the tools' age limits. Every "Your turn" is an in-browser interactive (sorters, spot-the-fake
  quizzes, reveals) that works with zero external tools. At most, an optional "at home, if you're old
  enough and a parent/carer knows" aside.
- **Be accurate about age limits** (quarantined in the dashed "Current as of {month year}" panel in
  E2 for termly refresh): ChatGPT/Gemini/Copilot/Meta AI 13+, **Claude 18+**. Under-18s generally
  need parent/carer consent. Don't recommend or link tools — name them factually.
- **Safeguarding content must stay firm and specific** (E3): making sexual deepfakes of anyone,
  including classmates, is a crime even for under-18s; victims should save evidence, tell a trusted
  adult, and use Childline/IWF **Report Remove** and **CEOP**. Don't soften or cut this in edits.
- **Academic integrity:** GCSE/A-level references use JCQ malpractice language (unacknowledged AI in
  coursework/NEA can void a qualification). Exams are handwritten, no AI.
- The copilot-not-pilot principle and the "three lines that never move" (not at school / nothing
  personal / your brain does the thinking) run through every module — the student equivalent of the
  staff site's data-safety spine.
- Keep links between pages **relative**. Don't add a build step. Don't commit any real student data
  or real student names in examples.
