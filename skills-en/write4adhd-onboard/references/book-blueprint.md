# Onboarding book blueprint

This document says **what to write**: the book's skeleton, what every chapter must contain, how to present code and commands, and the pre-delivery onboarding checklist.

**How to write** (voice, ADHD-friendly rules, HTML/book-site craft) is not here — that is `write4adhd-book`'s job. Before writing, read its SKILL.md and its `references/style-guide.md` and `references/html-craft.md` in full; this document's rules stack on top of them.

## 0. The five overrides of write4adhd-book

Only these five differ. Everything else (paragraphs ≤ 4 lines, a closing line per chapter, the three-step term intro, soft recaps, plain-words captions, TOC hooks, the navigation trio, Canvas diagram rules, animation never holding attention hostage…) is **inherited unchanged**.

1. **"Stop right there, no implementation details" → you do explain implementation details.** The first two steps of the three-step term intro stay (plain-words phenomenon first, then the jargon + original term); step three becomes: **land the implementation on a concrete location** — which file defines this concept, who creates it, who consumes it. Details must appear attached to code locations; no discussing them in mid-air.
2. **"Never explain a technical concept with another technical concept" → you may use what the reader already knows.** The reader is a junior programmer; the `for` loop they've written, the `git` they've used, the HTTP request they've sent, the database index they've built are the best anchors — use them first. But **the book still needs one homespun, everyday core metaphor** to carry the overall architecture (a construction site, a parcel sorting hub, a restaurant kitchen…); technical analogies are for local mechanisms only.
3. **"Shorter is better" → the book is longer overall, each page stays short.** Job training must cover "able to change code"; cutting content cuts away the reader's livelihood. So: 9–12 pages (1–3 sections each), and **control the load with layered reading, not by deleting** (see section 6). The per-page short rules (1–3 sections, paragraphs ≤ 4 lines, one closing line per page) are not relaxed by a single one.
4. **"No math formulas" → allowed when necessary.** If the project itself involves algorithms/formulas (scheduling, cost models, machine learning) they may appear, but every formula must be followed by one plain-words sentence plus its location in code (`file:symbol`). No pure theory derivations without a code anchor.
5. **"Don't belittle the difficulty to sound accessible" → pits and warnings are first-class citizens.** This book's reader touches this codebase tomorrow. "This part is twisty", "this config silently does nothing", "this area carries heavy legacy" is useful information, not negativity. Warn where the chapter must warn, in a `.warn` box (see `assets/code-kit.css`).

## 1. The spine principle: the whole book hangs on one main line

Before writing, fix **the spine**: the smallest, user-visible, meaningful operation, walked end to end from entry to result (the product of survey layer 5).

**Every chapter is a segment of this line**:

