# Onboarding book blueprint

This document says **what to write**: the book's skeleton, what every chapter must contain, how to present code and commands, and the pre-delivery onboarding checklist.

**How to write** (voice, ADHD-friendly rules, HTML/book-site craft) is not here — that is `write4adhd-book`'s job. Before writing, read its SKILL.md and its `references/style-guide.md` and `references/html-craft.md` in full; this document's rules stack on top of them.

**Correct one easy mistake first**: the demystified style controls the **reading barrier**, not the **amount of knowledge**. The reader is going to become the owner of part of this project, so "accessible style" never means "shallower content" — the subsystems that must be covered, the structures that must be explained, the responsibility surfaces that must be given in full are all debts you must pay; you pay them by **adding chapters and grouping them into parts**, never by deleting knowledge to make the book shorter. Inside a single chapter, stay short and conversational; the whole book may be long.

## 0. The five overrides of write4adhd-book

Only these five differ. Everything else (paragraphs ≤ 4 lines, a closing line per chapter, the three-step term intro, soft recaps, plain-words captions, TOC hooks, the navigation trio, Canvas diagram rules, animation never holding attention hostage…) is **inherited unchanged**.

1. **"Stop right there, no implementation details" → you do explain implementation details.** The first two steps of the three-step term intro stay (plain-words phenomenon first, then the jargon + original term); step three becomes: **land the implementation on a concrete location** — which file defines this concept, who creates it, who consumes it, what its public surface looks like. Details must appear attached to code locations; no discussing them in mid-air.
2. **"Never explain a technical concept with another technical concept" → you may use what the reader already knows.** The reader is a junior programmer; the `for` loop they've written, the `git` they've used, the HTTP request they've sent, the database index they've built are the best anchors — use them first. But **the book still needs one homespun, everyday core metaphor** to carry the overall architecture (a construction site, a parcel sorting hub, a restaurant kitchen…); technical analogies are for local mechanisms only.
3. **"Shorter is better" → each page stays short, the book may be long.** Job training must cover "able to own a part of this"; cutting content cuts away the reader's livelihood. So: **the per-page short rules are not relaxed by a single one** (1–3 sections, paragraphs ≤ 4 lines, one closing line per page); the book's total length is carried by **adding chapters** and **grouping them into parts** (section 2), and layered reading controls the load of one sitting — not the total load.
4. **"No math formulas" → allowed when necessary.** If the project itself involves algorithms/formulas (scheduling, cost models, machine learning) they may appear, but every formula must be followed by one plain-words sentence plus its location in code (`file:symbol`). No pure theory derivations without a code anchor.
5. **"Don't belittle the difficulty to sound accessible" → pits and warnings are first-class citizens.** This book's reader touches this codebase tomorrow. "This part is twisty", "this config silently does nothing", "this area carries heavy legacy" is useful information, not negativity. Warn where the chapter must warn, in a `.warn` box (see `assets/code-kit.css`).

## 1. Two axes: the spine gives depth, the inventory gives breadth

### 1.1 The spine (depth axis)

Before writing, fix **the spine**: the smallest, user-visible, meaningful operation, walked end to end from entry to result (the product of survey layer 5).

**The walk-through chapters are segments of this line**: prerequisites = the nouns needed to read this line; what-it-is = why this line deserves to exist; make-it-run = run this line yourself once; the walk-through chapter = the line itself.

If you can't define a spine, the survey isn't done — go back. If you end up with two or more spines, the book should be split in two — pick one and demote the other to a highlighted section inside the tour chapter.

If the spine runs past 20 hops in the survey, it naturally splits in two ("before entering the engine / after entering the scheduler" and the like) — **split it into "the walk-through (part 1) / (part 2)"**, cutting at a cross-layer call (e.g. SQL layer → core layer): part 1 ends on a hook, part 2's lead-in picks it up. Don't squeeze it into one page, and don't cut hops to save pages.

Also fix the **base commit** and put it on the cover page ("this book is based on `<project>` at `abc1234`, 2026-10-17"). Every `file:line` is relative to it; that one sentence keeps readers from doubting the whole book when line numbers drift.

### 1.2 The subsystem inventory (breadth axis) ★

