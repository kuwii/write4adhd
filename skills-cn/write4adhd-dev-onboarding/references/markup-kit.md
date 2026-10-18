# 标记速查：code-kit 组件怎么用

只在产出 **HTML** 时读本文。基础页面结构、主题切换、Canvas 图表与动画脚手架全部照 `write4adhd-book` 的 `references/html-craft.md` 和它的模板（单页 `assets/skeleton.html`，书站 `assets/site/`），本文只补开发者文档特有的那几块。

⚠️ **本文所有代码、路径、行号、术语都是示范用的假例子**（拿 Spark 举的），不要原样搬进产物——写作时一律换成你调查笔记里的真实内容。

## 0. 接入方式

先把 `code-kit.css` 复制到产物目录（书站：与 `style.css` 并列；单页：把内容并入内联 `<style>` 末尾），然后每页 `<head>` 里：

```html
<link rel="stylesheet" href="style.css">
<link rel="stylesheet" href="code-kit.css">   <!-- 必须在 style.css 之后 -->
```

`code-kit.css` 只用 `style.css` 里已有的 CSS 变量，主题切换、防闪烁脚本、`__redraws` 重绘机制都不需要改。

## 1. 源码片段（带出处与注意力标注）

```html
<p>这段在干什么：<code>SparkContext</code> 构造到最后，会去向调度器要一个"任务发射器"。</p>

<div class="code-block">
  <div class="code-head">
    <span>core/src/main/scala/org/apache/spark/SparkContext.scala:583</span>
    <span class="tag">scala</span>
  </div>
  <pre><code>  private[spark] def createDAGScheduler(): DAGScheduler = {
<span class="hl">    new DAGScheduler(this)</span>
  }
  <span class="dim">// … 省略：这里还有一大段 metrics 与 listener 的注册</span>
  _dagScheduler = <span class="mark">createDAGScheduler()</span>
</code></pre>
</div>

<p>注意看高亮那一行：<code>DAGScheduler</code> 是<strong>在这里被 new 出来的</strong>，不是外面传进来的。</p>
```

规矩（详见 `book-blueprint.md` 第 5 节）：块前一句人话、块后指路、≤15 行、不改标识符、`.hl`/`.mark`/`.dim` 一段最多标 2 处、`<pre>` 里的 `<` `>` `&` 要转义。**省略一律用 `// … 省略：这里做 XXX` 注释行表示，不要用空行**——空行在源码里是有含义的（分组、段落），读者会误以为源码里真有空行。

## 2. 命令与输出（分两块）

```html
<div class="code-block">
  <div class="code-head"><span>终端</span><span class="tag">约 8 分钟，第一次会下载依赖</span></div>
  <pre><code>$ ./build/mvn -pl core -am package -DskipTests</code></pre>
</div>

<div class="code-block out">
  <div class="code-head"><span>输出（已裁剪）</span></div>
  <pre><code>[INFO] Building Spark Project Core 4.3.0-SNAPSHOT
…
[INFO] BUILD SUCCESS
[INFO] Total time:  07:41 min</code></pre>
</div>
```

有副作用的命令（改配置、装东西、删文件、起后台服务）前面挂一个 `.warn`，写清它动了什么、怎么撤销。没实测过的命令，在 `.code-head` 里加 `<span class="pill warn">⚠️ 未实测</span>` 并写出处。

## 3. 文件树

```html
<div class="code-block file-tree">
  <div class="code-head"><span>仓库顶层</span><span class="tag">橙色 = 现在就要看，灰色 = 第一遍可忽略</span></div>
  <pre><code><span class="hot">core/</span>          <span class="c"># 引擎本体：调度、内存、shuffle</span>
<span class="hot">sql/</span>           <span class="c"># DataFrame 与优化器，改动最频繁</span>
<span class="cold">mllib/</span>         <span class="c"># 机器学习库，跟主线无关</span>
<span class="cold">docs/</span>          <span class="c"># 官网源码，不是给你读的文档</span>
pom.xml        <span class="c"># Maven 总配置，模块列表在这里</span>
</code></pre>
</div>
```

## 4. 表格

窄内容直接 `<table class="striped">`；宽表（≥5 列或含长代码）外面包 `.table-wrap` 让它横向滚动，不要撑破版面。

```html
<div class="table-wrap">
  <table class="term-table striped">
    <thead>
      <tr><th>术语</th><th>人话</th><th>英文</th><th>代码里叫什么</th><th>定义位置</th></tr>
    </thead>
    <tbody>
      <tr>
        <td>驱动器</td>
        <td>跑你 <code>main</code> 的那个进程，负责发号施令</td>
        <td>driver</td>
        <td><code>SparkContext</code></td>
        <td><code>core/…/SparkContext.scala</code></td>
      </tr>
    </tbody>
  </table>
</div>
```

黑话词典页整页就是一张 `.term-table`，每行都必须有"代码里叫什么"——这一列是词典和普通术语表的区别。

## 5. 动手任务框

```html
<div class="hands-on">
  <div class="ho-head">🔧 动手 5 分钟：让它自己报出名字</div>
  <ol>
    <li>打开 <code class="path">core/src/main/scala/…/DAGScheduler.scala</code>，找到 <code>handleJobSubmitted</code>。</li>
    <li>在方法第一行加一句 <code>logInfo("我到这里了：" + job.jobId)</code>。</li>
    <li>只重新构建这个模块并跑那个最小示例：命令见第 3 章。</li>
    <li><strong>你会看到</strong>：日志里出现一行"我到这里了：0"。看到它就说明你已经能改这个项目的代码了。</li>
  </ol>
  <p class="undo">改坏了怎么恢复：<code>git checkout -- .</code>（会丢掉所有未提交的修改）。</p>
</div>
```

