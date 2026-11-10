# SDK, gateway, guardiano, modelli locali

[CODICE] salvo diversa etichetta.

## Tre ingressi

SDK (Python o TypeScript), `POST /v1/traces` (OTLP), `POST /api/v1/receipts`. La console propone solo l'SDK perché è l'unico con il battito.

## SDK Python 0.12.0

`sigillo.init(endpoint, api_key, system_id, ...)`. Strumenta LangChain, CrewAI, OpenAI e la libreria standard (urllib, requests, httpx, scritture di file). Invio in secondo piano. Il contenuto non parte mai in chiaro: solo impronte. **Non è su PyPI**: si installa dallo zip o dal pacchetto del server. SDK TypeScript con le stesse promesse, senza modelli locali né strumentazione stdlib.

## "Carica il tuo agente" (console)

Avviene tutto nel browser: il file non va al server. Inserisce `sigillo.init(...)` e fa riscaricare il file. Python e Node/TS attivi; Java e .NET sospesi ("coming soon").

## Battito e guardiano

L'SDK manda un battito ogni 60 secondi. Un guardiano (processo separato) segnala se l'agente finisce, il computer dorme, va offline o riparte. Il server scrive ricevute riservate `sigillo.connection.*` (start, stop, lost dopo 3 minuti, ended, sleep, offline, power_off, restored, script_changed). Modalità `strict`: se cambiano lo script, l'SDK o le librerie OpenTelemetry l'agente viene fermato (codice 70). I prefissi `sigillo.` e `model_requested.` sono riservati al server.

## Gateway dei modelli ("More security")

L'agente chiama il modello passando da Sigillo: `/llm/<fornitore>/<percorso>`. **18 fornitori**: openai, anthropic, gemini, mistral, groq, openrouter, deepseek, xai, together, fireworks, perplexity, cohere, cerebras, moonshot, qwen, zai, nvidia, sambanova. Esclusi Azure OpenAI, Bedrock, Vertex, Hugging Face.

- Ogni chiamata produce una ricevuta `llm_call`; la risposta torna **dopo** la scrittura (503 se non si scrive); in streaming si registra alla fine.
- Percorsi che emettono chiavi temporanee sono chiusi (403).
- Quando il modello chiede uno strumento il server scrive `model_requested.<strumento>`. Ogni 60 s un controllo confronta le richieste con gli strumenti registrati dall'agente e dopo 10 minuti scrive l'allarme `sigillo.check.tool_unrecorded.<strumento>`.
- **Il traffico attraversa la memoria del server in chiaro** (non su disco né nei log). È l'unico punto in cui Sigillo "vede" il contenuto.
- Le chiavi dei modelli dei clienti sono cifrate (AES-256-GCM), ma il server deve poterle aprire: **non promettere "invisibili al server"**.
- Latenza aggiunta circa 5-6 ms per chiamata [DOCUMENTI].

## Porta per i modelli locali (door)

Per Ollama, LM Studio, llama.cpp, vLLM, Jan. Ascolta sull'indirizzo abituale e sposta il modello sulla porta successiva. **Limite aperto**: la porta spostata non ha autenticazione, un processo locale può chiamare il modello direttamente. Non dire che la guardia è ermetica.
