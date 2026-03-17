# OpenCode Go Agents

A multi-agent system for [OpenCode](https://opencode.ai) that closes the gap with Claude Opus-level reasoning at a fraction of the cost. Uses OpenCode Go models (GLM-5, Kimi K2.5, MiniMax M2.5) with prompt-engineered agent orchestration, structured debates, chain-of-thought enforcement, and mechanical verification -- techniques derived from studying Claude Code's own system prompts.

**$10/month subscription vs. $90/month for Claude Code, with comparable reasoning depth on coding tasks.**

## The Brain / Muscle / Operations Philosophy

The core insight: **if programming becomes trivial under clear specifications, the value is no longer in the code-writing model, but in the one engineering the instructions.**

This system assigns models to three role categories based on where they add the most value:

| Category | Model | Cost ($/MTok out) | What it does | Agents |
|----------|-------|-------------------|-------------|--------|
| **Brain** | GLM-5 | $2.30 | Reasons, plans, designs, tests, analyzes. Engineers the precise instruction blocks that make execution trivial. | planner, reasoner, architect, advocate, critic, auditor, tester |
| **Muscle** | Kimi K2.5 | $2.20 | Executes code from specs. Follows Brain instructions faithfully. Also routes as the orchestrator. | auto (orchestrator), executor, debugger, autoresearch |
| **Operations** | MiniMax M2.5 | $1.20 | Mechanical, routine tasks. Compares outputs, maps structure, aggregates context, handles docs and git. | verifier, mapper, synthesizer, docs-manager, git-manager |
| **Emergency** | Claude Opus 4.6 | $25.00 | Fallback only. Handles work directly when Go models fail. | auto-fallback |

**Why this split works:** The Brain (GLM-5) is the most expensive Go model, but it only fires for tasks that require genuine reasoning -- planning, architecture, test strategy design, adversarial analysis. The Muscle (Kimi K2.5) handles the volume work (writing code, fixing bugs) at slightly lower cost, following the Brain's precise specifications. Operations (MiniMax M2.5) handles everything mechanical at half the cost.

The orchestrator (Auto) stays on Kimi K2.5 because it manages wide context and selects agents -- a routing and context-management job, not a deep reasoning job.

## Why This Exists

Claude Code's $90/month subscription gives you Claude Opus 4.6 ($5/$25 per million input/output tokens on the API) with tight tool integration. It produces excellent results because Anthropic's system prompts enforce structured reasoning, adversarial verification, evidence-based analysis, and engineering discipline.

This project replicates those techniques on top of much cheaper models:

| Model | Input $/MTok | Output $/MTok | Role in this system |
|-------|-------------|--------------|---------------------|
| MiniMax M2.5 | $0.25 | $1.20 | Operations: verification, mapping, aggregation, docs, git |
| Kimi K2.5 | $0.45 | $2.20 | Muscle + Orchestration: code execution, debugging, routing |
| GLM-5 | $0.72 | $2.30 | Brain: planning, reasoning, architecture, testing, analysis |
| Claude Sonnet 4.6 | $3.00 | $15.00 | (reference -- Claude Code inner model) |
| Claude Opus 4.6 | $5.00 | $25.00 | Emergency fallback only, via GitHub Copilot credits |

**Per-token, GLM-5 is 7x cheaper than Opus on input and 11x cheaper on output.** Kimi K2.5 is 11x and 11x cheaper respectively. MiniMax M2.5 is 20x and 21x cheaper.

### Cost Comparison

With the OpenCode Go subscription at **$10/month**, you get access to all three Go models with generous rate limits. Equivalent API costs for the same token volume would be approximately **$50-70/month** at pay-per-token rates.

Claude Code at **$90/month** provides direct Opus 4.6 access. Equivalent API costs for the same usage would be **$150-250/month** depending on volume.

| | OpenCode Go Agents | Claude Code |
|---|---|---|
| **Monthly cost** | $10 | $90 |
| **Equivalent API cost** | ~$60 | ~$200 |
| **Cost ratio** | **1x** | **~3-4x** |
| **Primary model cost (per MTok out)** | $2.30 (GLM-5 Brain) / $2.20 (Kimi Muscle) | $25.00 (Opus 4.6) |
| **Reasoning quality** | Engineered via multi-agent debate + CoT | Native (single model) |
| **Fallback** | Claude Opus 4.6 via Copilot | N/A (already Opus) |

The key insight: **a $2.30/MTok Brain model engineering instructions for a $2.20/MTok Muscle model produces results competitive with a $25/MTok model running as a single agent.** The orchestration overhead adds ~30-40% to token usage, but at these rates, the overhead is negligible compared to the 11x per-token savings.

### How We Close the Gap

Raw model capability is only part of the equation. Claude Code's advantage comes significantly from its system-level engineering:

1. **Structured reasoning enforcement** -- Claude Code's prompts force chain-of-thought, evidence citation, and multi-step analysis. We replicate this with mandatory `<thinking>` blocks on every Brain agent.
2. **Test strategy design** -- Our @tester agent (GLM-5 Brain) designs comprehensive test strategies informed by real-world practices from 100+ major software companies, covering test pyramid, chaos engineering, contract testing, mutation testing, and more.
3. **Engineering discipline** -- Claude Code's prompts enforce scope control, minimal changes, no over-engineering. We replicate this in executor, debugger, and architect agents.
4. **Multi-perspective analysis** -- Claude Code achieves this through a single powerful model reasoning from multiple angles. We achieve it through literal debate between two GLM-5 Brain agents with opposing objectives.

Where Claude Code still wins: novel problems requiring broad world knowledge, extremely long context windows, and tasks where raw model intelligence matters more than process. For standard software engineering (implementing features, fixing bugs, refactoring, code review), the gap is small.

## Benchmarks

### Visual Comparison

<table align="center">
  <tr>
    <th align="center">Claude Code (Opus 4.6)</th>
    <th align="center">OpenCode Go Agents</th>
  </tr>
  <tr>
    <td align="center"><img src="benchmarks%20results/claude-result.gif" width="350" height="auto" alt="Claude Result"></td>
    <td align="center"><img src="benchmarks%20results/go-result.gif" width="350" height="auto" alt="Go Result"></td>
  </tr>
  <tr>
    <td align="center"><strong>17,562 tokens</strong></td>
    <td align="center"><strong>31,966 tokens</strong></td>
  </tr>
  <tr>
    <td align="center">~$0.44</td>
    <td align="center">~$0.09</td>
  </tr>
</table>

### Token Usage Breakdown

Real-world comparison on a complex creative coding task (30-second seamless looping printer animation with CSS keyframes, DOM manipulation, and JavaScript):

| Metric | Claude Code (Opus 4.6) | OpenCode Go Agents |
|--------|------------------------|-------------------|
| **Total Tokens** | 17,562 | 31,966 |
| **Input Tokens** | ~4,500 | ~12,000 |
| **Output Tokens** | ~13,062 | ~19,966 |
| **Models Used** | Claude Opus 4.6 (single) | GLM-5 + Kimi K2.5 + MiniMax M2.5 (orchestrated) |
| **Agent Calls** | 1 (direct) | 4 (@advocate, @critic, @synthesizer, @executor) |

### API Cost Analysis

Pricing comparison at pay-per-token rates:

| Model | Input $/MTok | Output $/MTok | Task Cost |
|-------|-------------|--------------|-----------|
| **Claude Opus 4.6** | $5.00 | $25.00 | **~$0.44** (17,562 tokens) |
| **OpenCode Go (Mixed)** | $0.25-0.72 | $1.20-2.30 | **~$0.09** (31,966 tokens) |
| **GLM-5 (solo)** | $0.72 | $2.30 | ~$0.05-0.08 (est. 20k tokens) |
| **Kimi K2.5 (solo)** | $0.45 | $2.20 | ~$0.05-0.07 (est. 20k tokens) |

**Monthly API Cost Projection** (based on 500k tokens/day usage):

| Service | Daily Cost | Monthly Cost | Savings |
|---------|-----------|--------------|---------|
| Claude Opus 4.6 API | ~$12.50 | **~$375** | - |
| Claude Code subscription | ~$3.00 | **$90** | 76% vs API |
| OpenCode Go subscription | ~$0.33 | **$10** | 97% vs Claude API, 89% vs Claude Code |
| OpenCode Go pay-per-use | ~$0.80 | **~$24** | 94% vs Claude API |

### Which is Better?

**Choose OpenCode Go Agents when:**
- Cost efficiency is priority (80% cheaper on complex tasks)
- You want structured reasoning with adversarial debate
- Tasks benefit from multi-agent verification (architecture, debugging)
- You need 90%+ of Claude's capability at 11-20x lower cost

**Choose Claude Code when:**
- Budget is not a constraint
- You need maximum model intelligence for novel problems
- Tasks require extremely long context windows (>200k tokens)
- You prefer simplicity over cost optimization
- Speed is priority (single model, no debate overhead)

**Verdict:** For standard software engineering tasks (features, bugs, refactoring), OpenCode Go Agents deliver comparable results at **11x lower API cost** and **9x lower subscription cost**. The 82% higher token count is more than offset by the per-token price advantage. The multi-agent debate adds ~20-30 seconds of latency but catches errors that single-model approaches miss.

### Task Complexity

This benchmark task involved:
- Creative design decisions (animation approach, timing calculations)
- Discussion Protocol debate between @advocate and @critic agents
- CSS keyframe animation engineering with GPU-composited properties
- Single-file HTML/CSS/JS implementation
- Seamless 30-second loop with proper layering
- Start button with animation control

See `benchmarks results/` directory for source video recordings.

## Architecture

Every user message goes through the **Auto** orchestrator (Kimi K2.5), which reads relevant code, prompt-engineers structured XML instruction blocks, and routes to the best agent for the job. Brain agents (GLM-5) do the thinking; Muscle agents (Kimi K2.5) do the coding; Operations agents (MiniMax M2.5) handle the routine work.

```
User message
    |
    v
 Auto (Kimi K2.5) -- reads code, reasons, engineers structured prompt
    |
    |-- Simple question? ----------> Answers directly (no agent cost)
    |-- Need exploration? ----------> @mapper (MiniMax M2.5 Operations)
    |-- Need deep analysis? --------> @reasoner (GLM-5 Brain)
    |-- Need complex planning? -----> @planner (GLM-5 Brain)
    |-- Need code work? ------------> @executor (Kimi K2.5 Muscle) / @architect (GLM-5 Brain)
    |-- Need bug fixing? -----------> @debugger (Kimi K2.5 Muscle)
    |-- Need code review? ----------> @auditor (GLM-5 Brain, read-only)
    |-- Need test strategy? --------> @tester (GLM-5 Brain) -> @executor -> @verifier
    |-- Need validation? -----------> @verifier (MiniMax M2.5 Operations, read-only)
    |-- Documentation? -------------> @docs-manager (MiniMax M2.5 Operations)
    |-- Git operations? ------------> @git-manager (MiniMax M2.5 Operations)
    |-- ML experiments? -----------> @autoresearch (Kimi K2.5 Muscle, autonomous loop)
    |-- Ambiguous approach? --------> DISCUSSION PROTOCOL (see below)
    |
    v
 Failed? --> Tab to switch --> Auto-Fallback (Claude Opus 4.6, handles directly)
```

## Agents

| Agent | Model | Category | Role | Tool Access |
|-------|-------|----------|------|-------------|
| **auto** | Kimi K2.5 | Orchestrator | Prompt-engineering orchestrator, final decision-maker | Read-only |
| **auto-fallback** | Claude Opus 4.6 | Emergency | Direct fallback handler | Full |
| | | | | |
| **planner** | GLM-5 | Brain | Task breakdown, structured instruction output | Read-only, no bash |
| **reasoner** | GLM-5 | Brain | Deep analysis, trade-off evaluation | Read-only, no bash |
| **architect** | GLM-5 | Brain | System design, API contracts | Full |
| **advocate** | GLM-5 | Brain | Proposes best approach in debates | Read-only, no bash |
| **critic** | GLM-5 | Brain | Stress-tests proposals, finds weaknesses | Read-only, no bash |
| **auditor** | GLM-5 | Brain | Code review, security, performance audit | Read-only |
| **tester** | GLM-5 | Brain | Test strategy design, edge-case coverage | Read-only, no bash |
| | | | | |
| **executor** | Kimi K2.5 | Muscle | Code implementation, tests, refactoring | Full |
| **debugger** | Kimi K2.5 | Muscle | Bug investigation and fixing | Full |
| **autoresearch** | Kimi K2.5 | Muscle | Autonomous ML experiment loops (Apple Silicon/MPS) | Full |
| | | | | |
| **verifier** | MiniMax M2.5 | Operations | Mechanical output comparison, test validation | Read-only |
| **mapper** | MiniMax M2.5 | Operations | Codebase exploration, structure mapping | Read-only, no bash |
| **synthesizer** | MiniMax M2.5 | Operations | Aggregates debate context for orchestrator | Read-only, no bash |
| **docs-manager** | MiniMax M2.5 | Operations | Documentation writing and maintenance | Full |
| **git-manager** | MiniMax M2.5 | Operations | Git operations, commits, branches, PRs | Full |

## System Prompt Engineering

The agent prompts in this system were developed by studying **25 of Claude Code's own system prompts** (from Anthropic's public repository) and extracting the techniques that make Claude Code effective. These techniques are model-agnostic -- they work on any capable code model, not just Claude.