四要素缺一不可：确切位置、确切命令、**成功判据（你会看到什么）**、恢复方法。

## 6. 章末检查点

```html
<div class="checkpoint">
  <div class="cp-head">读完这章你应该能：</div>
  <ul>
    <li>说出 driver 和 executor 各是一个什么进程、谁先启动</li>
    <li>指出 <code>SparkContext</code> 定义在哪个文件</li>
    <li>用一句话说清一次 <code>show()</code> 为什么要经过调度器</li>
  </ul>
</div>
```

≤3 条，紧跟在收束句后面。答不上来 = 回去重读那一节。

## 7. 「第一遍可跳过」标记

```html
<h3>内存管理里的执行/存储二八分账 <span class="skip-flag">第一遍可跳过</span></h3>
```

只挂在 `h2`/`h3` 上，全书不超过节数的 1/3。目录页要说明这个标记的含义。

## 8. 坑与警告

```html
<div class="warn">
  <p><span class="warn-head">⚠️ 坑：</span>这个配置项在 3.0 之后改了名字，网上大部分教程写的是旧名字，设了不生效也不报错。</p>
  <p>新名字是 <code>spark.sql.shuffle.partitions</code>，见 <code class="path">sql/catalyst/…/SQLConf.scala</code>。</p>
</div>
```

`.warn`（紫）= 会咬人的东西；`.callout`（橙，来自 `write4adhd-book`）= 自问自答。别混用。

## 9. 置信度胶囊

正文里坦白不确定时用，别把推断写成断言。**胶囊里只放短标签**（十个字以内）；要解释原因就另起一句正文或一个 `.warn`，别把整句话塞进胶囊（窄屏下会撑破版面）：

```html
<span class="pill ok">▶ 实测</span>
<span class="pill warn">⚠️ 未实测</span>
<span class="pill unk">❓ 没确认</span>
<p>这条命令抄自 <code class="path">CONTRIBUTING.md</code>，写书的机器上没跑过；想确认可以从 <code>xxx</code> 查起。</p>
```

## 10. 编号步骤

"怎么跑起来""怎么提 PR"这类线性流程用 `.steps`，比 `.flow-list` 胶囊链更能承载每步的说明文字：

```html
<ol class="steps">
  <li><strong>确认 JDK 版本。</strong>这个项目要 17，用 <code>java -version</code> 看一眼；不对就换，别硬试。</li>
  <li><strong>只构建你要改的模块。</strong>完整构建约 20 分钟，单模块约 3 分钟。</li>
  <li><strong>跑单个测试。</strong>命令见下面那块。</li>
</ol>
```

## 11. 图表

沿用 `write4adhd-book` 的 Canvas 规范与 `shared.js` 辅助函数，不需要 `code-kit` 参与。本书的六张必备图（生态位置 / 进程拓扑 / 模块+分层 / 持有关系 / 脊柱时序 / 子系统全景）里，**持有关系图和脊柱时序图优先做成 `setupAnim` 分步动画**：调用链和抽象的层层构造本质是时间性的，分步演示比一次性画完更好懂。动画条（暂停/单步/重播）和滚出视口自动暂停照旧，不能省。

## 12. 部分（parts）与目录页分组

书超过 ~10 章就要分部分（见 `book-blueprint.md` 第 2 节）。不需要新组件：目录页用带编号的 `h2` 做部分头，章页 badge 带上部分号。

```html
<!-- 目录页：按部分分组 -->
<h2><span class="num">Ⅱ</span> 地基</h2>
<p class="muted">读完这部分：看懂任何一章需要的名词；按目录猜代码在哪；说出核心类各自能干什么。</p>
<nav class="toc">
  <a class="toc-item" href="ch4.html">…</a>
  <a class="toc-item" href="ch5.html">…</a>
</nav>
<h2><span class="num">Ⅲ</span> 走读</h2>
…
```

```html
<!-- 章页：badge 带部分号 -->
<span class="badge">部分 Ⅱ · 第 6 章</span>
```

## 13. 职责面表与巡览节

**职责面表**（第 6 章）用普通 `.striped` 表，第一列是方法分组名；每个核心抽象一张，表前一句人话（“这个类一辈子就干这四组事”）：

```html
<div class="table-wrap">
  <table class="striped">
    <thead><tr><th>方法分组</th><th>代表方法</th><th>干什么</th><th>哪类需求落到这</th></tr></thead>
    <tbody>
      <tr><td>提交作业</td><td><code>runJob</code> / <code>submitJob</code></td><td>把 RDD + 函数交给调度器</td><td>改调度入口、加作业级钩子</td></tr>
    </tbody>
  </table>
</div>
```

**巡览节**（第 9 章）每节固定四小段，保持短：它解决什么（一句）→ 入口与核心类（`.file-tree` 或小表，带位置）→ 和脊柱的接口（一句，接不上就写“独立运行，接口是 X”）→ 改它的第一站（一句 + 一个只读动手任务可选）。**不标 skip-flag**——这些节就是为“读者下一个任务在那儿”写的。
