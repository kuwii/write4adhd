---
name: write4adhd-dev-onboarding
description: Write developer onboarding docs / newcomer books for a software project: survey the unfamiliar codebase with a six-layer protocol (shape, history, entry points, an end-to-end trace of one call, the build-and-test loop), then write a chaptered primer that takes a junior developer who can code but knows nothing about this domain from zero to "can change the code" — build it locally, find the file a feature or error lives in, run a single test, submit a first change. Output formats match write4adhd-book (self-contained HTML / multi-page book site / Markdown) and follow its demystified style; this skill only says what to investigate and what to write, and is installed alongside write4adhd-book. One sentence triggers it — "help me get up to speed on this project", "walk me through this codebase" — no parameters needed; also for newcomer/contributor guides, codebase tours, architecture primers, or when the user names write4adhd-dev-onboarding. All prose produced is in English.
---

# write4adhd-dev-onboarding: the project onboarding book

Write onboarding docs for **someone about to take over a software project**. The reader profile is specific: **they can code** (junior developer / student / researcher) **but know nothing about the domain this project lives in**.

The goal is not "they understood it" — it's **they can start changing code after reading**: build it on their own machine, follow a feature to the file that needs changing, get a single test to pass, walk a submission from start to finish.

So this is not a popular explainer, it's **job training**. An explainer only needs the reader to stop finding the thing intimidating; job training needs them filing a PR tomorrow.

## Division of labour with write4adhd-book

This skill is used together with `write4adhd-book`, and the responsibilities do not overlap:

- **`write4adhd-book` owns "how to write"**: the demystified voice, the ADHD-friendly rules, the HTML / book-site templates and craft rules.
- **This skill owns "what to investigate, what to write"**: how to get your bearings in an unfamiliar codebase, plus the chapter blueprint and hard rules for an onboarding book.

**Before writing a single word you must read `write4adhd-book` in full**: find its SKILL.md in the agent's skills list, and read its `references/style-guide.md` and `references/html-craft.md` along with it, then write by its rules.

This skill's rules are **additions and overrides on top of it**; where they conflict, this skill wins (the reader profile is different). There are exactly five override clauses — see `references/book-blueprint.md` section 0. Everything else is inherited.

## Works from a single sentence

Users often say just **"help me get up to speed on this project"** and hand you nothing else. That is normal usage, not missing information: **do not fire back a list of questions**. Start with the defaults below and report them in one sentence before you begin ("Going with defaults: …; say the word now if you want any of these changed").

Only two situations justify a question: the current directory holds several repos or candidate projects; or the user's own words contain contradictory requirements. Everything else is inferred:

| Item | Default |
|---|---|
| Codebase | the current working directory (or the one the user named) |
| Reader | can code, zero background in this domain |
| Format | multi-page HTML book site; switch only if the user explicitly says "commit it into the repo / Markdown please" (see blueprint section 9) |
| Output dir | `./onboarding-book/` (`onboarding-book-2/` if taken) |
| Survey notes | `./.notes/`; use the project's own gitignored scratch dir if it has one |
| Scope | one spine for depth + full subsystem-inventory coverage for breadth; chapters split and grouped into parts automatically per blueprint sections 1–2, without asking |
| Environment | probed by you (workflow step 2); never assume the user knows what the machine lacks |

## Workflow

1. **Broadcast first, then start**: before **any survey action**, tell the user this run's defaults and output locations in one sentence (before that, only `ls`/`git`-style lookups to confirm where the codebase is are allowed), e.g. "Going with defaults: reader = can code but new to this domain; deliverable = multi-page HTML book site in `onboarding-book/`; survey notes in `.notes/`; say the word now if you want any of these changed, otherwise I'm starting." Send it, then begin the survey. Do not wait for confirmation — silence is consent.
2. **Survey**: read `references/survey-protocol.md`. **First thing: probe the environment.** Infer the toolchain from the build config (`pom.xml`→java/maven, `build.sbt`→sbt, `package.json`→node, `go.mod`→go, `pyproject.toml`→python…), check each with `command -v`, and record the result as the first row of notes layer 6; whatever is missing puts the "⚠️ not run on this machine" rule in force from that moment on — **the user never has to tell you what the machine lacks**. Then work through the six layers and write findings into survey notes (copy `assets/survey-notes-template.md` into a scratch area, writing each layer as you finish it). **No notes, no writing** — every technical claim in an onboarding book must trace back to a real search, read or run.
3. **Fix the spine, the inventory, the parts**: read `references/book-blueprint.md`. First fix the end-to-end main line (the spine, depth axis); then check the subsystem inventory's coverage and space allocation (breadth axis: every subsystem gets at least a first layer of context, and the area the user named gets a full chapter); then cut the table of contents from the chapter blueprint and **group it into 4–6 parts**; finally write the TOC page (grouped by part, one hook per chapter + one "after this you can do X").
4. **Write**: produce page by page, following `write4adhd-book`'s voice and craft rules. For HTML output, **also read `references/markup-kit.md`** and **additionally copy this skill's `assets/code-kit.css`** (book site: a second `<link>` next to `style.css`; single page: merge its contents into the inline `<style>`). It provides the developer-doc components — code blocks, file trees, tables, hands-on task boxes — reusing the same CSS variables.
5. **Self-check**: first pass `write4adhd-book`'s checklist, then the **onboarding checklist** at the end of `references/book-blueprint.md` — especially "re-verify code citations against the repo one by one" and "was every command actually run".
6. **Acceptance**: the most reliable way is to have someone who has never touched this project (or another agent playing "just learned to program, knows nothing about this domain") read it from the top and actually do the hands-on tasks. Where they get stuck is a defect in the book, not in the reader.

## The seven non-negotiables

1. **No survey, no writing.** Never guess content from the project's name, from common sense, or from training memory. A guessed onboarding doc is worse than none — it walks the newcomer into a ditch with total confidence.
2. **Every technical claim must be traceable.** Cite code as `file:line` and state on the cover page which commit the book is based on; run the commands you give yourself, and mark the ones you can't run with an explicit ⚠️ not-run-on-this-machine.
3. **Don't fake understanding.** For what you didn't get, write "I did not fully confirm this; start looking at X" and give the entry point for checking. An honest "I don't know" is a feature of this book, not a flaw.
4. **One 5–15 minute hands-on task per chapter**: reproducible, with visible feedback, and recoverable with git if it goes wrong. Readers learn with their hands, not their eyes.
5. **Prerequisites just-in-time**: only backfill what later chapters actually use, and point back to it at the point of use. Do not write a computer-science course.
6. **Layered reading**: mark every section "must understand" or "skippable on a first pass", and control the reader's load with layers instead of by deleting content.
7. **No breadth debts.** The demystified style controls the reading barrier, not the amount of knowledge: the reader is going to own part of this project, so subsystems off the spine, the global shape and the core classes' responsibility surfaces must all have a landing spot (full chapter / tour section / one map line + one glossary row). Pay debts by **adding chapters and grouping them into parts**, never by deleting knowledge to make the book shorter.

Plus the five non-negotiables inherited from `write4adhd-book` (one core metaphor, short, a closing line per chapter, the three-step term intro, intuition first), with exactly one situational adjustment: **the reader is a programmer, so analogies may use code they have already written and tools they have already used** (a for loop, git, an HTTP request, a database index) — but the book still needs one homespun, everyday core metaphor to carry the overall architecture.

All prose you produce with this skill is in English.
