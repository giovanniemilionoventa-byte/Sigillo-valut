---
tags: [component, sdk]
source: code (packages/sdk-typescript/src)
---
# TypeScript SDK (`@sigillo/sdk`, 0.1.0, private)

Same promises as the [[Python SDK]], **zero dependencies**: its own minimal tracer registered in the OpenTelemetry API slot, its own OTLP exporter.
`init({endpoint, apiKey, systemId, redactContent, saltContent, customerKey, heartbeatSeconds, showSent, strict, onHalt, guardSeconds})`, `artifact()`, `nonceFor()`, `pseudonym()`, `SigilloSpanExporter`, `Tracing.tool(name, fn, agent)`. Has the guard process port; no local-model door. Upload from TypeScript agents is active.
