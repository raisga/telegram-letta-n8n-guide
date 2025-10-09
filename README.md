# Integrating Letta with n8n — Quickstart Guide

Build stateful AI agents with long-term memory in your n8n workflows using Letta. This guide covers installation, credentials, a first workflow, and tips for memory and production hardening.

---

## What you’ll need

* **n8n** v1.0.0+ (Cloud or self-hosted). ([GitHub][1])
* **Letta** account (Cloud or self-hosted) and an **API token**. ([GitHub][1])
* A **Letta Agent ID** (e.g., `agent_abc123`). ([GitHub][1])

---

## Install the Letta community node

### Option A — from the n8n UI (recommended)

1. In n8n, go to **Settings → Community Nodes → Install**.
2. Enter `@letta-ai/n8n-nodes-letta` and confirm. ([GitHub][1])

### Option B — npm / Docker

* In the n8n root directory:

```bash
npm install @letta-ai/n8n-nodes-letta
```

* If you run n8n with Docker, add to your `.env` **before** `N8N_CUSTOM_EXTENSIONS`:

```
N8N_CUSTOM_EXTENSIONS=@letta-ai/n8n-nodes-letta
```

Then rebuild/restart n8n. ([GitHub][1])

---

## Create Letta credentials in n8n

1. Open the **Credentials** screen → **New** → choose **Letta API**.
2. Fill in:

   * **API Token**: your Letta token (from the Letta dashboard).
   * **Base URL**: `https://api.letta.com` (or your self-hosted URL). ([GitHub][1])

> Where to get the token: Letta dashboard → API settings → generate token. ([GitHub][1])

---

## Your first workflow (Manual trigger → Letta → Respond)

**Nodes**

1. **Manual Trigger**
2. **Letta (Send Message)**

   * **Credentials**: the Letta API credentials you created
   * **Agent ID**: `agent_abc123` (replace with yours)
   * **Role**: `user`
   * **Message**: `Hello! Who are you?`
   * *(Optional)* **Max Steps**, **Enable Thinking**, **Return Message Types**
3. **Respond / Display** (use Set/Function/Webhook reply, etc.)

The Letta node hits the Messages API and returns the full agent response, including `messages`, `stop_reason`, and `usage`. ([GitHub][1])

**Shape**

```
Manual Trigger → Letta → (Display/Return)
```

You can also import the “Simple Chat” demo workflow from the node repo’s `demo/` folder to see a working example. ([GitHub][1])

---

## (Alternative) Use HTTP Request node instead of the community node

If you can’t install community nodes, call Letta directly:

* **Method:** `POST`
* **URL:** `https://api.letta.com/v1/agents/{{ $json.agentId }}/messages`
* **Auth:** Bearer token (= Letta API token)
* **Body (JSON):**

  ```json
  {
    "role": "user",
    "content": "Hello! Who are you?",
    "max_steps": 10
  }
  ```

This mirrors what the Letta node does under the hood. ([GitHub][1])

---

## How Letta memory works (what to expect)

Letta gives your agent **two tiers of memory**:

* **Core (in-context) memory blocks** — persistent, structured context that’s always visible in the agent’s window (e.g., persona, user prefs).
* **External (out-of-context) memory** — unlimited stores (conversation search, archival memory, filesystem, or your own DB/vector store via tools) that the agent retrieves on demand. ([docs.letta.com][2])

Agents actively **manage** their memory (e.g., `memory_insert`, `memory_replace`, `memory_rethink`) rather than just passively retrieving docs like traditional RAG. Best practice: keep an “executive summary” in core memory and the detailed history in external stores. ([docs.letta.com][2])

> If you already have an n8n flow you want to keep deterministic, Letta also supports a **workflow agent** pattern with tool rules and branching. Useful when migrating a known sequence from n8n into Letta, or combining both. ([docs.letta.com][3])

---

## Tips & patterns

* **Identity / multi-user**: Pass a stable user identifier so your agent can maintain per-user memory. (See Letta docs on multi-user and memory.) ([docs.letta.com][2])
* **Guardrails vs autonomy**: Start constrained (lower **Max Steps**, fewer tools). Add tools and steps as you gain confidence. ([GitHub][1])
* **Long-running & summaries**: Schedule daily/weekly summaries (“Scheduled Summary” demo in the repo) to keep core memory concise. ([GitHub][1])
* **Observability**: Log `stop_reason` and `usage` from the node output for debugging and cost tracking. ([GitHub][1])

---

## Troubleshooting

* **401/403**: Check the Letta token and Base URL (Cloud vs self-hosted). ([GitHub][1])
* **404 Agent**: Verify the **Agent ID** exists in your Letta project. ([GitHub][1])
* **Community node blocked**: Use the **HTTP Request** fallback shown above. ([n8n Docs][4])
* **Unexpected/loopy behavior**: Reduce **Max Steps**, disable “Enable Thinking”, or add tool rules (if using workflow agents). ([GitHub][1])

---

## References

* Letta n8n node (install, params, demos, API details). ([GitHub][1])
* Letta memory concepts (core vs external, memory tools). ([docs.letta.com][2])
* Letta workflow agents & tool rules (deterministic sequences). ([docs.letta.com][3])
* n8n docs — installing community nodes / generic HTTP calls. ([n8n Docs][4])

---

### Appendix: Minimal JSON you can send to Letta

```json
{
  "role": "user",
  "content": "Summarize my latest tasks and suggest next steps.",
  "max_steps": 6
}
```

Pair that with your Agent ID in the URL path and your Bearer token in the header to test quickly with the HTTP Request node. ([GitHub][1])

---

## Resources

[1]: https://github.com/letta-ai/n8n-nodes-letta "GitHub - letta-ai/n8n-nodes-letta: This is the official n8n node that allows you to integrate Letta AI agents into your n8n workflows."
[2]: https://docs.letta.com/guides/agents/memory "Agent Memory | Letta"
[3]: https://docs.letta.com/guides/agents/architectures/workflows "Workflows | Letta"
[4]: https://docs.n8n.io/integrations/ "n8n Integrations Documentation and Guides | n8n Docs "
