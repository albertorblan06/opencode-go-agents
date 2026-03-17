# OpenCode Go Agents

A multi-agent system for [OpenCode](https://opencode.ai) that closes the gap with Claude Opus-level reasoning at a fraction of the cost. Uses OpenCode Go models (GLM-5, Kimi K2.5, MiniMax M2.5) with prompt-engineered agent orchestration, structured debates, chain-of-thought enforcement, and adversarial verification -- techniques derived from studying Claude Code's own system prompts.

**$10/month subscription vs. $90/month for Claude Code, with comparable reasoning depth on coding tasks.**

## Why This Exists

Claude Code's $90/month subscription gives you Claude Opus 4.6 ($5/$25 per million input/output tokens on the API) with tight tool integration. It produces excellent results because Anthropic's system prompts enforce structured reasoning, adversarial verification, evidence-based analysis, and engineering discipline.

This project replicates those techniques on top of much cheaper models:

| Model | Input $/MTok | Output $/MTok | Role in this system |
|-------|-------------|--------------|---------------------|
| MiniMax M2.5 | $0.25 | $1.20 | Docs, git ops (routine tasks) |
| Kimi K2.5 | $0.45 | $2.20 | Orchestration, reasoning, planning, synthesis |
| GLM-5 | $0.72 | $2.30 | Code generation, debugging, architecture |
| Claude Sonnet 4.6 | $3.00 | $15.00 | (reference -- Claude Code inner model) |
| Claude Opus 4.6 | $5.00 | $25.00 | Fallback only, via GitHub Copilot credits |

**Per-token, GLM-5 is 7x cheaper than Opus on input and 11x cheaper on output.** Kimi K2.5 is 11x and 11x cheaper respectively. MiniMax M2.5 is 20x and 21x cheaper.

### Cost Comparison

With the OpenCode Go subscription at **$10/month**, you get access to all three Go models with generous rate limits. Equivalent API costs for the same token volume would be approximately **$50-70/month** at pay-per-token rates.

Claude Code at **$90/month** provides direct Opus 4.6 access. Equivalent API costs for the same usage would be **$150-250/month** depending on volume.

| | OpenCode Go Agents | Claude Code |
|---|---|---|
| **Monthly cost** | $10 | $90 |
| **Equivalent API cost** | ~$60 | ~$200 |
| **Cost ratio** | **1x** | **~3-4x** |
| **Primary model cost (per MTok out)** | $2.30 (GLM-5) | $25.00 (Opus 4.6) |
| **Reasoning quality** | Engineered via multi-agent debate + CoT | Native (single model) |
| **Fallback** | Claude Opus 4.6 via Copilot | N/A (already Opus) |

The key insight: **a $2.30/MTok model with structured reasoning, adversarial debate, and evidence-first enforcement produces results competitive with a $25/MTok model running as a single agent.** The orchestration overhead (extra Kimi calls for planning, debate synthesis, verification) adds ~30-40% to token usage, but at Kimi's $2.20/MTok output rate, this overhead is negligible compared to the 11x per-token savings.

### How We Close the Gap

Raw model capability is only part of the equation. Claude Code's advantage comes significantly from its system-level engineering:

1. **Structured reasoning enforcement** -- Claude Code's prompts force chain-of-thought, evidence citation, and multi-step analysis. We replicate this with mandatory `<thinking>` blocks on every reasoning agent.
2. **Adversarial verification** -- Claude Code uses a Verification Specialist pattern that tries to break implementations rather than confirm them. We replicate this in our verifier agent.
3. **Engineering discipline** -- Claude Code's prompts enforce scope control, minimal changes, no over-engineering. We replicate this in executor, debugger, and architect agents.
4. **Multi-perspective analysis** -- Claude Code achieves this through a single powerful model reasoning from multiple angles. We achieve it through literal debate between two GLM-5 agents with opposing objectives.

Where Claude Code still wins: novel problems requiring broad world knowledge, extremely long context windows, and tasks where raw model intelligence matters more than process. For standard software engineering (implementing features, fixing bugs, refactoring, code review), the gap is small.

## Benchmarks

### Visual Comparison

| Claude Code (Opus 4.6) | OpenCode Go Agents |
|:----------------------:|:------------------:|
| ![Claude Result](benchmarks%20results/claude-result.gif) | ![Go Result](benchmarks%20results/go-result.gif) |
| **17,562 tokens** | **31,966 tokens** |
| ~$0.44 | ~$0.09 |

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

Every user message goes through the **Auto** orchestrator (Kimi K2.5), which reads relevant code, prompt-engineers structured XML instruction blocks, and routes to the best agent for the job. GLM-5 only fires when code actually needs to be written.

```
User message
    |
    v
 Auto (Kimi K2.5) -- reads code, reasons, engineers structured prompt
    |
    |-- Simple question? ----------> Answers directly (no GLM-5 cost)
    |-- Need exploration? ----------> @mapper (Kimi K2.5)
    |-- Need deep analysis? --------> @reasoner (Kimi K2.5)
    |-- Need complex planning? -----> @planner (Kimi K2.5)
    |-- Need code work? ------------> @executor / @architect / @debugger (GLM-5)
    |-- Need code review? ----------> @auditor (GLM-5, read-only)
    |-- Need validation? -----------> @verifier (Kimi K2.5, read-only)
    |-- Documentation? -------------> @docs-manager (MiniMax M2.5)
    |-- Git operations? ------------> @git-manager (MiniMax M2.5)
    |-- ML experiments? -----------> @autoresearch (GLM-5, autonomous loop)
    |-- Ambiguous approach? --------> DISCUSSION PROTOCOL (see below)
    |
    v
 Failed? --> Tab to switch --> Auto-Fallback (Claude Opus 4.6, handles directly)
```

## Agents

| Agent | Model | Role | Tool Access |
|-------|-------|------|-------------|
| **auto** | Kimi K2.5 | Prompt-engineering orchestrator | Read-only |
| **auto-fallback** | Claude Opus 4.6 | Direct fallback handler | Full |
| **architect** | GLM-5 | System design, API contracts | Full |
| **executor** | GLM-5 | Code implementation, tests, refactoring | Full |
| **debugger** | GLM-5 | Bug investigation and fixing | Full |
| **auditor** | GLM-5 | Code review, security, performance audit | Read-only |
| **planner** | Kimi K2.5 | Task breakdown, structured instruction output | Read-only, no bash |
| **mapper** | Kimi K2.5 | Codebase exploration, structure mapping | Read-only, no bash |
| **reasoner** | Kimi K2.5 | Deep analysis, trade-off evaluation | Read-only, no bash |
| **verifier** | Kimi K2.5 | Adversarial verification, test validation | Read-only |
| **advocate** | GLM-5 | Proposes best approach in debates | Read-only, no bash |
| **critic** | GLM-5 | Stress-tests proposals, finds weaknesses | Read-only, no bash |
| **synthesizer** | Kimi K2.5 | Final decision-maker in debates | Read-only, no bash |
| **docs-manager** | MiniMax M2.5 | Documentation writing and maintenance | Full |
| **git-manager** | MiniMax M2.5 | Git operations, commits, branches, PRs | Full |
| **autoresearch** | GLM-5 | Autonomous ML experiment loops (Apple Silicon/MPS) | Full |

## System Prompt Engineering

The agent prompts in this system were developed by studying **25 of Claude Code's own system prompts** (from Anthropic's public repository) and extracting the techniques that make Claude Code effective. These techniques are model-agnostic -- they work on any capable code model, not just Claude.

