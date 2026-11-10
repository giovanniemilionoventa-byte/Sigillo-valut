---
tags: [component, completeness]
source: code (apps/server/src/check/tool-reconcile.ts)
---
# Tool reconciler

If the [[LLM gateway]] saw the model ask for a tool, and the agent (which records its own tool calls through the SDK) records no matching `tool_call` within **10 minutes**, the server writes `sigillo.check.tool_unrecorded.<tool>` with outcome `error`. Sweep every 60 s. It catches an agent that runs tools without recording them.