### Chain-of-Thought Enforcement

Every Brain agent has a **mandatory `<thinking>` block** that must appear before any output. Each agent gets a role-specific thinking protocol:

| Agent | Category | Thinking Focus |
|-------|----------|---------------|
| **architect** | Brain | Component dependency mapping, extension point analysis, trade-off evaluation |
| **planner** | Brain | Task decomposition, dependency graph, critical path identification |
| **reasoner** | Brain | Multi-step reasoning chains with intermediate conclusions |
| **advocate** | Brain | Solution space exploration, criteria alignment scoring |
| **critic** | Brain | Independent claim verification, structured weakness search |
| **auditor** | Brain | Risk assessment, security analysis, performance evaluation |
| **tester** | Brain | Test strategy design, risk-based coverage, edge-case identification |

The thinking structure follows the GIVEN -> EVIDENCE -> ANALYSIS -> GAPS -> CONCLUSION template, forcing the model through a complete reasoning chain rather than jumping to conclusions.

### Evidence-First Reasoning

All Brain agents enforce 5 evidence rules:

1. **Read code BEFORE forming conclusions.** No reasoning from assumptions.
2. **Every claim must cite `file:line`.** Vague assertions are prohibited.
3. **Unsupported claims must be marked `[UNVERIFIED]`.** No presenting speculation as fact.
4. **Distinguish observation from inference.** Label each explicitly.
5. **Counter-evidence cannot be ignored.** Must be addressed, not hidden.

