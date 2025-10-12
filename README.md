# Interact with your Letta agent via n8n

Chat with your stateful AI agent with long-term memory in your n8n workflows using Letta.

This guide covers installation, credentials, an initial workflow, and tips for memory and production hardening.

## Setp 0: Prerequisites

### 1. What you'll need

* **n8n** v1.0.0+ (Cloud or self-hosted).
* **Letta** account (Cloud or self-hosted) and an **API token**.
* A **Letta Agent ID** (e.g., `agent-90009dba-8012-46c5-a0f5-5630cc457363`).

### 2. Install the Letta community node

#### Option A — from the n8n UI (recommended)

1. In n8n, go to **Settings → Community Nodes → Install**.
2. Enter `@letta-ai/n8n-nodes-letta` and confirm.

#### Option B — npm / Docker

* In the n8n root directory:

```bash
npm install @letta-ai/n8n-nodes-letta
```

* If you run n8n with Docker, add to your `.env` **before** `N8N_CUSTOM_EXTENSIONS`, like this:

```
N8N_CUSTOM_EXTENSIONS=@letta-ai/n8n-nodes-letta
```

Then rebuild/restart n8n.

### 3. Create Letta credentials in n8n

1. Open the **Credentials** screen → **New** → choose **Letta API**.
2. Fill in:

   * **API Token**: your Letta token (from the Letta dashboard).
   * **Base URL**: `https://api.letta.com` (or your self-hosted URL).

> Where to get the token: Letta dashboard → API settings → generate token.

## Step 1: Create a simple workflow

...

[1]: https://github.com/letta-ai/n8n-nodes-letta "GitHub - letta-ai/n8n-nodes-letta: This is the official n8n node that allows you to integrate Letta AI agents into your n8n workflows."
[2]: https://docs.letta.com/guides/agents/memory "Agent Memory | Letta"
[3]: https://docs.letta.com/guides/agents/architectures/workflows "Workflows | Letta"
[4]: https://docs.n8n.io/integrations/ "n8n Integrations Documentation and Guides | n8n Docs "