A book with only a spine has **tunnel vision**: the reader walks one line to the end and looks up still not knowing what rooms this project has. And the reader's next task may well land off the spine (the UI, monitoring, some connector, history replay…) — by then this book must already have given them a first layer of context.

So the survey must produce a **subsystem inventory** (product of survey layers 2 and 4) covering **all** major subsystems of the project, each with:

| Field | Meaning |
|---|---|
| Name | subsystem / module name |
| One-line responsibility | whose problem it solves |
| Entry | where users or code come in (command / API / class / config switch) |
| Core classes | ≤5, with definition locations |
| Interface to the spine | at which hop it connects; for standalone ones write "standalone; interface is X" |
| Recent change heat | from the history layer's stats |
| Space in this book | full chapter / one tour section / one line in the map + one glossary row |

**Coverage promise (hard rule)**: the reader may be assigned work on any item of this inventory, so **every item must have at least a "first layer of context"** — entry, core classes, relation to the main line, first stop when changing it. Allocation rules:

- on the spine, or the area the user named as theirs → **full chapter**;
- off the spine but real and maintained → **one section in the tour chapter** (see "subsystem tour" in section 4);
- legacy / frozen / barely touched → one sentence in the codebase map + one glossary row, **but that sentence must contain the entry and the core class names** — never just "legacy, skipped".

"Exists, not expanding" is allowed only as a **space allocation on the inventory**, never as an excuse for zero information.

### 1.3 Module internals and core-class responsibility surfaces (structure axis)

With only a directory tree and scattered snippets, the reader still cannot think top-down. Two mandatory reinforcements:

- **Module internal layering**: what each core module is divided into internally (directory → layer). E.g. some SQL module = parser / analysis / optimizer / physical plans / operator implementations; some core module = scheduling / shuffle / memory / storage / execution / UI. The goal: the reader can **guess which layer a piece of code lives in from its directory**.
- **Core-class responsibility surfaces**: for each core abstraction (≤7), don't stop at a one-line explanation — give a **responsibility-surface table**: its public methods grouped by function (e.g. "submit jobs / create data / config & state / lifecycle / broadcasts & accumulators"), who it holds, who holds it, **which kinds of requests land on it**, key internal subclasses or inner classes. The reader must be able to answer from this table: "the feature I'm adding will probably sit next to which group of methods on which class".

## 2. Parts: cutting a long book into learning stages

Past ~10 chapters you must group into **parts**. A part = a learning stage for the reader, a natural stopping point (readable across several days), and a memorable skeleton for the whole book.

Default five parts + appendices (trim per project; merge or add as needed, but keep the order "meet it → foundations → walk-through → territory → doing the work"):

| Part | Contents | After this part the reader can |
|---|---|---|
| Ⅰ Meet it | what it is / **global shape** / make it run | say whom the project serves and what it actually looks like on a machine; run it once themselves |
| Ⅱ Foundations | prerequisites crash course / codebase map (incl. module layering) / **core abstractions & responsibility map** | read any chapter's vocabulary; guess where code lives from directories; say what each core class can do |
| Ⅲ Walk-through | the spine (part 1/2) / hard bones | trace one call from entry to result along the main line, and point at the breaks |
| Ⅳ Territory | **subsystem tour** (incl. subsystems off the spine that the reader may own) | hold a first layer of context for every subsystem off the spine, and know the first stop when assigned one |
| Ⅴ Doing the work | changing code: build to PR / your first change / wrap-up & handover | walk one submission; know what to read next and whom to ask |
| Appendices | jargon glossary / errors & pitfalls lookup | look things up |

Presentation (no new components needed; use the existing design language):

- **TOC page grouped by part**: each part gets an `<h2><span class="num">Ⅱ</span> part title</h2>` + one line "after this part you can …", with that part's `.toc-item` cards under it;
- **chapter badge carries the part**: `<span class="badge">Part Ⅱ · Chapter 5</span>`;
- no standalone pages between parts (the TOC page carries the grouping), but **the first chapter of each part** opens with a soft recap of the previous part ("last part you got the map; this part we start walking into it").

## 3. Acceptance criteria: write "what you can do after reading" first