- prerequisites = the nouns needed to read this line;
- what-it-is = why this line deserves to exist;
- make-it-run = run this line yourself once;
- codebase map = which directories this line crosses;
- core abstractions = the handful of things passed along this line;
- the walk-through chapter = the line itself (the book's centre of gravity);
- hard bones = the two or three places on this line most likely to jam;
- changing code / first change = making one cut on this line.

If you can't define a spine, the survey isn't done — go back. If you end up with two or more spines, the book should be split in two — pick one and demote the other to an appendix or "where to go next".

Also fix the **base commit** and put it on the cover page ("this book is based on `<project>` at `abc1234`, 2026-10-17"). Every `file:line` is relative to it; that one sentence keeps readers from doubting the whole book when line numbers drift.

If the spine runs past 20 hops in the survey, it naturally splits in two ("before entering the engine / after entering the scheduler" and the like) — **split it into "the walk-through (part 1) / (part 2)"**, cutting at a cross-layer call (e.g. SQL layer → core layer): part 1 ends on a hook, part 2's lead-in picks it up. Don't squeeze it into one page, and don't cut hops to save pages.

## 2. Acceptance criteria: write "what you can do after reading" first

The cover / TOC page carries a **capability list**. It is the book's contract and the only criterion for selecting material — anything that doesn't serve a line on this list gets deleted.

Default list (tweak per project type):

- say in your own words what problem this project solves for whom, and where it stands in its ecosystem;
- build it on your own machine, run a minimal example, see the expected output;
- name roughly five core abstractions, their responsibilities, and how they relate;
- walk the spine: who does a call pass through, from entry to result;
- given a feature request or an error message, find the file to look at within ten minutes;
- get a single test to pass; add a breakpoint or a log line and see runtime state;
- walk one submission: branch → change → checks pass → PR opened.

## 3. Chapter blueprint

One page = one chapter (a heavy chapter may take two pages). **Every required chapter is mandatory**; optional ones are picked per project.

| # | Chapter | | Purpose | Must contain |
|---|---|---|---|---|
| 0 | TOC page | required | let the reader judge whether it's worth reading, and how to read it | book title (says plainly which project's onboarding book this is) + core-metaphor subtitle + base commit + capability list + "how to read this book" (layered-reading explanation) + one hook per chapter |
| 1 | Prerequisites crash course | required | backfill the minimum domain knowledge | 3–6 concepts, each: plain phenomenon → jargon → **which chapter it reappears in**; one small diagram of "how these concepts relate" |
| 2 | What it is, and why it exists | required | positioning and motivation | one real usage scenario (ideally the runnable snippet/command); what it replaced and where the pain was; an ecosystem-position diagram; how it differs from neighbours (briefly, neither praising nor trashing); **the core metaphor is set up here** |
| 3 | Make it run | required | build "I can make it move" confidence early | environment requirements (incl. versions); build command + **expected duration**; one minimal runnable example; expected output (trimmed); a troubleshooting order when it fails (3–5 items, most common first) |
| 4 | Codebase map | required | know where things are, and what can be ignored | directory tree (core vs ignorable marked); one-line responsibility per top-level module; module dependency diagram; "what's been most active in the last six months"; order of magnitude |
| 5 | Core abstractions: memorise these nouns | required | build the mental model | ≤7 abstractions, each: plain words → jargon → definition location → who creates / who consumes → lifetime; one relationship diagram (**step animation recommended**) |
| 6 | Walk it: what happens from A to B | required (centre of gravity) | turn static structure into dynamic understanding | the spine trace in 8–20 hops, each: `file:symbol` + one plain sentence + key snippet (≤15 lines, attention-marked); every break point (async / RPC / reflection / dynamic registration) explained on its own; one sequence diagram (**step animation strongly recommended**) |
| 7 | A few hard bones | optional (1–3 sections) | explain the mechanisms you'll hit when changing code | pick by one of three: most-frequently-changed area / pit every newcomer hits / performance-critical path. Each: why it's hard → the design trade-off → where the code is → what to watch when changing it |
| 8 | Changing code: from build to PR | required | close the dev loop | incremental build; **the exact command to run a single test**; run style/static checks in one command; debugging (log switches, breakpoints, remote debug); branch and commit-message conventions; what CI checks and where failures show up |
| 9 | Your first change | required | turn knowledge into one submission | 2–3 graded practice tasks (change a log message → add a test case → fix a small bug); how to find issues (labels, entry links); the full branch-to-PR steps; which checks to pass locally |
| ∞ | Wrap-up + where to go next | required | close and hand over | one formula closing the book; one sentence the reader carries away; in-repo advanced material (where the design/architecture docs live); community entry points (mailing list / forum / chat / issues) |
| app | Jargon glossary | strongly recommended | term lookup table | table: term / one plain sentence / original term / **what it's called in code (class name, config key, command)** / definition location. Every unfamiliar noun used in the book goes in |
| app | Errors & pitfalls lookup | optional | lifesaver | table: error keyword or symptom / likely cause / what to do / related code location |

**One strong ordering opinion: chapter 3 "make it run" must come before any architecture talk.** A newcomer builds trust in a project by making it move on their own machine, not by looking at architecture diagrams first. Once it runs, the map and the abstractions all have somewhere to land.

Every chapter's **four-piece set** (none optional):

1. page lead-in: one soft recap + one sentence posing this page's problem;
2. at least one diagram **or** one annotated code block (the walk-through chapter needs both);
3. one hands-on task (`.hands-on` box);
4. page-end closing line + one `.checkpoint` box ("after this chapter you can …", ≤3 items).

**Appendix lookup pages (jargon glossary, error table) are exempt from the hands-on task** — the page itself is a lookup table; bolting a task onto it is noise. Lead-in and closing line (or checkpoint) still apply.

