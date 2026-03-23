# Synthflow Skills

This repository contains agent skills for building voice AI with Synthflow.

## MCP Documentation Server

This project includes a Synthflow documentation MCP server (`synthflow-docs`). Use the `searchDocs` tool to look up Synthflow documentation when:

- A user asks about Synthflow features not fully covered by these skills
- You need to verify API parameters, provider options, or current behavior
- You encounter errors or need troubleshooting guidance
- The user asks about SDKs, platform capabilities, or advanced configuration

The skills cover common workflows. The MCP docs server covers everything else.

## Available Skills

- `setup-api-key` — API key configuration
- `create-assistant` — Voice assistant creation
- `create-call` — Outbound call initiation
- `create-contact` — Contact management
- `list-voices` — Voice discovery
- `create-simulation` — Simulation test suites
- `create-eval` — Custom evaluations

## Configuration

All API calls require `SYNTHFLOW_API_KEY` environment variable. Base URL: `https://api.synthflow.ai/v2`

## CLI

Synthflow also has a Python CLI (`synthflow`) that wraps these API endpoints. Install with `pip install synthflow` and use commands like `synthflow calls make`, `synthflow assistants create`, etc.