The cover / TOC page carries a **capability list**. It is the book's contract and the only criterion for selecting material — anything that doesn't serve a line on this list gets deleted; any line on the list that can't be ticked gets a chapter added.

Default list (tweak per project type):

- say in your own words what problem this project solves for whom, and where it stands in its ecosystem;
- **draw the project's global shape**: which processes/components exist, who talks to whom, where control flow and data flow each go, the path of one job across the topology;
- build it on your own machine, run a minimal example, see the expected output;
- state each core abstraction's **responsibility surface** (what it can do, who it holds, who holds it, which requests land on it) and how they relate;
- **state each core module's internal layering from its directory**, and guess which layer a given class lives in;
- walk the spine: who a call passes through from entry to result, and point at the break points;
- **give a first layer of context for every subsystem off the spine**: entry, core classes, interface to the main line, first stop when changing it;
- given a feature request or an error message, find the file to look at within ten minutes;
- get a single test to pass; add a breakpoint or a log line and see runtime state;
- walk one submission: branch → change → checks pass → PR opened.

## 4. Chapter blueprint

One page = one chapter (a heavy chapter may take two pages). **Every required chapter is mandatory**; optional ones are picked per project. Which part each chapter belongs to: see section 2.

| # | Chapter | | Purpose | Must contain |
|---|---|---|---|---|
| 0 | TOC page | required | let the reader judge whether it's worth reading, and how | book title (says plainly which project's onboarding book this is) + core-metaphor subtitle + base commit + capability list + "how to read this book" (layered reading + part structure) + TOC **grouped by part**, one hook per chapter |
| 1 | What it is, and why it exists | required | positioning and motivation | one real usage scenario (ideally the runnable snippet/command); what it replaced and where the pain was; an ecosystem-position diagram; how it differs from neighbours (briefly, neither praising nor trashing); **the core metaphor is set up here** |
| 2 | **Global shape: what this project looks like** | required | the system view from 10 km | process/component topology diagram (who talks to whom; control flow vs data flow as two kinds of line); a "what runs in which process" table; the path one job takes across the topology (component level only, no code yet); where the three cross-cutting concerns — config, logs, monitoring — each live; local minimal form vs cluster form |
| 3 | Make it run | required | build "I can make it move" confidence early | environment requirements (incl. versions); build command + **expected duration**; one minimal runnable example; expected output (trimmed); a troubleshooting order when it fails (3–5 items, most common first) |
| 4 | Prerequisites crash course | required | backfill the minimum domain knowledge | 3–6 concepts, each: plain phenomenon → jargon → **which chapter it reappears in**; one small diagram of "how these concepts relate" |
| 5 | Codebase map | required | know where things are, and what can be ignored | directory tree (core vs ignorable marked); one-line responsibility per top-level module; **each core module's internal layering** (directory → layer); module dependency diagram; "what's been most active recently"; order of magnitude |
| 6 | **Core abstractions & responsibility map** | required | a top-down mental model | ≤7 abstractions, each: plain words → jargon → definition location → **responsibility-surface table** (method groups / holds whom / held by whom / which requests land on it / key internal subclasses) → lifetime; one ownership diagram (**step animation recommended**) |
| 7 | Walk it (part 1/2) | required (centre of gravity) | turn static structure into dynamic understanding | the spine trace in 8–20 hops per page, each: `file:symbol` + one plain sentence + key snippet (≤15 lines, attention-marked); every break point explained on its own; one sequence diagram (**step animation strongly recommended**) |
| 8 | A few hard bones | optional (1–3 sections) | explain the mechanisms you'll hit when changing code | pick by one of three: most-frequently-changed area / pit every newcomer hits / performance-critical path. Each: why it's hard → the design trade-off → where the code is → what to watch when changing it |
| 9 | **Subsystem tour** | required (1–3 pages) | first-layer context for the world off the spine | one section per inventory item that didn't get a chapter: what it solves / entry and core classes (with locations) / interface to the spine / first stop when changing it; **a subsystem the user named as theirs but that isn't on the spine must get a full chapter or at least two sections here**; frozen/legacy subsystems each get one sentence (with entry and core class names) |
| 10 | Changing code: from build to PR | required | close the dev loop | incremental build; **the exact command to run a single test**; run style/static checks in one command; debugging (log switches, breakpoints, remote debug); branch and commit-message conventions; what CI checks and where failures show up |
| 11 | Your first change | required | turn knowledge into one submission | 2–3 graded practice tasks (change a log message → add a test case → fix a small bug); how to find issues (labels, entry links); the full branch-to-PR steps; which checks to pass locally |
| ∞ | Wrap-up + where to go next | required | close and hand over | one formula closing the book; one sentence the reader carries away; in-repo advanced material (where the design/architecture docs live); community entry points (mailing list / forum / chat / issues) |
| app | Jargon glossary | strongly recommended | term lookup table | table: term / one plain sentence / original term / **what it's called in code (class name, config key, command)** / definition location. Every unfamiliar noun used in the book goes in |
| app | Errors & pitfalls lookup | optional | lifesaver | table: error keyword or symptom / likely cause / what to do / related code location |