### Chain-of-Thought Enforcement

Every reasoning agent (architect, debugger, advocate, critic, reasoner, planner, synthesizer) has a **mandatory `<thinking>` block** that must appear before any output. Each agent gets a role-specific thinking protocol:

| Agent | Thinking Focus |
|-------|---------------|
| **architect** | Component dependency mapping, extension point analysis, trade-off evaluation |
| **debugger** | Hypothesis chain with confirm/refute cycles, root cause isolation |
| **advocate** | Solution space exploration, criteria alignment scoring |
| **critic** | Independent claim verification, structured weakness search |
| **reasoner** | Multi-step reasoning chains with intermediate conclusions |
| **planner** | Task decomposition, dependency graph, critical path identification |
| **synthesizer** | Disputed claim resolution, weakness mitigation assessment |

The thinking structure follows the GIVEN -> EVIDENCE -> ANALYSIS -> GAPS -> CONCLUSION template, forcing the model through a complete reasoning chain rather than jumping to conclusions.

### Evidence-First Reasoning

All 7 reasoning agents enforce 5 evidence rules:

1. **Read code BEFORE forming conclusions.** No reasoning from assumptions.
2. **Every claim must cite `file:line`.** Vague assertions are prohibited.
3. **Unsupported claims must be marked `[UNVERIFIED]`.** No presenting speculation as fact.
4. **Distinguish observation from inference.** Label each explicitly.
5. **Counter-evidence cannot be ignored.** Must be addressed, not hidden.

