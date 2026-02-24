---
theme: default
colorSchema: dark
transition: fade
class: title-slide
fonts:
  sans: Geist
  mono: Geist Mono
---

# A Sled3 Retrospective
<p> In Thunderbolt </p>

---

# The Problem

- The sleds were **hard to manage and run**
  - Local
  - CI
- Tests were **flaky and slow**
- Sled2 used old browsers from <strong>'22–'23</strong>

---

# First Attempts & Failures

<div style="display:grid; grid-template-columns:1fr 1fr; gap:24px; align-items:start;">
<div>

- We kicked off with an AI-first workflow
- Naive prompt failed - AI **hallucinated** 
- Trial and Error led to a **Cursor Command** <code> /migrated-sled3</code>

</div>
<div style="display:flex; align-items:center; justify-content:center;">
  <img src="/command.gif" alt="Command demo" style="border-radius: var(--radius-card); max-height:340px;" />
</div>
</div>

---

# The 9-Phase Process

<div class="flowchart">
  <div class="fc-row">
    <div class="fc-node"><div class="fc-num">01</div><div class="fc-name">Script Migration</div></div>
    <div class="fc-arrow"></div>
    <div class="fc-node"><div class="fc-num">02</div><div class="fc-name">Static Checks</div></div>
    <div class="fc-arrow"></div>
    <div class="fc-node"><div class="fc-num">03</div><div class="fc-name">Code Changes</div></div>
  </div>
  <div class="fc-wrap-arrow"></div>
  <div class="fc-row">
    <div class="fc-node"><div class="fc-num">04</div><div class="fc-name">Screenshots</div></div>
    <div class="fc-arrow"></div>
    <div class="fc-node"><div class="fc-num">05</div><div class="fc-name">Run Tests</div></div>
    <div class="fc-arrow"></div>
    <div class="fc-node"><div class="fc-num">06</div><div class="fc-name">Debugging</div></div>
  </div>
  <div class="fc-wrap-arrow"></div>
  <div class="fc-row">
    <div class="fc-node"><div class="fc-num">07</div><div class="fc-name">Agent-Review</div></div>
    <div class="fc-arrow"></div>
    <div class="fc-node"><div class="fc-num">08</div><div class="fc-name">Validate</div></div>
    <div class="fc-arrow"></div>
    <div class="fc-node"><div class="fc-num">09</div><div class="fc-name">Create PR</div></div>
  </div>
</div>

<p style="position: absolute; bottom: 24px; right: 40px; font-size: 0.75rem;">
  <a href="https://github.com/wix-private/thunderbolt/blob/master/.cursor/commands/migrate-sled3.md" target="_blank" style="color: rgba(255,255,255,0.2); text-decoration: none;">View the full migrate-sled3 command →</a>
</p>

---

# Hardening the Infrastructure

<div style="display:grid; grid-template-columns:1fr 1fr; gap:24px; align-items:start;">
<div>

- `flakyless` went from crashing on <span class="red" style="font-weight:600">10+ tests</span> to running <a href="https://sled-reports.com/thunderbolt/45365/7c4a614-1782e54b-a7ef-440b-8fc4-b0ed9007926a/wix-thunderbolt/52fe66ae-b04a-4ba2-adbb-0edfe723f00e/" target="_blank"><span class="green" style="font-weight:600">150+ suites (~3k runs)</span></a> 
- Fixed sled infra bugs: 
  - function serialization
  - CLI multi-file running
  - Parallelism

</div>
<div style="display:flex; align-items:center; justify-content:center; height:100%; padding-left:24px;">
  <img src="/images/flakyless.png" alt="Flakyless report" style="border-radius: var(--radius-card); max-height:400px;" />
</div>
</div>

---

# Evolving with the Models
<br/>
<br/>
<br/>
<br/>
<div class="model-flow">
  <div class="model-card">
    <div class="model-name">Sonnet 4.5</div>
    <span class="model-tag ok">Good Enough</span>
  </div>
  <div class="model-arrow">→</div>
  <div class="model-card">
    <div class="model-name">Haiku 4.5</div>
    <span class="model-tag fast">Fast</span>
  </div>
  <div class="model-arrow">→</div>
  <div class="model-card breakthrough">
    <div class="model-name">GPT-5.2-Codex & <br/> Opus-4.5</div>
    <div class="model-desc">Skills + Subagents</div>
    <span class="model-tag breakthrough">Breakthrough</span>
  </div>
</div>

---

# Scaling Up

<div class="scaling-slide-v2">
<div class="scaling-top">
<div class="scaling-content">

- **Running agents in parallel**
- Multiple **git worktrees**

