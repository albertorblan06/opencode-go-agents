# OpenCode Go Agents

A multi-agent system for [OpenCode](https://opencode.ai) that uses OpenCode Go models ($10/month) for most work and falls back to Claude Opus 4.6 via GitHub Copilot only when needed. Designed for the best economy-to-performance ratio.

Features a **Discussion Protocol** where subagents debate among themselves (advocate -> critic -> synthesizer) to find the best approach for ambiguous tasks before implementation begins.

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
| **verifier** | Kimi K2.5 | Requirement verification, test validation | Read-only |
| **advocate** | Kimi K2.5 | Proposes best approach in debates | Read-only, no bash |
| **critic** | Kimi K2.5 | Stress-tests proposals, finds weaknesses | Read-only, no bash |
| **synthesizer** | Kimi K2.5 | Final decision-maker in debates | Read-only, no bash |
| **docs-manager** | MiniMax M2.5 | Documentation writing and maintenance | Full |
| **git-manager** | MiniMax M2.5 | Git operations, commits, branches, PRs | Full |

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

### Discussion Protocol (Subagent Debates)

When a task has multiple viable approaches, the Auto orchestrator triggers a structured debate between three methodology agents. This ensures better decisions through adversarial reasoning.

```
 Auto identifies ambiguous decision
    |
    v
 @advocate (Kimi K2.5)
    |  Examines code, evaluates options, champions best approach
    |  Produces: <proposal> with arguments, trade-offs, implementation sketch
    |
    v
 @critic (Kimi K2.5)
    |  Independently verifies claims, stress-tests the proposal
    |  Finds weaknesses, checks dismissed alternatives
    |  Produces: <critique> with verdict (STRONG_SUPPORT / CONDITIONAL_SUPPORT / MAJOR_CONCERNS / OPPOSE)
    |
    v
 @synthesizer (Kimi K2.5)
    |  Weighs both sides, resolves disputed claims
    |  Produces: <decision> with final approach, implementation steps, guardrails
    |
    v
 Auto merges <decision> into instruction blocks -> routes to @architect / @executor
```

**When it triggers:**
- User asks "should we use X or Y?" or "what's the best approach?"
- Multiple viable approaches exist and the best path is unclear
- A design decision has significant long-term consequences
- Trade-offs between competing priorities (performance vs. readability, etc.)

**When it doesn't trigger:**
- Clear single approach (just do it)
- User already decided the approach
- Time-sensitive bug fixes
- Simple implementation tasks

The whole debate runs on Kimi K2.5 (cheap thinking tokens), so the cost of better decisions is minimal compared to the cost of implementing the wrong approach on GLM-5.

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
```

### Option 2: Use from `~/.config/opencode/`

```bash
git clone https://github.com/albertorblan06/opencode-go-agents.git
cp -r opencode-go-agents/agents/ ~/.config/opencode/agents/
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
