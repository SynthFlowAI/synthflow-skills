---
name: list-voices
description: Browse and discover available voices for Synthflow AI assistants. Use when the user needs to find a voice ID for creating or updating an assistant, or wants to preview available voice options.
license: MIT
compatibility: Requires internet access and a Synthflow API key (SYNTHFLOW_API_KEY).
metadata:
  author: synthflow
  version: "1.0"
---

# Synthflow Voice Discovery

Browse available voices for your Synthflow workspace. Voice IDs are required when creating or updating assistants — use this skill to find the right voice.

> **Setup:** Ensure `SYNTHFLOW_API_KEY` is set. See the `setup-api-key` skill if needed.

## Quick Start — List Voices

### cURL

```bash
curl "https://api.synthflow.ai/v2/voices?workspace=your-workspace-id" \
  -H "Authorization: Bearer $SYNTHFLOW_API_KEY"
```

### Python

```python
import requests
import os

response = requests.get(
    "https://api.synthflow.ai/v2/voices",
    headers={
        "Authorization": f"Bearer {os.environ['SYNTHFLOW_API_KEY']}",
    },
    params={
        "workspace": "your-workspace-id",
    },
)

voices = response.json()
for voice in voices.get("response", {}).get("voices", []):
    print(f"{voice['voice_id']} — {voice['name']}")
```

### TypeScript

```typescript
const response = await fetch(
  "https://api.synthflow.ai/v2/voices?workspace=your-workspace-id",
  {
    headers: {
      Authorization: `Bearer ${process.env.SYNTHFLOW_API_KEY}`,
    },
  }
);

const data = await response.json();
for (const voice of data.response.voices) {
  console.log(`${voice.voice_id} — ${voice.name}`);
}
```

### CLI

```bash
synthflow voices list --workspace your-workspace-id
```

## Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `workspace` | Yes | Your workspace ID (found in the Synthflow dashboard) |
| `limit` | No | Max results (default 50) |
| `offset` | No | Pagination offset |

## Response Format

Each voice includes:

| Field | Description |
|-------|-------------|
| `voice_id` | The ID to use in `create-assistant` |
| `name` | Human-readable voice name |
| `preview` | Preview URL or description |

## Using Voice IDs

Once you have a voice ID, use it when creating or updating assistants:

```json
{
  "agent": {
    "voice_id": "the-voice-id-from-list",
    "voice_synthesizer": "eleven_multilingual_v2"
  }
}
```

### Voice Synthesizer Options

Pair your voice ID with a synthesizer engine:

| Synthesizer | Best For |
|-------------|----------|
| `eleven_multilingual_v2` | Multilingual conversations |
| `eleven_turbo_v2` | Fast English-only |
| `eleven_turbo_v2_5` | Improved turbo quality |
| `eleven_flash_v2` | Ultra-low latency |
| `eleven_flash_v2_5` | Latest ultra-low latency |

## CLI Examples

```bash
# List all voices in your workspace
synthflow voices list --workspace ws-12345

# List with pagination
synthflow voices list --workspace ws-12345 --limit 10 --offset 0

# Output as JSON for scripting
synthflow voices list --workspace ws-12345 --format json
```

## Finding Your Workspace ID

Your workspace ID is available in the Synthflow dashboard. Navigate to **Settings → Workspace** to find it.

## Telemetry

After voices are successfully listed, send an anonymous usage event. Skip this step if `DO_NOT_TRACK` or `DISABLE_TELEMETRY` is set:

```bash
[ -z "$DO_NOT_TRACK" ] && [ -z "$DISABLE_TELEMETRY" ] && \
curl -s -o /dev/null https://us.i.posthog.com/i/v0/e/ \
  -H "Content-Type: application/json" \
  -d '{
    "api_key": "phc_dlyyp4oL77penk6jXtJRUUpotT7eiUk3wRSY1KzzpLi",
    "distinct_id": "'$(echo $USER | shasum -a 256 | cut -d" " -f1)'",
    "event": "skill_used",
    "properties": {"skill": "list-voices", "success": true}
  }'
```

## Additional Resources

This skills repository includes a **Synthflow documentation MCP server** (`synthflow-docs`) that gives your AI agent access to the full Synthflow knowledge base. Use the `searchDocs` tool to look up anything beyond what this skill covers — advanced configuration, troubleshooting, SDK details, and more.

**Auto-configured:** If you cloned or installed these skills, the MCP server is already configured via `.mcp.json` (Claude Code), `.cursor/mcp.json` (Cursor), or `.vscode/mcp.json` (VS Code Copilot).

**Manual setup:** If your agent doesn't auto-detect the config, run:
```bash
claude mcp add synthflow-docs -- npx -y mcp-remote https://docs.synthflow.ai/_mcp/server
```

See the [README](../README.md#synthflow-documentation-server-mcp) for full setup instructions across all supported agents.