**One strong ordering opinion: chapter 3 "make it run" must come before any architecture talk.** A newcomer builds trust in a project by making it move on their own machine, not by looking at architecture diagrams first. Once it runs, the shape, the map and the responsibility surfaces all have somewhere to land.

Every chapter's **four-piece set** (none optional):

1. page lead-in: one soft recap + one sentence posing this page's problem;
2. at least one diagram **or** one annotated code block (the walk-through chapter needs both);
3. one hands-on task (`.hands-on` box);
4. page-end closing line + one `.checkpoint` box ("after this chapter you can …", ≤3 items).

**Appendix lookup pages (jargon glossary, error table) are exempt from the hands-on task** — the page itself is a lookup table; bolting a task onto it is noise. Lead-in and closing line (or checkpoint) still apply.

**Hands-on task gradient**: early chapters (prerequisites, locating things, before it runs) should use **read-only commands** (`git ls-files`, `rg`, `wc -l`, opening a file and reading a few lines) so readers without a build environment can still do them; tasks needing a build/run start from chapter 3 onward and state the prerequisite up front ("finish chapter 3's build first"). Difficulty rises monotonically across the book: look → run → change one line and see the effect → add a test → open a PR. Tour-chapter tasks are always read-only: "open this subsystem's entry class and write down its core class list" and the like.

## 5. Rules for quoting code

Code is a first-class citizen in this book, and the easiest thing to let run wild. Hard rules:

- **One plain sentence before, one pointer after**: before the code block, one sentence on "what this does"; after it, one sentence on "which line / which variable to watch". No unexplained bare code blocks.
- **≤15 lines.** Longer must be trimmed; replace the deleted part with a `// … omitted: this does XXX` placeholder so the reader knows what's missing and where to see it in full.
- **Never rename.** Keep original identifiers, original indentation, original comments — the reader will search the repo for these names in a minute.
- **Always give the source**: the code block header carries `path:start-line` (`.code-head`). First mention in the book uses the full path; later you may use a short name, but it must still identify the file uniquely.
- **Translate comments without touching the original**: if the source comment is jargon or in another register, add one plain sentence **below** the block; don't edit the source comment.
- **No syntax highlighting — attention marking instead**: `.hl` for the whole key line, `.mark` for an inline focus, `.dim` for the don't-bother-yet part. At most 2 marks per snippet — marking everything marks nothing.
- **Never ship a whole file or a whole config.** For config files quote only the few lines that actually change behaviour; the rest becomes an ellipsis.
- **For structural content, prefer tables and diagrams over piles of code**: responsibility surfaces, module layering and ownership relationships build top-down understanding far better as one table or one diagram than as ten snippets. Code snippets belong only where "this one line changes the reader's mental model".
- Before delivery, re-check every quoted location against the repo with `rg` (spot-check at least 5): path exists, line number roughly matches, and you didn't "prettify" the code.

## 6. Rules for commands and output

