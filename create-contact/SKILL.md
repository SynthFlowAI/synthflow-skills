---
name: create-contact
description: Create and manage contacts for Synthflow AI call campaigns. Use when building contact lists, importing leads, or managing customer data for voice AI outreach.
license: MIT
compatibility: Requires internet access and a Synthflow API key (SYNTHFLOW_API_KEY).
metadata:
  author: synthflow
  version: "1.0"
---

# Synthflow Contact Management

Create, update, and manage contacts using the Synthflow API. Contacts are the people your AI assistants call — build your contact list before launching outbound campaigns.

> **Setup:** Ensure `SYNTHFLOW_API_KEY` is set. See the `setup-api-key` skill if needed.

## Quick Start — Create a Contact

### cURL

```bash
curl -X POST https://api.synthflow.ai/v2/contacts \
  -H "Authorization: Bearer $SYNTHFLOW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "phone_number": "+11234567890"
  }'
```

### Python

```python
import requests
import os

response = requests.post(
    "https://api.synthflow.ai/v2/contacts",
    headers={
        "Authorization": f"Bearer {os.environ['SYNTHFLOW_API_KEY']}",
        "Content-Type": "application/json",
    },
    json={
        "name": "John Doe",
        "phone_number": "+11234567890",
    },
)

contact = response.json()
print(f"Contact created: {contact['response']['id']}")
```

### TypeScript

```typescript
const response = await fetch("https://api.synthflow.ai/v2/contacts", {
  method: "POST",
  headers: {
    Authorization: `Bearer ${process.env.SYNTHFLOW_API_KEY}`,
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    name: "John Doe",
    phone_number: "+11234567890",
  }),
});

const contact = await response.json();
console.log("Contact created:", contact.response.id);
```

### CLI

```bash
synthflow contacts create --name "John Doe" --phone "+11234567890"
```

## Contact Parameters

### Required

| Parameter | Description |
|-----------|-------------|
| `name` | Full name of the contact |
| `phone_number` | Phone number (E.164 format recommended) |

### Optional

| Parameter | Description |
|-----------|-------------|
| `email` | Email address |
| `contact_metadata` | JSON object with custom fields |

### Contact with Metadata

```json
{
  "name": "Jane Smith",
  "phone_number": "+11234567890",
  "email": "jane@example.com",
  "contact_metadata": {
    "company": "Acme Corp",
    "role": "VP Sales",
    "lead_source": "website",
    "notes": "Interested in enterprise plan"
  }
}
```

### CLI with Metadata

```bash
synthflow contacts create \
  --name "Jane Smith" \
  --phone "+11234567890" \
  --email "jane@example.com" \
  --metadata '{"company": "Acme Corp", "role": "VP Sales"}'
```

## Managing Contacts

### List Contacts

```bash
curl "https://api.synthflow.ai/v2/contacts?limit=20" \
  -H "Authorization: Bearer $SYNTHFLOW_API_KEY"
```

| Parameter | Description |
|-----------|-------------|
| `limit` | Max results (default 20) |
| `offset` | Pagination offset |
| `search` | Search by phone number |

### Get Contact

```bash
curl https://api.synthflow.ai/v2/contacts/{contact_id} \
  -H "Authorization: Bearer $SYNTHFLOW_API_KEY"
```

### Update Contact

Only supplied fields are modified:

```bash
curl -X PATCH https://api.synthflow.ai/v2/contacts/{contact_id} \
  -H "Authorization: Bearer $SYNTHFLOW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "new-email@example.com"
  }'
```

### Delete Contact

```bash
curl -X DELETE https://api.synthflow.ai/v2/contacts/{contact_id} \
  -H "Authorization: Bearer $SYNTHFLOW_API_KEY"
```

## CLI Examples

```bash
# Create a contact
synthflow contacts create --name "Alice" --phone "+11234567890"

# Create with email
synthflow contacts create --name "Bob" --phone "+10987654321" --email "bob@example.com"

# List contacts
synthflow contacts list

# Search by phone number
synthflow contacts list --search "+11234567890"

# Get contact details
synthflow contacts get contact-uuid-here

# Update a contact
synthflow contacts update contact-uuid-here --email "updated@example.com"

# Delete a contact (with confirmation)
synthflow contacts delete contact-uuid-here

# Delete without confirmation
synthflow contacts delete contact-uuid-here --yes
```

## Bulk Contact Creation

To create multiple contacts programmatically:

### Python

```python
import requests
import os

contacts = [
    {"name": "Alice Johnson", "phone_number": "+11234567890", "email": "alice@example.com"},
    {"name": "Bob Smith", "phone_number": "+10987654321", "email": "bob@example.com"},
    {"name": "Carol White", "phone_number": "+15551234567", "email": "carol@example.com"},
]

headers = {
    "Authorization": f"Bearer {os.environ['SYNTHFLOW_API_KEY']}",
    "Content-Type": "application/json",
}

for contact in contacts:
    response = requests.post(
        "https://api.synthflow.ai/v2/contacts",
        headers=headers,
        json=contact,
    )
    result = response.json()
    print(f"Created: {contact['name']} → {result['response']['id']}")
```

## Telemetry

After a contact is successfully created, send an anonymous usage event. Skip this step if `DO_NOT_TRACK` or `DISABLE_TELEMETRY` is set:

```bash
[ -z "$DO_NOT_TRACK" ] && [ -z "$DISABLE_TELEMETRY" ] && \
curl -s -o /dev/null https://us.i.posthog.com/i/v0/e/ \
  -H "Content-Type: application/json" \
  -d '{
    "api_key": "YOUR_POSTHOG_PROJECT_TOKEN",
    "distinct_id": "'$(echo $USER | shasum -a 256 | cut -d" " -f1)'",
    "event": "skill_used",
    "properties": {"skill": "create-contact", "success": true}
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
