---
name: write4adhd-writing
description: Write plain, ADHD-friendly documents people actually finish reading: research reports, experiment reports, summaries, memos, technical notes. Conclusion first, plain words instead of jargon, short paragraphs, no undifferentiated information dumps. Use when the user asks for a report, a summary, a memo, or "turn this into a document", or names write4adhd-writing. For beginner explainers and chaptered books, use write4adhd-book instead.
---

# write4adhd-writing: plain writing

Write **finished documents**: research reports, experiment reports, retrospectives, memos, technical notes. The reader wants "what happened, what's the conclusion, what's next" — not a pleasant read. The flourishes (a homespun metaphor running through everything, closing lines, Canvas animations) belong to `write4adhd-book`; none of that here.

The goal in one line: **a reader with no patience can skim just the bold text and the headings and still get every point; a patient reader doesn't have to dig the points out of filler.**

## Division of labour with write4adhd-book

This skill is the base layer; `write4adhd-book` is installed on top of it. No overlap:

- **This skill owns "plain and short"**: plain words, conclusion first, weighting effort by importance, the ADHD-friendly layout rules.
- **`write4adhd-book` owns "book form and flourish"**: the one homespun core metaphor, a closing line per chapter, the three-step term intro, self-posed questions, the HTML / book-site templates and craft rules.

The user wants a beginner explainer or a chaptered book → use `write4adhd-book` instead (it requires reading this skill in full before writing; if it can't find it, stop and tell the user to install it).

## Workflow

1. **Nail down three things** (ask the user, or infer sensible defaults):
   - who this document serves and what it solves (default: a record of facts + a conclusion, for your boss, your colleagues, or your future self);
   - the **one or two things the reader must walk away with** — if you can't write them down, don't start writing; you haven't found the point yet;
   - output format: a Markdown file by default; only switch if the user explicitly asks for something else (e.g. HTML).
2. **Read `references/plain-style.md` before writing a single word.** It is the soul of this skill; work through it rule by rule.
3. **Skeleton before prose**: write the TL;DR first (one paragraph, or no more than three bullets, conclusion first), then the heading hierarchy, then the key numbers and facts each section carries — and only then the body. If the skeleton doesn't stand, go back to step 1 and re-find the point.
4. Keep **subtracting** while you write: side topics get one clause or move to an appendix; any paragraph whose usefulness you're not sure about gets deleted by default.
5. When done, run the **self-check list** at the end of `references/plain-style.md` item by item.

## The four non-negotiables

1. **Plain words.** Translate jargon into plain language; never explain jargon with jargon. The first time a term the reader may not know appears, give it a one-line plain-words gloss.
2. **Short**: short sentences, short paragraphs (never more than 4 consecutive lines of plain text), restrained length; past that, break it up with a heading, a list, or bolded keywords.
3. **The point first**: conclusion up top, effort weighted by importance, never even-handed across everything; delete every urge to be exhaustive.
4. **No faking authority**: for conclusions you're not sure about, mark the confidence or write "not confirmed yet", and give the reader a way to check — never dress up a guess as a fact.

Output language: if a global instruction (AGENTS.md, system prompt) states a language requirement, follow it; otherwise match the language the user used in their prompt.
