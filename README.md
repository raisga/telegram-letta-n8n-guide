# Interact with your Letta agent using n8n and Telegram

Chat with your stateful AI agent with long-term memory in your n8n workflows using a Telegram bot.
This document covers installation, credentials, and workflow implementation.

## Pre-requisites

We are assuming you have basic familiarity with n8n and Telegram bots.

### What you'll need

* **n8n** v1.0.0+ (Cloud or self-hosted).
* **Letta** account (Cloud or self-hosted) and an **API token**.
* A **Letta Agent ID** (e.g., `agent-90009dba-8012-46c5-a0f5-5630cc457363`).
* A **Telegram bot** and its **API token** (from [BotFather](https://t.me/botfather)).

### Install the Letta community node

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

### Create Letta credentials in n8n

1. Open the **Credentials** screen → **New** → choose **Letta API**.
2. Fill in:

   * **API Token**: your Letta token (from the Letta dashboard).
   * **Base URL**: `https://api.letta.com` (or your self-hosted URL).

> Where to get the token: Letta dashboard → API settings → generate token.

## Implement the n8n workflow

You can create a workflow that listens for new Telegram messages and forwards them to your Letta agent, then sends the agent's response back to the user.

### Basic workflow

1. Create a new workflow in n8n.
2. Add a **Telegram Trigger** node:
   * Set **Resource** to `Message`.
   * Set **Operation** to `Get Updates`.
   * Enter your Telegram bot token.
   * Optionally, set **Poll Interval** (default is 3000 ms).
3. Add a **Letta - Send Message to Agent** node:
   * Connect it to the Telegram Trigger node.
   * Set **Credentials** to the Letta credentials you created.
   * Set **Agent ID** to your Letta Agent ID.
   * Set **Message** to `{{$node["Telegram Trigger"].json["message"]["text"]}}`.
4. Save and activate the workflow.

### Advanced workflow with reply indicator and voice message support

See the complete workflow example [Telegram Letta Chat](./workflows/telegram-letta-chat.json).

![n8n workflow example](./n8n_workflow-telegram-letta-chat.jpg)

It uses the following sub-workflows:

- [Telegram Reply Indicators](./workflows/telegram-reply-indicators.json): shows a "typing..." indicator or an audio recoding while the agent processes the message.

![n8n workflow example](./n8n_workflow-telegram-reply-indicators.jpg)

## Resources

[1]: https://github.com/letta-ai/n8n-nodes-letta "GitHub - letta-ai/n8n-nodes-letta: This is the official n8n node that allows you to integrate Letta AI agents into your n8n workflows."
