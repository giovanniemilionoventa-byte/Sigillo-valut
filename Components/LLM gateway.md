---
tags: [component, gateway]
source: code (apps/server/src/gateway/*)
---
# LLM gateway (`/llm/<provider>/...`)

A proxy in front of model providers: the agent calls Sigillo instead of the provider and **every call is recorded before the answer is returned**. The agent never holds the provider key and cannot skip the recording.

- 18 providers (`catalog.ts`): openai, anthropic, gemini, mistral, groq, openrouter, deepseek, xai, together, fireworks, perplexity, cohere, cerebras, moonshot, qwen, zai, nvidia, sambanova.
- Auth with the Sigillo API key (Bearer, `x-api-key`, `x-goog-api-key` or `?key=`). Provider key stored sealed, see [[Keyholder]].
- Receipt: `llm_call`, name = provider path, `model{name, provider}`, input = request body, output = answer, both salted.
- Non-streaming: no receipt → no answer (503). Streaming: passed through, receipt at the end. POST and GET recorded, except model lists. 502 if provider silent (recorded `unknown`).
- Errors: 404 unknown path, 403 closed path (realtime, keys, organization admin) or gateway off for this account, 409 no provider key, 415 not JSON, 423 vault locked.
- **Tool requests** (`SIGILLO_LLM_TOOL_REQUESTS`): when the model asks for a tool, a `model_requested.<tool>` receipt (outcome unknown) is written; see [[Tool reconciler]].
- Header `x-sigillo-agent` names the agent.

Related: [[Flow - gateway call]].
