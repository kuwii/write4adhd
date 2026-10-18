# Markup cheat-sheet: how to use the code-kit components

Read this only when producing **HTML**. The base page structure, theme switching, Canvas diagrams and the animation scaffolding all come from `write4adhd-book`'s `references/html-craft.md` and its templates (single page: `assets/skeleton.html`; book site: `assets/site/`). This document only adds the pieces specific to developer docs.

⚠️ **Every snippet of code, path, line number and term below is a made-up illustrative example** (borrowing Spark for concreteness). Do not carry any of it into your deliverable as-is — always substitute the real content from your survey notes.

## 0. Wiring it in

First copy `code-kit.css` into the deliverable directory (book site: next to `style.css`; single page: merge its contents into the end of the inline `<style>`), then in every page's `<head>`:

```html
<link rel="stylesheet" href="style.css">
<link rel="stylesheet" href="code-kit.css">   <!-- must come after style.css -->
```

`code-kit.css` uses only the CSS variables that already exist in `style.css`; theme switching, the anti-flash script and the `__redraws` repaint mechanism need no changes.

## 1. Source snippet (with source line and attention marks)

```html
<p>What this does: at the end of its construction, <code>SparkContext</code> asks the scheduler for a "task launcher".</p>

<div class="code-block">
  <div class="code-head">
    <span>core/src/main/scala/org/apache/spark/SparkContext.scala:583</span>
    <span class="tag">scala</span>
  </div>
  <pre><code>  private[spark] def createDAGScheduler(): DAGScheduler = {
<span class="hl">    new DAGScheduler(this)</span>
  }
  <span class="dim">// … omitted: a long stretch of metrics and listener registration</span>
  _dagScheduler = <span class="mark">createDAGScheduler()</span>
</code></pre>
</div>

<p>Watch the highlighted line: the <code>DAGScheduler</code> is <strong>newed right here</strong>, not passed in from outside.</p>
```

Rules (details in `book-blueprint.md` section 5): one plain sentence before the block, one pointer after, ≤15 lines, never rename identifiers, at most 2 `.hl`/`.mark`/`.dim` marks per snippet, escape `<` `>` `&` inside `<pre>`. **Omissions are always shown as a `// … omitted: this does XXX` comment line, never as a blank line** — blank lines carry meaning in source (grouping, paragraph breaks), and readers will assume the source really has them.

## 2. Commands and output (two blocks)

```html
<div class="code-block">
  <div class="code-head"><span>terminal</span><span class="tag">~8 min; first run downloads dependencies</span></div>
  <pre><code>$ ./build/mvn -pl core -am package -DskipTests</code></pre>
</div>

<div class="code-block out">
  <div class="code-head"><span>output (trimmed)</span></div>
  <pre><code>[INFO] Building Spark Project Core 4.3.0-SNAPSHOT
…
[INFO] BUILD SUCCESS
[INFO] Total time:  07:41 min</code></pre>
</div>
```

Commands with side effects (changing config, installing things, deleting files, starting background services) get a `.warn` in front, saying what they touch and how to undo. A command you didn't run gets `<span class="pill warn">⚠️ not run</span>` in the `.code-head`, plus its source.

## 3. File tree

```html
<div class="code-block file-tree">
  <div class="code-head"><span>repo top level</span><span class="tag">orange = look now, grey = ignorable on a first pass</span></div>
  <pre><code><span class="hot">core/</span>          <span class="c"># the engine itself: scheduling, memory, shuffle</span>
<span class="hot">sql/</span>           <span class="c"># DataFrames and the optimizer; most-changed area</span>
<span class="cold">mllib/</span>         <span class="c"># machine-learning library; off the main line</span>
<span class="cold">docs/</span>          <span class="c"># website source, not docs meant for you</span>
pom.xml        <span class="c"># Maven master config; the module list lives here</span>
</code></pre>
</div>
```

## 4. Tables

Narrow content: a bare `<table class="striped">`. Wide tables (≥5 columns, or long code inside) go inside `.table-wrap` so they scroll sideways instead of breaking the layout.

```html
<div class="table-wrap">
  <table class="term-table striped">
    <thead>
      <tr><th>Term</th><th>Plain words</th><th>Original</th><th>What it's called in code</th><th>Defined at</th></tr>
    </thead>
    <tbody>
      <tr>
        <td>driver</td>
        <td>the process running your <code>main</code>; it gives the orders</td>
        <td>driver</td>
        <td><code>SparkContext</code></td>
        <td><code>core/…/SparkContext.scala</code></td>
      </tr>
    </tbody>
  </table>
</div>
```

The jargon glossary page is one big `.term-table`; every row must have the "what it's called in code" column — that column is what separates a glossary from an ordinary term list.

## 5. Hands-on task box

```html
<div class="hands-on">
  <div class="ho-head">🔧 Hands-on, 5 minutes: make it announce its own name</div>
  <ol>
    <li>Open <code class="path">core/src/main/scala/…/DAGScheduler.scala</code> and find <code>handleJobSubmitted</code>.</li>
    <li>Add <code>logInfo("I got here: " + job.jobId)</code> as the method's first line.</li>
    <li>Rebuild just this module and run the minimal example: the command is in chapter 3.</li>
    <li><strong>You will see</strong>: a line "I got here: 0" in the logs. Seeing it means you can now change this project's code.</li>
  </ol>
  <p class="undo">If it breaks: <code>git checkout -- .</code> (throws away all uncommitted changes).</p>
</div>
```