- **Input and output in two separate blocks**: input lines start with `$` (`.code-head` says `terminal`); output goes in `.code-block.out` (`.code-head` says `output (trimmed)`). Mixed together, the reader can't tell what to type.
- **Trim output to just enough to prove "it worked"**, marking omissions with `…`.
- **Mark expected duration on slow commands**: `# ~8 min; first run downloads dependencies`. Without it, the newcomer assumes a hang at minute three and goes searching for "XXX build hangs".
- **Warn before commands with side effects**: changing global config, installing things, deleting files, starting background services — use a `.warn` box saying what it touches and how to undo.
- **Run it yourself first**: commands in the book were run by you (mark ▶). Ones you can't run (build too expensive, needs external services, needs credentials) are copied from `CONTRIBUTING.md` or CI config and marked ⚠️ not run + source next to the command. **Never present an unrun command as if it had been run.**
- **⚠️ marks only "project-specific" commands.** `java -version`, `ls`, `git log` — generic tool commands need no mark. What needs marking is **this project's own** build/test/run commands (`./build/mvn …`, `dev/run-tests …`), because only those can vary per project and only those need a source. A page full of ⚠️ drowns the reader and the real warnings stop being read.
- Commands needing env vars put the variable on the same line (`FOO=bar cmd`) or on their own `export` line; never assume the reader's shell already has it.

## 7. Layered reading: two switches for the reader

Facing an unfamiliar project, a newcomer drowns most easily in "I want to understand everything". Solve it with layers, not with deletion:

- **Skippable marks**: a secondary section gets `<span class="skip-flag">skippable on a first pass</span>` after its heading; the TOC page explains "anything carrying this mark can be jumped on a first pass; come back when you need it". Marked sections must stay **under 1/3 of all sections** — more means the main line wasn't found properly. Note: **tour-chapter sections are never marked skippable** — they exist precisely for "the reader's next task is there"; marking them skippable makes them pointless.
- **Checkpoints**: a `.checkpoint` box at each chapter's end, ≤3 items, saying "after this chapter you can answer/do …". Can't answer = you didn't get it, go re-read that section; can answer = move on with confidence.