This is adapted from Claude Code's internal approach where the model is trained to ground reasoning in concrete code references rather than abstract analysis.

### Test Strategy Design (from howtheytest)

The @tester agent (GLM-5 Brain) designs test strategies informed by real-world practices from 100+ major software companies via the [howtheytest](https://github.com/abhivaikar/howtheytest) repository. It incorporates:

- **Test Pyramid / Honeycomb / Trophy** -- structural frameworks for test distribution
- **Contract Testing** -- Pact-style interface verification between services
- **Chaos Engineering** -- Netflix/Amazon-style failure injection and resilience testing
- **Visual Regression** -- Chromatic/Percy-style screenshot comparison
- **Property-Based Testing** -- Hypothesis/QuickCheck-style generative testing
- **Mutation Testing** -- Stryker/PIT-style code mutation to verify test effectiveness
- **Shift-Left Testing** -- moving test design upstream into planning
- **Risk-Based Testing** -- prioritizing test coverage by business and technical risk
- **Quality Assistance Model** -- Atlassian's approach of embedding quality in teams

The tester produces `<test-strategy>` blocks that @executor implements and @verifier validates.

### Mechanical Verification

The @verifier agent (MiniMax M2.5 Operations) performs mechanical comparison of actual vs. expected output:

- Runs commands and captures output
- Compares against expected behavior specified in instruction blocks
- Reports PASS/FAIL with exact command evidence
- Machine-parseable `VERDICT: PASS/FAIL/PARTIAL` format

This is deliberately simple -- deep test reasoning belongs to @tester (Brain), not @verifier (Operations).

### Engineering Discipline (from Claude's "Doing Tasks" Prompts)

The executor, debugger, and architect agents enforce engineering rules extracted from Claude Code's system prompts:

- **Scope control**: Only make changes directly requested. A bug fix does not need surrounding code cleaned up.
- **No over-engineering**: No premature abstractions (need 3+ occurrences), no unnecessary error handling (only at system boundaries), no compatibility hacks.
- **Delete unused code**: When replacing functionality, remove the old implementation completely.
- **When blocked**: Reconsider the approach rather than brute-forcing. Never bypass safety checks.

### Executing with Care (from Claude's Blast-Radius Framework)

The executor and git-manager agents assess **reversibility and blast radius** before every action:

- **Freely take**: Local, reversible actions (editing files, running tests)
- **Pause and verify**: Hard-to-reverse actions (deleting files, modifying configs)
- **Never without permission**: Shared-state actions (pushing, force-pushing, database modifications)

### Reasoning Decomposition

When Auto detects a deep reasoning task (5+ logical steps), it decomposes it into a chain of sequential @reasoner calls, each focused on one sub-question, with outputs chaining as context to the next. This prevents the shallow analysis that single-call reasoning produces on complex multi-dimensional questions.

### Multi-Pass Verification

For complex or high-stakes reasoning, Auto sends the agent's own output back to the SAME agent with a `<verify-reasoning>` prompt that audits evidence citations, logic chain integrity, blind spots, and confidence calibration. Triggered when tasks are complex, high-stakes, or the agent's confidence is medium/low.

### Output Efficiency (from Claude's Output Efficiency Prompt)

Auto's communication follows Claude Code's output efficiency directive: lead with the answer, not the reasoning. Skip filler, preamble, and transitions. One sentence over three.

### Never Delegate Understanding (from Claude's Subagent Prompt Writing)

Auto's instruction blocks must prove it understood the task: specific file paths, line numbers, what to change and why. Patterns like "based on your findings, fix the bug" are prohibited -- Auto must include the diagnosis itself.

### Info-Dense Memory (from Claude's Session Memory Instructions)

Memory entries must include specifics: file paths, function names, exact error messages, exact commands. Vague entries like "discovered a pattern in the auth module" are prohibited.

## How It Works

### Prompt Engineering Pipeline

The core idea: the Brain (GLM-5) crafts precise instructions so the Muscle (Kimi K2.5) gets it right on the first try, and Operations (MiniMax M2.5) handles the mechanical work cheaply.

1. User sends a message (e.g., "add auth to the API")
2. Auto (Kimi K2.5) reads relevant source files to understand context
3. Auto engineers a structured `<executor-instructions>` block with exact file paths, steps, code examples, validation commands, and guardrails
4. Auto forwards the block to @executor (Kimi K2.5 Muscle), which follows it precisely
5. Auto calls @verifier (MiniMax M2.5 Operations) to validate the result

For complex tasks, Auto calls @planner (GLM-5 Brain) first for deep breakdown, or @architect (GLM-5 Brain) for design work before implementation.

### Instruction Blocks

The planner and auto orchestrator produce XML-tagged instruction blocks:

- `<architect-instructions>` -- design tasks (sent to @architect, GLM-5 Brain)
- `<executor-instructions>` -- implementation tasks (sent to @executor, Kimi K2.5 Muscle)
- `<debugger-instructions>` -- bug fixes (sent to @debugger, Kimi K2.5 Muscle)
- `<auditor-instructions>` -- code reviews (sent to @auditor, GLM-5 Brain)
- `<verifier-instructions>` -- validation tasks (sent to @verifier, MiniMax M2.5 Operations)
- `<test-instructions>` -- test writing (sent to @executor after @tester designs strategy)
- `<test-strategy>` -- test strategy design (produced by @tester, GLM-5 Brain)
- `<tester-instructions>` -- test strategy requests (sent to @tester, GLM-5 Brain)
- `<needs-investigation>` -- unknown context (routed to @mapper or @reasoner, then retried)
- `<discussion-topic>` -- ambiguous decisions (triggers Discussion Protocol debate)
- `<autoresearch-instructions>` -- autonomous ML experiments (sent to @autoresearch, Kimi K2.5 Muscle)

### Discussion Protocol (Subagent Debates)

**Every task that results in code changes goes through the Discussion Protocol first.** Two GLM-5 Brain agents debate the approach from opposing perspectives, a MiniMax M2.5 Operations agent aggregates the context, and Auto (Kimi K2.5) makes the final decision. This catches bad approaches before they waste implementation tokens.

```
 Any task requiring code changes
    |
    v
 @advocate (GLM-5 Brain)
    |  Examines code, evaluates options, champions best approach
    |  Uses mandatory <thinking> block with solution space exploration
    |  Produces: <proposal> with evidence-cited arguments, trade-offs, implementation sketch
    |
    v
 @critic (GLM-5 Brain)
    |  Independently verifies claims, stress-tests the proposal
    |  Uses mandatory <thinking> block with claim verification chain
    |  Finds weaknesses with file:line evidence, checks dismissed alternatives
    |  Produces: <critique> with verdict (STRONG_SUPPORT / CONDITIONAL_SUPPORT / MAJOR_CONCERNS / OPPOSE)
    |
    v
 @synthesizer (MiniMax M2.5 Operations)
    |  Collects both sides' arguments, formats into structured summary
    |  Produces: <aggregated-context> with points of agreement, disagreement, and unresolved questions
    |
    v
 Auto (Kimi K2.5 Orchestrator)
    |  Weighs the aggregated arguments, applies project context and user intent
    |  Makes the final decision, merges into instruction blocks
    |  Routes to @architect / @executor for implementation
```

**What skips Discussion (read-only operations):**
- Simple questions about code
- Codebase exploration (@mapper)
- Code review (@auditor)
- Git operations (@git-manager)
- Documentation (@docs-manager)

**Cost:** 2 GLM-5 Brain calls + 1 cheap MiniMax Operations call per discussion. The cost of debating is small compared to the cost of implementing the wrong approach and redoing it.

### Test Strategy Pipeline

For test-related tasks, the system uses a three-stage pipeline:

```
 @tester (GLM-5 Brain) -- designs the test strategy
    |  Produces: <test-strategy> with risk assessment, test specifications,
    |  edge cases, adversarial scenarios, coverage gaps, execution order
    |
    v
 @executor (Kimi K2.5 Muscle) -- writes the test code
    |  Receives: <test-instructions> derived from the test strategy
    |  Implements the tests following the specifications
    |
    v
 @verifier (MiniMax M2.5 Operations) -- runs and validates
    |  Mechanically runs tests, compares output vs expected
    |  Reports PASS/FAIL with command evidence
```

### Autonomous ML Research (@autoresearch)

The `@autoresearch` agent integrates [miolini/autoresearch-macos](https://github.com/miolini/autoresearch-macos) -- a macOS/Apple Silicon fork of [karpathy/autoresearch](https://github.com/karpathy/autoresearch) -- into the multi-agent system. It runs autonomous LLM training experiment loops on your Mac using MPS (Metal Performance Shaders).

#### What It Does

The agent edits `train.py`, runs 5-minute training experiments, evaluates `val_bpb` (validation bits-per-byte, lower is better), keeps improvements, discards regressions, and loops indefinitely until you stop it. It follows a strict scientific method: hypothesis, implement, run, evaluate, learn, repeat.

#### macOS/Apple Silicon Adaptations

The miolini fork makes several changes from the original CUDA-only repo that the agent is aware of:

| Aspect | Original (karpathy) | macOS Fork (miolini) |
|--------|---------------------|----------------------|
| **Device** | NVIDIA GPU (CUDA) | Apple Silicon (MPS) |
| **Attention** | FlashAttention-3 | PyTorch SDPA + manual sliding window |
| **torch.compile** | Enabled | Disabled (unstable on MPS) |
| **Autocast** | bf16 on CUDA | nullcontext on MPS |
| **DEPTH** | 8 | 4 |
| **DEVICE_BATCH_SIZE** | 128 | 16 |
| **TOTAL_BATCH_SIZE** | 2^19 | 2^16 |
| **WINDOW_PATTERN** | "SSSL" | "L" |
| **Memory tracking** | peak_vram_mb | Reports 0.0 (no MPS equivalent) |

#### How to Use It

**Prerequisites:**

1. Clone the autoresearch-macos repo:
   ```bash
   git clone https://github.com/miolini/autoresearch-macos.git
   cd autoresearch-macos
   ```

2. Prepare the data (one-time setup):
   ```bash
   uv run prepare.py
   ```
   This downloads data shards to `~/.cache/autoresearch/`.

**Running experiments:**

Tell Auto (or invoke `@autoresearch` directly) to start experiments:

```
> Run autoresearch experiments on ~/autoresearch-macos
```

Or with specific constraints:

```
> Run autoresearch focusing on architecture changes, target val_bpb below 0.99
```

Auto will engineer an `<autoresearch-instructions>` block and delegate to `@autoresearch`, which will:

1. Create a git branch (`autoresearch/<date-tag>`)
2. Run a baseline experiment (unmodified `train.py`)
3. Begin the autonomous experiment loop -- hypothesize, implement, run, evaluate, keep/discard, repeat
4. Log every result to `results.tsv` (tab-separated: commit, val_bpb, memory_gb, status, description)

**The agent runs indefinitely.** It will not pause to ask for confirmation. Interrupt manually (Ctrl+C) when you want it to stop.

**Resuming:**

```
> Resume autoresearch from where we left off
```

The agent reads `results.tsv` and the git history to pick up where it stopped.

#### Scope Rules

- **Editable:** `train.py` only -- model architecture, optimizer, hyperparameters, training loop
- **Read-only:** `prepare.py` -- contains the fixed evaluation harness, data loader, tokenizer
- **No new dependencies** -- only packages already in `pyproject.toml`

#### MPS-Specific Constraints

- `peak_vram_mb` always reports `0.0` on MPS. The `memory_gb` column in results.tsv will always be `0.0`.
- OOM on MPS may manifest as hangs or corrupted tensors rather than clean error messages. The agent reduces `DEVICE_BATCH_SIZE` as its first response to memory issues.
- Architecture experiments must use SDPA-compatible attention patterns (no FlashAttention-3 operations).

### Persistent Memory System

Agents accumulate knowledge across sessions via markdown files in the `memory/` directory:

```
memory/
  project.md               -- Project-level memory (tech stack, conventions, decisions, pitfalls)
  scratchpad-executor.md   -- Executor's patterns, past mistakes, codebase knowledge
  scratchpad-architect.md  -- Architect's design patterns, past decisions
  scratchpad-debugger.md   -- Debugger's bug patterns, past investigations, fragile areas
  scratchpad-auditor.md    -- Auditor's risk areas, recurring issues, standards
```

**How it works:**
1. Auto reads `memory/project.md` at the start of every conversation
2. Before routing to an agent, Auto reads the agent's scratchpad and injects relevant entries into the instruction CONTEXT
3. After task completion, agents write new discoveries to their scratchpad (info-dense: file paths, function names, exact commands)
4. Over time, the system builds a knowledge base that prevents repeated mistakes and enforces learned conventions

### Self-Correction Loop

When post-implementation validation detects a regression:

```
Post-validation fails (build error, test regression, lint failure)
    |
    v
Auto diagnoses the specific failure from error output
    |
    v
Auto re-engineers instructions with the error context and fix strategy
    |
    v
Same agent retries with targeted fix instructions
    |
    v
Post-validation runs again
    |
    +--> Fixed -> Report success with self-correction note
    +--> Still failing -> Escalate to user, suggest auto-fallback
```

One retry only. If self-correction fails, the user is informed with full context.

### Pre/Post Validation Hooks

Before any code change, Auto captures a validation baseline (build status, test results, lint output). After the change, the same commands run again and results are diffed:

- **CLEAN** -- nothing broke
- **IMPROVED** -- previously failing tests now pass
- **REGRESSION** -- something that worked before now fails (triggers self-correction)
- **NEW_FAILURE** -- new failures introduced (triggers self-correction)

Validation commands are auto-discovered on first use and stored in `memory/project.md` for future sessions.

### Fallback

If Go model agents fail, press Tab to switch to **auto-fallback** which uses Claude Opus 4.6 via GitHub Copilot credits. Unlike Auto, the fallback handles work directly instead of delegating.

## Installation

### Option 1: Copy to your OpenCode config directory

```bash
# Clone the repo
git clone https://github.com/albertorblan06/opencode-go-agents.git

# Copy to your project or OpenCode config
cp opencode.json /path/to/your/project/opencode.json
cp -r agents/ /path/to/your/project/agents/
cp -r memory/ /path/to/your/project/memory/
```

### Option 2: Use from `~/.config/opencode/`

```bash
git clone https://github.com/albertorblan06/opencode-go-agents.git
cp -r opencode-go-agents/agents/ ~/.config/opencode/agents/
cp -r opencode-go-agents/memory/ ~/.config/opencode/memory/
```

Then update the `opencode.json` prompt paths from `{file:./agents/...}` to `{file:~/.config/opencode/agents/...}`.

## Requirements

- [OpenCode](https://opencode.ai) installed
- [OpenCode Go subscription](https://opencode.ai/pricing) ($10/month) for GLM-5, Kimi K2.5, and MiniMax M2.5
- (Optional) GitHub Copilot access for the Claude Opus 4.6 fallback agent

## Customization

### Adding your own provider

Edit `opencode.json` to add providers (e.g., Ollama for local models):

```json
{
  "provider": {
    "ollama": {
      "models": {
        "your-model:tag": {
          "_launch": true,
          "name": "your-model:tag"
        }
      },
      "name": "Ollama (local)",
      "npm": "@ai-sdk/openai-compatible",
      "options": {
        "baseURL": "http://127.0.0.1:11434/v1"
      }
    }
  }
}
```

### Changing agent models

Swap any agent's model in `opencode.json`. For example, to run the executor on a local model:

```json
"executor": {
  "model": "ollama/your-model:tag",
  ...
}
```

### Modifying agent behavior

Each agent's prompt is in `agents/<name>.md`. Edit the markdown to change behavior, add guardrails, or adjust the instruction templates.

## License

MIT
