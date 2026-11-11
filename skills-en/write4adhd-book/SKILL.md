---
name: write4adhd-book
description: Write ADHD-friendly, jargon-free explainers, docs and chaptered "books" in the demystified style - conversational tone, short chapters, one homespun metaphor running through the whole piece, a bolded closing line for every chapter. Output is a single self-contained HTML page, a multi-page HTML book site (inline Canvas diagrams/animations, light & dark themes), or Markdown. Use when the user asks for a plain-English explainer, primer, crash course, ELI5, demystification or beginner-friendly intro to a technical concept or project, or explicitly asks for the "write4adhd-book / demystified style". This skill builds on write4adhd-writing (the plain-writing base layer) and must be installed together with it.
---

# write4adhd-book: writing in the demystified style

Write explainers and docs for readers with zero background. The goal is not to train them to operate the thing — it's to **bring the barrier down to where the reader already stands**: they finish able to say in their own words what the thing roughly is, and it no longer feels out of reach. "That's it?" is a reaction the reader has on their own — **it is not a line you put on the page.**

Style is the means, never the end: metaphors, self-posed questions, closing lines and quips all exist to make things clearer, and the prose only passes if it sounds like a person talking properly. Announcing "it's really not complicated" over and over just makes the piece feel forced — let the content prove it isn't. Every book-form style rule and worked example lives in `references/style-guide.md`; the plain-writing base layer lives in `write4adhd-writing`.

## Division of labour with write4adhd-writing

This skill is installed on top of `write4adhd-writing`, and the responsibilities do not overlap:

- **`write4adhd-writing` owns "plain and short"**: plain words, conclusion first, weighting effort by importance, the ADHD-friendly layout rules, the general part of the never-do list.
- **This skill owns "book form and flourish"**: the one homespun core metaphor, a closing line per chapter, the three-step term intro, self-posed questions, the chapter/page rhythm, the HTML / book-site templates and craft rules.

**Before writing a single word you must read `write4adhd-writing` in full**: find its SKILL.md in the agent's skills list, and read its `references/plain-style.md` along with it; if you can't find it, stop and tell the user it needs to be installed — don't half-execute from memory.

This skill's rules are additions on top of it; where they conflict, this skill wins.

## Workflow

1. **Nail down three things** (ask the user, or infer sensible defaults):
   - the topic and the audience (default: a curious reader with no technical background);
   - the scale and the output format: a short piece (Markdown or a single self-contained HTML page, skimmable in ~15 minutes) or a chaptered "book" (a multi-page HTML book site, one page per chapter) — see "Choosing a format" in `references/html-craft.md`;
   - if it's a book and you need to explore a technical project/codebase first, explore it thoroughly — but **only write the layer the reader actually needs**. The implementation details you dug up exist to make your own intuition solid, not to be dumped into the prose.
2. **Read two layers before writing**: first `write4adhd-writing`'s SKILL.md and `references/plain-style.md`, then this skill's `references/style-guide.md` — style is the soul of this skill, and both layers must be worked through rule by rule.
3. If the output is HTML, **also read** `references/html-craft.md` and copy the matching template as your starting point (single page: `assets/skeleton.html`; book site: the whole `assets/site/` directory). The templates already ship with the full stylesheet, the components, the Canvas drawing helpers, theme switching and the animation-control scaffolding — never hand-write a page from scratch.
4. While outlining, settle these first: **the one core metaphor that runs through the whole piece**, the **closing line** for each chapter, a **plain-words caption** for every diagram, and the **title pair on every page** (the h1 names the subject plainly; the .subtitle is where the core metaphor comes in). For a book, also settle the **page split**: 1–3 sections per page, and one hooky one-line blurb per chapter on the table-of-contents page. These are the skeleton — think them through before you write prose.
5. A book doesn't have to be written in one sitting: produce it in batches, "TOC page → chapter by chapter", so each finished page can be opened and reviewed on its own. But the core metaphor, the term choices and the voice of the closing lines must stay consistent across the whole book — before writing the next chapter, re-read the intro and closing line of the ones you already finished.
6. When you're done, run `write4adhd-writing`'s self-check list, then go through the **self-check list** at the end of `references/style-guide.md` item by item (page by page for a book, plus one whole-book consistency pass). For HTML output, check layout and diagram rendering under both light and dark themes in a browser (or with playwright MCP screenshots).

## The four non-negotiables (on top of write4adhd-writing's four)

1. **One homespun core metaphor runs through the whole piece** (cloud storage = "a locker you rent inside someone else's gym"), and every later explanation ties back to it.
2. **Every chapter ends with one bolded closing line** that compresses the chapter into something the reader can carry away — an instant "I got it" reward. Let the shape follow the content: **never reuse the same template every chapter** (three "Bottom line:" chapters in a row reads like a machine), and when it won't compress into one pretty sentence, two plain accurate ones are better.
3. **The three-step term intro**: describe the phenomenon in plain words first → then name the jargon (bolded, plus the original term) → stop right there. No implementation details, no edge cases.
4. **Intuition over formulas.** A cheesy metaphor beats an abstraction every time. On questions that are genuinely unsettled, say "nobody knows" out loud instead of faking authority.

Output language: if a global instruction (AGENTS.md, system prompt) states a language requirement, follow it; otherwise match the language the user used in their prompt.