Two more load-reducers (inherited from `write4adhd-book`): **soft recaps** (one sentence at each chapter's start tying back to the previous one) and the **navigation trio** (the reader always knows where they are and how much is left).

## 8. Diagram requirements

Follow `write4adhd-book/references/html-craft.md`'s Canvas rules (`setupCanvas` / `setupAnim` / `drawBox` / `drawArrow` / `getColors`, plain-words captions, legible in both themes, animation auto-pausing off-viewport). This book needs at least these six:

1. **Ecosystem position** (chapter 1): this project in the middle, upstream/downstream/neighbours/dependencies around it — the reader sees "where it stands" at a glance.
2. **Global shape / process topology** (chapter 2): components as nodes, **control flow and data flow as two kinds of line**; paired with the "what runs in which process" table. This diagram is the book's base map — later chapters keep pointing back to it.
3. **Module map + internal layering** (chapter 5): core modules + dependency arrows + a layering strip inside each module; mark "you only need these for now".
4. **Core abstraction ownership diagram** (chapter 6): who holds whom, who creates whom. **`setupAnim` step-by-step recommended**, adding one abstraction per step.
5. **Spine sequence diagram** (chapter 7): how one call propagates between components. **Step animation strongly recommended** — a call chain is essentially temporal; a static diagram flattens the ordering away.
6. **Subsystem panorama** (chapter 9): place every inventory item on one diagram, marking which ones the spine crosses and where the interfaces are — so the reader sees at a glance "which rooms I haven't entered yet".

In Markdown form there is no Canvas: fall back to ASCII diagrams (inside fenced code blocks) + one plain-words caption, or reference images the project already has (relative paths that render on GitHub).

## 9. Markdown form (when the deliverable goes into the project repo)

If the book will be committed as project documentation (`docs/onboarding/` or similar), use Markdown instead of HTML:

- one directory; `README.md` as the TOC page (capability list + layered-reading explanation + part structure + base commit + chapter links), one file per chapter (`01-what-it-is.md`, `07-trace-1.md`… numeric prefixes keep the order; parts may be expressed by filename prefix or by subdirectories);
- fenced code blocks with language tags; **the source goes in the block's first comment line** (`// core/src/main/…/SparkContext.scala:412`);
- `.callout` → a `> **Note**: …` blockquote; `.hands-on` → a `> **🔧 Hands-on, 5 min**` blockquote + ordered list; `.warn` → `> ⚠️ **Pit**: …`; tables stay tables;
- all links relative and clickable on GitHub (source references via `../..` relative paths or full commit permalinks — never local absolute paths);
- voice, chapter structure, the four-piece set, hands-on tasks and the onboarding checklist **lose nothing** — switching to Markdown changes the carrier, not the standard.

## 10. Length budget

- ≤800 words of prose per page (Chinese ≤1200 characters), **excluding code blocks and tables**; over that, split the page, don't stuff.
- The whole book: **12–20 pages + 1–2 appendices**, grouped into **4–6 parts**. Under 12 pages usually means a breadth debt (check against the subsystem inventory); over 20 means it should become a second book (e.g. "advanced: performance and tuning").
- Code blocks across the book stay under 1/3 of the prose volume. More code is not more useful — structural information belongs in tables and diagrams (section 5); code is quoted only for **the snippets that change the reader's mental model**.

## 11. Onboarding checklist (run before delivery, on top of write4adhd-book's checklist)

- [ ] Cover page states the base commit and the survey date; every `file:line` is relative to it
- [ ] **Subsystem inventory coverage is 100%**: every item has a landing spot in the book (full chapter / tour section / one map line + one glossary row); nothing is zero-information; the area the user named got a full chapter or at least two sections
- [ ] **The global-shape chapter exists**: process topology diagram (control flow / data flow as two line kinds) + "what runs in which process" table + where the cross-cutting concerns (config / logs / monitoring) live
- [ ] **Every core abstraction has a responsibility-surface table** (method groups / holds whom / held by whom / which requests land on it / key internal subclasses), and every core module has internal layering
- [ ] TOC page grouped by part; chapter badges carry the part number; part count between 4 and 6
- [ ] Spot-check ≥5 code citations: print the cited line's original text with `sed -n 'Np'` and compare against the book (path exists, line matches, snippet not prettified or renamed). Off by 1–2 lines still counts as wrong: fix the line number, or cite the symbol name only
- [ ] Every command was actually run, or explicitly marked ⚠️ not run + source
- [ ] Every ⚠️/ conclusion uses hedged wording in the prose, never disguised as a flat statement; unresolved spots give an entry point for checking
- [ ] The spine trace is continuous: no break between entry and result; every break point (async / RPC / reflection / dynamic registration) explains what reconnects it
- [ ] Every line of the capability list has a chapter supporting it; conversely, no chapter is off-contract filler
- [ ] Every chapter's four-piece set is complete: lead-in / diagram or annotated code / hands-on task / closing line + checkpoint (appendix lookup pages exempt from the hands-on task)
- [ ] Every hands-on task: file paths really exist, location descriptions are accurate, there is a success criterion ("you will see …"), and a "how to recover if it breaks"
- [ ] "Skippable on a first pass" marks ≤ 1/3 of all sections, and not a single tour-chapter section carries one
- [ ] The jargon glossary contains every unfamiliar noun used in the book, each with a "what it's called in code"
- [ ] No code block exceeds 15 lines; no whole-file / whole-config shipping; every block has prose before and after; structural information uses tables/diagrams rather than piles of code
- [ ] Command input and output are presented separately; slow commands carry expected durations; side-effecting commands carry a warning and an undo
- [ ] All six required diagrams present (ecosystem position / process topology / modules+layering / ownership / spine sequence / subsystem panorama), captions in plain words
- [ ] HTML form: `code-kit.css` shipped with the deliverable (book site has the `<link>`, single page has it inlined); code blocks, tables and hands-on boxes legible in light and dark themes; at ~375px code blocks scroll horizontally instead of breaking the layout
- [ ] **Final acceptance**: have someone who has never touched this project (or another agent playing "just learned to program, knows nothing about this domain") read it from the top and actually do the hands-on tasks of chapters 3, 10 and 11; then ask them three questions — "what shape is this project on a machine?", "what can <some core class> do, and who is it related to?", "if I assign you <some subsystem off the spine>, where's your first stop?" Any unanswered question is a debt the book owes