Four non-optional elements: exact location, exact command, **success criterion (what you will see)**, recovery method.

## 6. End-of-chapter checkpoint

```html
<div class="checkpoint">
  <div class="cp-head">After this chapter you can:</div>
  <ul>
    <li>say which two processes exist after <code>./bin/pyspark</code> starts, and how they talk</li>
    <li>point at the file that defines <code>SparkContext</code></li>
    <li>explain in one sentence why a <code>show()</code> has to pass through the scheduler</li>
  </ul>
</div>
```

≤3 items, right after the closing line. Can't answer = go re-read that section.

## 7. "Skippable on a first pass" mark

```html
<h3>The 80/20 execution/storage split in memory management <span class="skip-flag">skippable on a first pass</span></h3>
```

Hangs on `h2`/`h3` only; at most 1/3 of all sections across the book. The TOC page must explain what the mark means.

## 8. Pits and warnings

```html
<div class="warn">
  <p><span class="warn-head">⚠️ Pit:</span> this config key was renamed after 3.0. Most tutorials online show the old name; setting it does nothing and reports nothing.</p>
  <p>The new name is <code>spark.sql.shuffle.partitions</code>, see <code class="path">sql/catalyst/…/SQLConf.scala</code>.</p>
</div>
```

`.warn` (purple) = things that bite; `.callout` (orange, from `write4adhd-book`) = self-posed Q&A. Don't mix them.

## 9. Confidence pills

Use them in the prose when confessing uncertainty; never present an inference as a fact. **Pills carry short labels only** (under ~10 words); explain the reason in a separate sentence or a `.warn` — a whole sentence inside a pill breaks narrow layouts:

```html
<span class="pill ok">▶ ran it</span>
<span class="pill warn">⚠️ not run</span>
<span class="pill unk">❓ unconfirmed</span>
<p>This command is copied from <code class="path">CONTRIBUTING.md</code> and was never run on the writing machine; to confirm, start looking at <code>xxx</code>.</p>
```

## 10. Numbered steps

For linear flows like "how to run it" or "how to open a PR", use `.steps` — it carries per-step explanation text better than the `.flow-list` pill chain:

```html
<ol class="steps">
  <li><strong>Check the JDK version.</strong> This project needs 17; look with <code>java -version</code>. Wrong one? Switch — don't brute-force it.</li>
  <li><strong>Build only the module you'll change.</strong> A full build takes ~20 minutes; one module ~3.</li>
  <li><strong>Run a single test.</strong> The command is in the block below.</li>
</ol>
```

## 11. Diagrams

Follow `write4adhd-book`'s Canvas rules and the `shared.js` helpers; `code-kit` is not involved. Of this book's six required diagrams (ecosystem position / process topology / modules+layering / ownership / spine sequence / subsystem panorama), **make the ownership diagram and the spine sequence diagram `setupAnim` step animations** where possible: call chains and the layered construction of abstractions are essentially temporal, and stepping through them beats showing everything at once. The animation bar (pause / step / replay) and auto-pause off-viewport stay mandatory.

## 12. Parts and TOC grouping

Past ~10 chapters the book splits into parts (see `book-blueprint.md` section 2). No new components needed: the TOC page uses numbered `h2`s as part headers, and chapter badges carry the part number.

```html
<!-- TOC page: grouped by part -->
<h2><span class="num">Ⅱ</span> Foundations</h2>
<p class="muted">After this part: the vocabulary every chapter needs; guess where code lives from directories; say what each core class can do.</p>
<nav class="toc">
  <a class="toc-item" href="ch4.html">…</a>
  <a class="toc-item" href="ch5.html">…</a>
</nav>
<h2><span class="num">Ⅲ</span> Walk-through</h2>
…
```

```html
<!-- chapter page: badge carries the part number -->
<span class="badge">Part Ⅱ · Chapter 6</span>
```

## 13. Responsibility-surface tables and tour sections

**Responsibility-surface tables** (chapter 6) use a plain `.striped` table with the method-group name in the first column; one per core abstraction, preceded by one plain sentence ("this class does exactly these four groups of things in its life"):

```html
<div class="table-wrap">
  <table class="striped">
    <thead><tr><th>Method group</th><th>Representatives</th><th>What it does</th><th>Which requests land here</th></tr></thead>
    <tbody>
      <tr><td>Submit jobs</td><td><code>runJob</code> / <code>submitJob</code></td><td>hand an RDD + function to the scheduler</td><td>changing the submission entry, job-level hooks</td></tr>
    </tbody>
  </table>
</div>
```

**Tour sections** (chapter 9) keep a fixed four-beat shape, and stay short: what it solves (one sentence) → entry and core classes (`.file-tree` or a small table, with locations) → interface to the spine (one sentence; "standalone; interface is X" when it doesn't connect) → first stop when changing it (one sentence + optional read-only hands-on task). **Never mark them skippable** — these sections exist precisely for "the reader's next task is there".
