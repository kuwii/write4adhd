# Codebase survey protocol

Before writing the onboarding book, get your bearings in the project. This document is **how to survey**: in what order to look, with which commands, what to note when you see what, and when to stop.

Write every finding into the notes file (`assets/survey-notes-template.md`), kept in a scratch area, never in the deliverable directory. **The notes are the only source material for writing**: while writing, consult the notes plus a few targeted re-checks — do not resume wandering the codebase as you write (it burns tokens and lets the book's structure drift along with your wandering path).

## 0. Three disciplines (memorise first; breaking any one of them ends badly)

1. **Sample, don't read through.** A large repo cannot be read cover to cover and doesn't need to be. Always locate with `rg` first, then read only 30–120 lines around the hit; never `cat` a whole file. A single file gets at most 2–3 windows in one survey.
2. **Sampling controls depth per spot, never breadth**, and write as you go. The survey has two axes (see `book-blueprint.md` section 1): the spine gives depth, the subsystem inventory gives breadth. The sampling discipline governs **how deep you read one file** (locate with `rg` first, then take windows; ≤3 windows per file) — **not** "anything off the spine may be skipped". Every subsystem on the inventory gets at least one targeted look (entry + core-class list + first 30 lines of the class head); a zero-information "exists, not expanding" is a debt, not discipline.
   - **Notes first**: at the very start of the survey, copy the template into a notes file, and **write each layer's findings into it the moment that layer is done** (write/edit, on disk). Do not hoard everything and backfill from memory at the end. If you can't write the "one-line responsibility", you haven't understood that layer yet — the act of writing to disk is what forces you to converge, and an interruption mid-survey won't leave you with nothing.
   - **Budget in search steps**: count both `read` calls and "searching / windowing `bash`" calls (`rg`, `sed -n`, `git log`, `wc`, …) — one step each, ≤ 90 in total (≤ 140 for a very large repo). Per-layer quota: layers 1–4 ≤ 30, layer 5 ≤ 40, layer 6 ≤ 20. Past that you are digging one spot too deep — go back to the inventory: **finishing the breadth beats drilling one well through**.
3. **Separate "read" from "guessed".** Tag every finding with a confidence mark, in the notes, right away — don't wait until writing time to remember:

   | Mark | Meaning |
   |---|---|
   | ✅ | Conclusion drawn from reading source / config |
   | ▶ | Actually ran the command and saw the output |
   | ⚠️ | Inferred from naming, docs or common sense, **implementation not read** |
   | ❓ | Didn't get it; must confess this to the reader in the book |

   When writing: a ⚠️ either gets upgraded by verifying it, or appears in the prose as "it looks like … (I didn't read the implementation)"; a ❓ gets written out plainly with an entry point for checking. **Under no circumstances may a ⚠️/ be written as a flat declarative sentence.**

Out of scope: `.git/`, build outputs and dependency directories (`target/ build/ dist/ node_modules/ vendor/ .venv/ __pycache__/`), generated code (`*_pb2.py`, `generated/`, `target/generated-sources/`). Traverse with `git ls-files` rather than `find` — that excludes most of it for free.

## 1. Meta layer: how the project introduces itself

Read what the project wrote for humans first; best value per minute.

```bash
ls -a                                   # top-level files/dirs: the shape of the project at a glance
cat README.md                           # or README.rst / README.zh.md
ls docs/ && ls docs/*.md | head -30     # official docs; often holds design / architecture pages
cat CONTRIBUTING.md                     # ★ what a newcomer should read first: build, test, PR flow
ls .github/ .gitlab-ci.yml 2>/dev/null  # CI config: the rules the machine enforces for you
```

Note down: the project's one-line positioning, official docs entry points, the build/test commands from the contributing guide, which checks CI runs, issue templates and `good first issue`-style labels, licence, release cadence (`git tag --sort=-creatordate | head -10`).

⚠️ Docs go stale. Every command you plan to put in the book gets run once in layer 6; where docs and code disagree, code wins — and point the conflict out in the book (it's valuable to a newcomer: "don't trust that line in the README; actually …").

## 2. Shape layer: how big, which languages, how it's chunked

```bash
# file-type distribution (real source composition, no build output)
git ls-files | sed 's/.*\.//' | sort | uniq -c | sort -rn | head -20

# files per top-level dir: where the weight sits, at a glance
for d in $(git ls-files | awk -F/ 'NF>1{print $1}' | sort -u); do
  printf '%8d  %s\n' "$(git ls-files "$d" | wc -l)" "$d"
done | sort -rn

# second level: slice the heavy dirs once more to locate the real core modules
git ls-files | awk -F/ 'NF>2{print $1"/"$2}' | sort | uniq -c | sort -rn | head -25
```

Then read the build config to learn the module split and dependency direction (who depends on whom = the first layer of the architecture): `pom.xml` / `build.sbt` / `settings.gradle*` / `package.json` (workspaces) / `pyproject.toml` / `go.work` / `Cargo.toml` (workspace members). In a multi-module project, **the module dependency graph is the first answer to "what is the architecture"** and deserves a diagram in the book.

Note down: primary and secondary languages, order of magnitude of the code (thousands / hundreds of thousands / millions of lines — this decides how much the book can cover), the module list with one-line responsibilities, which dirs are core and which can be ignored for now (test data, examples, scripts, doc site, legacy).

**This layer also produces two tables** (the breadth axis's foundation; fields in `book-blueprint.md` section 1.2):

1. **Subsystem inventory**: condense modules + top-level dirs + entries into "subsystem" granularity (one subsystem = one piece a reader might be assigned to own, e.g. "scheduling", "shuffle", "web UI", "history replay", "Python API"). Per item note: one-line responsibility / entry / ≤5 core classes / interface to the spine / recent change heat / space allocated in this book. The inventory must be **exhaustive** over major subsystems — miss one and the reader's first assignment may be exactly there.
2. **Module internal layering**: what each core module is divided into internally (directory → layer), e.g. `sql/catalyst` = parser / analysis / optimizer / plan trees; `core` = scheduling / shuffle / memory / storage / execution / UI. Directory and package names are enough to derive this; no need to read implementations.

## 3. History layer: who changes what, where

```bash
git log --oneline -60                        # what recent commits talk about
git log -1 --format='%H %ci'                 # ★ record the base commit; the book must state it
# most-touched dirs in the last six months = the project's current battlefield
git log --since='6 months ago' --name-only --pretty=format: \
  | grep -v '^$' | awk -F/ 'NF>1{print $1"/"$2} NF==1{print $1}' \
  | sort | uniq -c | sort -rn | head -20
git shortlog -sn --since='1 year ago' | head -15   # core team / main contributors
```

Note down: the base commit, recently active areas (a newcomer changing these gets reviewed fastest — and collides fastest), commit-message conventions (`[module] verb first` / `feat:` / `Fixes #123`), release cadence, branch and release model.

The history layer feeds two chapters directly: `the codebase map` (which dirs are alive) and `changing code: from build to PR` (how to write a commit message).

⚠️ **Be honest about the statistical window**: if the repo is a shallow/partial clone (`git rev-list --count HEAD` is small, or it was cloned with `--filter`), then "most active dirs in the last six months" is not a six-month window. **State the real window in the book** (e.g. "this book is based on a 100-commit shallow clone, so the activity stats cover the last 6 days") — don't let readers mistake it for a long-term trend.

## 4. Entry layer: where the program starts moving

```bash
# executable entries
rg -n --no-heading 'static void main|def main\(|if __name__ ==|func main\(|@SpringBootApplication' \
   -g '!*test*' -g '!*Test*' | head -40
ls bin/ sbin/ cmd/ 2>/dev/null          # script entries; often the commands users actually type
cat <the packaging/release config in the build file>   # which module becomes the artifact
```

Find all three kinds of entry:
- **user entry**: the command users type / the API they call / the job they submit (chapters 2 and 3 need it);
- **process entry**: the `main` function, the daemon startup script (the starting point of the trace);
- **extension entry**: plugin/SPI/registry/config-driven dynamic loading (the place the trace is most likely to break — layer 5 handles it explicitly).

For a library-type project, the "entry" is the top-level types of the public API:

```bash
# types exposed by the top-level package = the nouns this project wants you to know
ls <core module>/src/main/<lang path>/     # the directory names are themselves a concept map
rg -l '^package ' <core module>/src/main | head -20
```

Repeat the entry identification for **every subsystem on the inventory** (the breadth axis's targeted look): its user/process/extension entries, its core-class list (`rg` for class definitions + reading the first 30 lines of the class head is enough), and at which hop it connects to the spine (write "standalone; interface is X" when it doesn't). Keep each subsystem to 2–3 searches — this is the floor cost of the coverage promise; don't skip it.

## 5. Lifeline layer ★: walk it end to end (the most important layer)

This layer decides whether the book is useful or not. Goal: **pick the smallest, user-visible, meaningful operation and follow it from entry to result, hop by hop**, recording every hop.

Criteria for the main line: small enough (one call tells the whole story), typical enough (it runs through core code paths, not a special case), visible enough (the reader can run it themselves and see the result). Examples: for a batch engine, "read a file, aggregate, print the result"; for a web service, "one `GET` request from route to response"; for a CLI tool, "the most-used subcommand"; for a library, "the snippet from the official quickstart".

How to walk it:

1. Start at the entry; `rg -n '<symbol>'` to find the definition; read 30–120 lines at the definition site;
2. Record the hop: `file:line` + one plain sentence (what this step does) + whether to quote the code in the book;
3. Find the next key symbol it calls; repeat. **8–20 hops is enough** — don't dig all the way into the standard library;
4. When you hit a **break** (async callback, message queue, RPC, reflection, dynamic registration, macro/codegen), do not skip it — record it as a knowledge point in its own right: where it breaks, and what reconnects it (a registry? a config file? SPI? an event bus?). Ninety percent of a newcomer's getting-lost happens at breaks; **this is the most valuable content in the whole book**;
5. When static reading stalls, **verify by running**: add one log/print line, re-run the minimal operation, see which path it actually took. More reliable than reading the code ten times, and it doubles as material for chapter 3 ("make it run") and chapter 8 ("debugging").

Note down: the complete hop table (index / location / plain-words explanation / confidence), every break point and how it reconnects, every unfamiliar noun that appears on this line (they go into the jargon glossary), and which few code snippets are most worth quoting in the book (pick 4–8, each trimmed to ≤15 lines).

Also identify the **core abstractions** (≤7; more means you haven't found them yet):

```bash
# most-extended/implemented types = the skeleton nouns of this project
rg -o '(extends|implements)\s+[A-Z]\w+' --no-filename -g '!*test*' \
  | awk '{print $2}' | sort | uniq -c | sort -rn | head -25
# most frequent type names inside one module
rg -o '\b[A-Z][A-Za-z]{3,}\b' --no-filename <core module>/src/main \
  | sort | uniq -c | sort -rn | head -40
```

For each core abstraction note: a plain-words explanation, which file defines it, who creates it, who consumes it, how long it lives (one call / one job / one process), and its **responsibility surface**: public methods grouped by function (listing signatures is enough — don't read method bodies), who it holds, who holds it, key internal subclasses/inner classes, and "which kinds of requests land on it". Pull the signatures in one shot with something like `rg -n '^\s*(def|public|private\[spark\] def) ' <file> | head -40` and then group them — the cheapest step there is for building top-down understanding.

## 6. Dev-loop layer: what the newcomer needs on day two

**Probe the environment before layer 1, not here** (it is a precondition of the whole survey, not a layer-6 chore): infer the toolchain from the build config (`pom.xml`→java/maven, `build.sbt`→sbt, `package.json`→node, `go.mod`→go, `pyproject.toml`→python), check each with `command -v`, and record "present / missing / version" as the first row of notes layer 6. Missing tools mean most of this layer's commands can only carry ⚠️ — **settle that conclusion at the very start of the survey**, not when you reach chapter 3 and discover you can verify nothing.

This layer's output goes almost verbatim into chapter 8 (and chapter 3). **Run every command yourself** (mark ▶); if you truly can't, mark ⚠️ and say why.

Establish and verify, one by one:

- **Environment requirements**: language versions, required build tools, external services needed (database / containers / JDK version). Don't just read the docs — run `--version` and confirm.
- **How to build**: the full build command plus **how long it takes** (first run vs incremental differ hugely; the book must say so or the newcomer will think it hung); whether you can build only the module you care about (in a big project this is life-saving).
- **How to test**: the command for all tests, **the command for a single test file / single case** (★ the single thing a newcomer needs most), how tests are organised (unit / integration / e2e; which need external services or are slow), whether there's a small set of "must pass before submitting" tests.
- **How to run a minimal example**: which example in examples/ or quickstart is most likely to succeed; what the output should look like.
- **Logs and observability**: where logs go (stdout / file / directory), how to change the level, whether there's a UI / dashboard / debug port.
- **Style and static checks**: the format command, lint/scalastyle/checkstyle/ruff config, what CI blocks on (and how to run all of it locally in one go).
- **Debugging**: the fastest way to add a log, whether you can attach a debugger (run from the IDE? remote debug port?), any ready-made debug switches / env vars.
- **Submission flow**: branch naming, commit-message rules, PR template, CLA/DCO signing, review cadence, where to look when CI fails.

The degraded path when the build is too expensive to run (common in big projects):
- build/test only one **small module** first, to prove the toolchain works;
- copy the full build command from `CONTRIBUTING.md` / CI config, and in the book mark it clearly ⚠️ not run + expected duration + source;
- reverse-engineer the real commands from `dev/`, CI scripts and `Makefile` (what CI runs is the authoritative answer).

## 7. Wrap-up: list the gaps

Before ending the survey, fill in these three tables and run the final coverage check in the notes — they directly determine the book's skeleton:

1. **Prerequisite gaps**: concepts the reader profile (junior programmer, zero domain background) won't know but that the spine and the layer-5 core abstractions require. For each: concept / why it's needed / one plain sentence / which chapter it will live in. Only the ones that **genuinely block understanding of the main line** go into chapter 1 "prerequisites crash course" (usually 3–6); the rest get a one-line backfill at the point of use, or an appendix.
2. **Pits and warnings**: every "this is unintuitive / the docs lie / easy to trip on" point you hit during the survey. These are the book's differentiating value — don't waste them.
3. **Unresolved questions**: every ❓. Decide per item while writing — verify it, or confess it in the book with an entry point for checking.
4. **Inventory coverage check**: go through the subsystem inventory row by row and confirm every item has its landing spot in the book filled in (full chapter / tour section / one map line + one glossary row); any blank row is a debt — go back and do the targeted look.

One last thing: invent **one homespun, everyday core metaphor** for the project (per `write4adhd-book`'s style rules). It must be able to carry the overall architecture, not just one concept — every structural layer the reader meets later should hang on it. If you can't find one, you don't yet have a grip on the project's overall shape; go back to layers 2 and 5.