This is adapted from Claude Code's internal approach where the model is trained to ground reasoning in concrete code references rather than abstract analysis.

### Adversarial Verification (from Claude's Verification Specialist)

The verifier agent was rebuilt using patterns from Claude Code's Verification Specialist prompt:

- **Anti-rationalization protocol**: Lists the 5 exact excuses agents use to skip verification ("the code looks correct based on my reading" -> "reading is not verification, run it") and requires doing the opposite.
- **Mandatory adversarial probes**: Every verification must include at least one attempt to break the implementation (boundary values, concurrency, idempotency, orphan operations).
- **Strict evidence format**: Every check requires Command run / Output observed / Result. A check without a command is not a PASS -- it is a skip.
- **VERDICT output**: Machine-parseable `VERDICT: PASS/FAIL/PARTIAL` format.

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

The core idea: cheap reasoning (Kimi) crafts precise instructions so expensive code models (GLM-5) get it right on the first try.

1. User sends a message (e.g., "add auth to the API")
2. Auto (Kimi) reads relevant source files to understand context
3. Auto engineers a structured `<executor-instructions>` block with exact file paths, steps, code examples, validation commands, and guardrails
4. Auto forwards the block to @executor (GLM-5), which follows it precisely
5. Auto calls @verifier to validate the result

For complex tasks, Auto calls @planner first for deep breakdown, or @architect for design work before implementation.

### Instruction Blocks

The planner and auto orchestrator produce XML-tagged instruction blocks:

- `<architect-instructions>` -- design tasks (sent to @architect)
- `<executor-instructions>` -- implementation tasks (sent to @executor)
- `<debugger-instructions>` -- bug fixes (sent to @debugger)
- `<auditor-instructions>` -- code reviews (sent to @auditor)
- `<verifier-instructions>` -- validation tasks (sent to @verifier)
- `<test-instructions>` -- test writing (sent to @executor)
- `<needs-investigation>` -- unknown context (routed to @mapper or @reasoner, then retried)
- `<discussion-topic>` -- ambiguous decisions (triggers Discussion Protocol debate)
- `<autoresearch-instructions>` -- autonomous ML experiments (sent to @autoresearch)

### Discussion Protocol (Subagent Debates)

**Every task that results in code changes goes through the Discussion Protocol first.** Two GLM-5 agents debate the approach from opposing perspectives, then a Kimi K2.5 agent synthesizes the final decision. This catches bad approaches before they waste implementation tokens.

```
 Any task requiring code changes
    |
    v
 @advocate (GLM-5)
    |  Examines code, evaluates options, champions best approach
    |  Uses mandatory <thinking> block with solution space exploration
    |  Produces: <proposal> with evidence-cited arguments, trade-offs, implementation sketch
    |
    v
 @critic (GLM-5)
    |  Independently verifies claims, stress-tests the proposal
    |  Uses mandatory <thinking> block with claim verification chain
    |  Finds weaknesses with file:line evidence, checks dismissed alternatives
    |  Produces: <critique> with verdict (STRONG_SUPPORT / CONDITIONAL_SUPPORT / MAJOR_CONCERNS / OPPOSE)
    |
    v
 @synthesizer (Kimi K2.5)
    |  Weighs both sides, resolves disputed claims by reading code independently
    |  Uses mandatory <thinking> block with dispute resolution analysis
    |  Produces: <decision> with final approach, implementation steps, guardrails
    |
    v
 Auto merges <decision> into instruction blocks -> routes to @architect / @executor
```

**What skips Discussion (read-only operations):**
- Simple questions about code
- Codebase exploration (@mapper)
- Code review (@auditor)
- Git operations (@git-manager)
- Documentation (@docs-manager)

**Cost:** 2 GLM-5 calls + 1 cheap Kimi call per discussion. The cost of debating is small compared to the cost of implementing the wrong approach and redoing it.

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