</div>
<div class="scaling-image-v2">
<img src="/images/sled3-diagram.png" alt="Sled3 scaling diagram" />
<p style="margin-top:0px" class="scaling-caption">12 parallel cursor CLI agents</p>
</div>
</div>
</div>

---

# Building Trust

<p style="margin-bottom: 12px;">Trust wasn't a decision — it was built incrementally</p>

<div class="trust-merged">
  <div class="trust-flow">
    <div class="flow">
      <div class="step"><span class="step-num">1</span><span> Battle tested <code>/migrate</code> command </span></div>
      <div class="step"><span class="step-num">2</span><span>Stable infrastructure — <code>flakyless</code></span></div>
      <div class="step"><span class="step-num">3</span><span>Deterministic steps delegated to <strong>bash scripts and skills</strong></span></div>
    </div>
  </div>
  <div class="trust-evidence">
    <img src="/images/validation.png" alt="Validation script output" />
    <img src="/images/pr-check.png" alt="PR check automation" />
  </div>
</div>

---

# The Mature Workflow
 <span>AI became a part of every step in the process, from creating the PR until its merged</span>
  <div class="workflow-comments">
    <p class="section-label">Real feedback → Agent fixes</p>
    <div class="pr-comments">
      <a class="pr-comment" href="https://github.com/wix-private/thunderbolt/pull/45474" target="_blank">"remove all imports from @wix/viewer-playwright-app in .ds. tests"</a>
      <a class="pr-comment" href="https://github.com/wix-private/thunderbolt/pull/45463" target="_blank">"Dont skip a test that was not skipped."</a>
      <a class="pr-comment" href="https://github.com/wix-private/thunderbolt/pull/45469" target="_blank">"why did we change this? did we get any value out of it?"</a>

  </div>
</div>

---
class: challenges-slide
---

# Challenges

<div class="challenges-grid">
  <div class="challenge-card">
    <div class="challenge-label">Silent Breakage</div>
    <p>A <code>test.only</code> slipped past CI and got merged</p>
    <div class="challenge-resolution">→ <u>Resolved with a lint rule</u></div>
  </div>
  <div class="challenge-card">
    <div class="challenge-label">False positive</div>
    <p>Screenshot tests were silently not running in CI</p>
    <div class="challenge-resolution">→ <u>Earlier throw local and CI</u></div>
  </div>
  <div class="challenge-card">
    <div class="challenge-label">Scaling</div>
    <p><code>flakyless</code> broke CI on new PRs</p>
    <div class="challenge-resolution">→ <u>Reinforcing the CI</u></div>
  </div>
</div>

---

# Results

<div class="metrics-row compact">
  <div class="metric-item">
    <div class="number">1,782</div>
    <div class="label">tests migrated</div>
  </div>
  <div class="metric-divider"></div>
  <div class="metric-item">
    <div class="number">1,255</div>
    <div class="label">files changed</div>
  </div>
  <div class="metric-divider"></div>
  <div class="metric-item">
    <div class="number">20/20</div>
    <div class="label">runs per test — no exceptions</div>
  </div>
</div>

<div class="results-grid">
  <div class="col col-accent-blue">
    <div class="col-title">Quality & Stability</div>
    <ul>
      <li>Every sled passed flakyless</li>
      <li>Real bugs found</li>
<li>Better Developer Experience</li>
    </ul>
  </div>
  <div class="col col-accent-cyan">
    <div class="col-title">Test Health & Cleanup</div>
    <ul>
      <li>Removed redundant tests</li>
      <li>Found tests that <strong>never actually ran</strong> their assertions</li>
      <li>Contributed to other wix repositories</li>
    </ul>
  </div>
  <div class="col col-accent-purple">
    <div class="col-title">Team Growth</div>
    <ul>
      <li>Added <strong>rules</strong> and <strong>skills</strong> for writing tests</li>
      <li>Team became <strong>knowledge pillars in AI and Testing</strong></li>
    </ul>
  </div>
</div>

---

# Final Thoughts and Looking Ahead

<div class="ponder-list">
  <div class="ponder-item">Opt out of deprecated testkits — embrace <strong>locator-based assertions</strong> and drop EE dependencies</div>

  <div class="ponder-item">Better <strong>metrics</strong> working with BI to track flakiness, health and pain points over time</div>

  <div class="ponder-item">More <strong>agentic workflows</strong> across the group (<code>VSM</code> 👀)</div>
  <div class="ponder-item"><strong>Unskip tests</strong> automatically investigate and fix currently-skipped tests</div>
  <div class="ponder-item">Migrate more test from E2E to integration, or deleted redundant ones</div>

</div>