**Hands-on task gradient**: early chapters (prerequisites, locating things, before it runs) should use **read-only commands** (`git ls-files`, `rg`, `wc -l`, opening a file and reading a few lines) so readers without a build environment can still do them; tasks needing a build/run start from chapter 3 onward and state the prerequisite up front ("finish chapter 3's build first"). Difficulty rises monotonically across the book: look → run → change one line and see the effect → add a test → open a PR.

## 4. Rules for quoting code

Code is a first-class citizen in this book, and the easiest thing to let run wild. Hard rules:

- **One plain sentence before, one pointer after**: before the code block, one sentence on "what this does"; after it, one sentence on "which line / which variable to watch". No unexplained bare code blocks.
- **≤15 lines.** Longer must be trimmed; replace the deleted part with a `// … omitted: this does XXX` placeholder so the reader knows what's missing and where to see it in full.
- **Never rename.** Keep original identifiers, original indentation, original comments — the reader will search the repo for these names in a minute.
- **Always give the source**: the code block header carries `path:start-line` (`.code-head`). First mention in the book uses the full path; later you may use a short name, but it must still identify the file uniquely.
- **Translate comments without touching the original**: if the source comment is jargon or in another register, add one plain sentence **below** the block; don't edit the source comment.
- **No syntax highlighting — attention marking instead**: `.hl` for the whole key line, `.mark` for an inline focus, `.dim` for the don't-bother-yet part. At most 2 marks per snippet — marking everything marks nothing.
- **Never ship a whole file or a whole config.** For config files quote only the few lines that actually change behaviour; the rest becomes an ellipsis.
- Before delivery, re-check every quoted location against the repo with `rg` (spot-check at least 5): path exists, line number roughly matches, and you didn't "prettify" the code.

## 5. Rules for commands and output

- **Input and output in two separate blocks**: input lines start with `$` (`.code-head` says `terminal`); output goes in `.code-block.out` (`.code-head` says `output (trimmed)`). Mixed together, the reader can't tell what to type.
- **Trim output to just enough to prove "it worked"**, marking omissions with `…`.
- **Mark expected duration on slow commands**: `# ~8 min; first run downloads dependencies`. Without it, the newcomer assumes a hang at minute three and goes searching for "XXX build hangs".
- **Warn before commands with side effects**: changing global config, installing things, deleting files, starting background services — use a `.warn` box saying what it touches and how to undo.
- **Run it yourself first**: commands in the book were run by you (mark ▶). Ones you can't run (build too expensive, needs external services, needs credentials) are copied from `CONTRIBUTING.md` or CI config and marked ⚠️ not run + source next to the command. **Never present an unrun command as if it had been run.**
- **⚠️ marks only "project-specific" commands.** `java -version`, `ls`, `git log` — generic tool commands need no mark. What needs marking is **this project's own** build/test/run commands (`./build/mvn …`, `dev/run-tests …`), because only those can vary per project and only those need a source. A page full of ⚠️ drowns the reader and the real warnings stop being read.
- Commands needing env vars put the variable on the same line (`FOO=bar cmd`) or on their own `export` line; never assume the reader's shell already has it.

## 6. Layered reading: two switches for the reader

Facing an unfamiliar project, a newcomer drowns most easily in "I want to understand everything". Solve it with layers, not with deletion:

- **Skippable marks**: a secondary section gets `<span class="skip-flag">skippable on a first pass</span>` after its heading; the TOC page explains "anything carrying this mark can be jumped on a first pass; come back when you need it". Marked sections must stay **under 1/3 of all sections** — more means the main line wasn't found properly.
- **Checkpoints**: a `.checkpoint` box at each chapter's end, ≤3 items, saying "after this chapter you can answer/do …". Can't answer = you didn't get it, go re-read that section; can answer = move on with confidence.

