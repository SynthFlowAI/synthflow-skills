# Synthflow Skills

Agent skills for [Synthflow](https://synthflow.ai) — the platform for building AI voice agents. These skills follow the [Agent Skills specification](https://agentskills.io/specification) and can be used with any compatible AI coding assistant including Claude Code, Cursor, VS Code Copilot, Gemini CLI, and more.

## Installation

### Option 1: npx skills (works with all agents)

```bash
npx skills add SynthFlowAI/synthflow-skills
```

Install specific skills:

```bash
npx skills add SynthFlowAI/synthflow-skills --skill create-assistant
npx skills add SynthFlowAI/synthflow-skills --skill create-call
```

Install for a specific agent:

```bash
npx skills add SynthFlowAI/synthflow-skills -a claude-code
npx skills add SynthFlowAI/synthflow-skills -a cursor
```

### Option 2: Claude Code Plugin (native integration)

```
/plugin marketplace add SynthFlowAI/synthflow-skills
/plugin install synthflow-voice-ai@synthflow-skills
```

### Option 3: Manual installation

Copy any skill directory into your project's `.claude/skills/` (for Claude Code), `.cursor/skills/` (for Cursor), or the equivalent skills directory for your agent.

### Supported agents

| Agent | Config File | Auto-detected |
|-------|------------|---------------|
| Claude Code | `.mcp.json` | Yes |
| Cursor | `.cursor/mcp.json` | Yes |
| VS Code Copilot | `.vscode/mcp.json` | Yes |

**Requires:** Node.js (for `npx`). Uses [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) to bridge the remote server. No API key needed for the docs server.

### Manual setup

If your agent doesn't auto-detect MCP configs:

**Claude Code:**
```bash
claude mcp add synthflow-docs -- npx -y mcp-remote https://docs.synthflow.ai/_mcp/server
```

**Any agent (JSON config):**
```json
{
  "mcpServers": {
    "synthflow-docs": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://docs.synthflow.ai/_mcp/server"]
    }
  }
}
```

## Available Skills

| Skill | Description |
|-------|-------------|
| [setup-api-key](./setup-api-key) | Guide through obtaining and configuring a Synthflow API key |
| [create-assistant](./create-assistant) | Create AI voice assistants with LLMs, voices, prompts, and advanced settings |
| [create-call](./create-call) | Initiate outbound phone calls through AI agents |
| [manage-actions](./manage-actions) | Create, update, and attach actions — transfers, SMS, webhooks, booking, extraction, evals |
| [create-simulation](./create-simulation) | Create and run simulation test suites to evaluate agent behavior |
| [create-eval](./create-eval) | Define custom evaluations to score call quality and outcomes |

## Configuration

All skills require a Synthflow API key. Set it as an environment variable:

```bash
export SYNTHFLOW_API_KEY="your-api-key"
```

Get your API key from the [Synthflow Dashboard](https://app.synthflow.ai) or use the `setup-api-key` skill.


## Quick Start

1. **Get an API key**: Use the `setup-api-key` skill or visit https://app.synthflow.ai
2. **Create an assistant**: Use the `create-assistant` skill to build a voice AI agent
3. **Add actions**: Use `manage-actions` to add transfers, SMS, webhooks, and more
4. **Make a call**: Use `create-call` to test your assistant


## API Reference

- **Base URL**: `https://api.synthflow.ai/v2`
- **Authentication**: Bearer token via `Authorization: Bearer $SYNTHFLOW_API_KEY`
- **Full API Docs**: https://docs.synthflow.ai
- **MCP Docs Server**: `https://docs.synthflow.ai/_mcp/server` (auto-configured via `.mcp.json`)

## Telemetry

These skills collect anonymous usage data via [PostHog](https://posthog.com) to help us improve. We track:

- **README views** — a tracking pixel fires when the README is viewed on GitHub
- **Skill usage** — a lightweight event is sent when an agent completes a skill workflow

**What's collected:** event name, skill name, success/failure, and a hashed user identifier (SHA-256 of `$USER`). No personal information is sent.

**Opt out:** Set either environment variable to disable all telemetry:

```bash
export DO_NOT_TRACK=1
# or
export DISABLE_TELEMETRY=1
```


## License

MIT

![](https://eu.i.posthog.com/e?ip=1&_host=eu.i.posthog.com&token=phc_dlyyp4oL77penk6jXtJRUUpotT7eiUk3wRSY1KzzpLi&event=$pageview&properties=%7B%22%24current_url%22%3A%22https%3A%2F%2Fgithub.com%2FSynthFlowAI%2Fsynthflow-skills%22%7D)