Two more load-reducers (inherited from `write4adhd-book`): **soft recaps** (one sentence at each chapter's start tying back to the previous one) and the **navigation trio** (the reader always knows where they are and how much is left).

## 7. Diagram requirements

Follow `write4adhd-book/references/html-craft.md`'s Canvas rules (`setupCanvas` / `setupAnim` / `drawBox` / `drawArrow` / `getColors`, plain-words captions, legible in both themes, animation auto-pausing off-viewport). This book needs at least these four:

1. **Ecosystem position** (chapter 2): this project in the middle, upstream/downstream/neighbours/dependencies around it — the reader sees "where it stands" at a glance.
2. **Module map** (chapter 4): core modules + dependency arrows, marking "you only need these for now".
3. **Core abstraction relationships** (chapter 5): who holds whom, who creates whom. **`setupAnim` step-by-step recommended**, adding one abstraction per step.
4. **Spine sequence diagram** (chapter 6): how one call propagates between components. **Step animation strongly recommended** — a call chain is essentially temporal; a static diagram flattens the ordering away.

In Markdown form there is no Canvas: fall back to ASCII diagrams (inside fenced code blocks) + one plain-words caption, or reference images the project already has (relative paths that render on GitHub).

## 8. Markdown form (when the deliverable goes into the project repo)

If the book will be committed as project documentation (`docs/onboarding/` or similar), use Markdown instead of HTML:

- one directory; `README.md` as the TOC page (capability list + layered-reading explanation + base commit + chapter links), one file per chapter (`01-prerequisites.md`, `06-trace.md`… numeric prefixes keep the order);
- fenced code blocks with language tags; **the source goes in the block's first comment line** (`// core/src/main/…/SparkContext.scala:412`);
- `.callout` → a `> **Note**: …` blockquote; `.hands-on` → a `> **🔧 Hands-on, 5 min**` blockquote + ordered list; `.warn` → `> ⚠️ **Pit**: …`; tables stay tables;
- all links relative and clickable on GitHub (source references via `../..` relative paths or full commit permalinks — never local absolute paths);
- voice, chapter structure, the four-piece set, hands-on tasks and the onboarding checklist **lose nothing** — switching to Markdown changes the carrier, not the standard.

## 9. Length budget

- ≤1200 words of prose per page (Chinese ≤1200 characters), **excluding code blocks**; over that, split the page, don't stuff.
- 9–12 pages + 1–2 appendices for the whole book. More means it should become a second book (e.g. "advanced: performance and tuning").
- Code blocks across the book stay under 1/3 of the prose volume. More code is not more useful — readers will go read the full source in the repo themselves; you quote only **the snippets that change their mental model**.

## 10. Onboarding checklist (run before delivery, on top of write4adhd-book's checklist)

- [ ] Cover page states the base commit and the survey date; every `file:line` is relative to it
- [ ] Spot-check ≥5 code citations: print the cited line's original text with `sed -n 'Np'` and compare against the book (path exists, line matches, snippet not prettified or renamed). Off by 1–2 lines still counts as wrong: fix the line number, or cite the symbol name only
- [ ] Every command was actually run, or explicitly marked ⚠️ not run + source
- [ ] Every ⚠️/ conclusion uses hedged wording in the prose, never disguised as a flat statement; unresolved spots give an entry point for checking
- [ ] The spine trace is continuous: no break between entry and result; every break point (async / RPC / reflection / dynamic registration) explains what reconnects it
- [ ] Every line of the capability list has a chapter supporting it; conversely, no chapter is off-contract filler
- [ ] Every chapter's four-piece set is complete: lead-in / diagram or annotated code / hands-on task / closing line + checkpoint (appendix lookup pages exempt from the hands-on task)
- [ ] Every hands-on task: file paths really exist, location descriptions are accurate, there is a success criterion ("you will see …"), and a "how to recover if it breaks"
- [ ] "Skippable on a first pass" marks ≤ 1/3 of all sections
- [ ] The jargon glossary contains every unfamiliar noun used in the book, each with a "what it's called in code"
- [ ] No code block exceeds 15 lines; no whole-file / whole-config shipping; every block has prose before and after
- [ ] Command input and output are presented separately; slow commands carry expected durations; side-effecting commands carry a warning and an undo
- [ ] At least four diagrams present (ecosystem position / module map / abstraction relationships / spine sequence), captions in plain words
- [ ] HTML form: `code-kit.css` shipped with the deliverable (book site has the `<link>`, single page has it inlined); code blocks, tables and hands-on boxes legible in light and dark themes; at ~375px code blocks scroll horizontally instead of breaking the layout
- [ ] **Final acceptance**: have someone who has never touched this project (or another agent playing "just learned to program, knows nothing about this domain") read it from the top and actually do the hands-on tasks of chapters 3, 8 and 9. Where they get stuck is what the book must fix
