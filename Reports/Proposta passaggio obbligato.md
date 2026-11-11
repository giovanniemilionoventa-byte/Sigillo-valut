---
tags: [report, security, proposal]
lang: it
source: codice letto il 2026-11-11 (sdk-python/src/sigillo/__init__.py, _watchdog.py, door.py, _gateway.py; apps/server/src/http/agent-setup.ts; apps/server/src/gateway/*). Codice non toccato.
stato: proposta, aspetta la scelta di Giovanni
---
# Proposta: ogni chiamata ai modelli passa per forza da Sigillo

Richiesta di Giovanni (2026-11-11): non fermare l'agente quando qualcosa va storto (es. script modificato). La priorità è **registrare sempre tutto senza interrompere**. Invece di bloccare, rendere impossibile aggirare Sigillo, così il blocco non serve mai. Anche in locale: ai modelli locali si arriva **solo** da Sigillo, punto.

## Come funziona oggi [codice]
- La registrazione avviene in gran parte **dentro il processo dell'agente** (SDK). Per questo c'è la modalità `strict`: se lo script o il pacchetto SDK cambiano, o se 3 battiti di fila non arrivano, l'agente viene **fermato** (uscita 70). La console mette `strict=True` quando è attiva la protezione.
- **Modelli cloud**: con il [[LLM gateway]] l'agente non ha la chiave del fornitore (la tiene il [[Keyholder]]), quindi non può chiamare il modello senza passare da Sigillo. Questo è già un passaggio obbligato vero. Ma il gateway è un'opzione ("più sicurezza"), non la regola.
- **Modelli locali**: la porta (`door.py`) sta davanti a Ollama sulla 11434, e Ollama viene spostato sulla 11435 **senza password**. Qualsiasi programma sulla macchina può chiamare la 11435 direttamente: il passaggio non è obbligato, c'è solo un allarme. Se Ollama è già aperto, la porta si apre accanto e si scrive `model_open`.
- Se il server non risponde per più di circa 10 secondi, le azioni di quel periodo si perdono (nessuna coda su disco).

## Idea di fondo
Spostare la registrazione **fuori dall'agente**, nel punto da cui l'agente deve per forza passare per parlare col modello. Se il modello si raggiunge solo da lì, non importa cosa fa l'agente al suo codice: ogni chiamata viene registrata lo stesso. Quindi fermare l'agente non serve più.

## Proposta in 4 punti
1. **Cloud: gateway sempre, non più opzione.** La chiave del fornitore la tiene solo Sigillo; l'agente ha solo la chiave Sigillo. Senza Sigillo non c'è modello. **Prezzo da dire chiaro:** con il gateway domande e risposte passano in chiaro nella memoria del server (non salvate, ma leggibili da un hacker dentro il server mentre gira, vedi [[Cosa vede un attaccante sul server]] caso 4); senza gateway il contenuto non arriva mai a Sigillo. Gateway predefinito = registrazione più sicura, promessa di privacy più debole. `agent-setup.ts` è toccato anche dal lavoro sulla `customer_key`: coordinarsi.
2. **Locale: "scatola Sigillo".** Il modello (Ollama) e la porta Sigillo girano insieme in un contenitore Docker. Il modello ascolta solo sull'indirizzo interno della scatola (127.0.0.1 *dentro* il contenitore), che dal computer non si raggiunge. L'unica cosa aperta verso il computer è la porta Sigillo sulla solita 11434. Così il modello è raggiungibile **solo** da Sigillo, a livello di sistema operativo, non per convenzione. Un comando solo per partire.
   - Limite onesto: chi è amministratore della macchina (o ha i permessi Docker) può sempre installare un altro modello. Vale per qualsiasi prodotto; l'agente normale non ha questi permessi.
   - Limite tecnico: su Mac, Ollama dentro Docker non usa la scheda grafica (va più lento). Per i server Linux in produzione nessun problema. Sul Mac di sviluppo resta la porta di oggi con l'allarme.
3. **Mai fermare l'agente.** `strict` non chiude più l'agente: uno script cambiato diventa la ricevuta `script_changed` (come oggi) e un allarme in console, e l'agente continua. Le chiamate ai modelli restano registrate perché passano dalla porta o dal gateway. Chi vuole davvero fermarsi può ancora usare `on_halt`.
4. **Non perdere niente se il server è giù.** La porta locale tiene una **coda su disco** (solo impronte, mai il contenuto) e la rimanda quando il server torna. Nel cloud il gateway è il server stesso: se è giù il modello non risponde (è un guasto, non un blocco di sicurezza).

## Cosa resta fuori (da dire sempre)
- Le **azioni sugli strumenti** (tool) vengono ancora registrate dall'SDK dentro l'agente. Il gateway e la porta però scrivono "il modello ha chiesto lo strumento X" ([[Tool reconciler]]): se l'agente non lo registra, compare un allarme. Da estendere anche alla porta locale.
- Non dire mai "impossibile da aggirare" a un cliente: dire "l'agente raggiunge il modello solo attraverso Sigillo".

## Sostituisce
La decisione aperta "porta del modello locale: porta casuale più allarme" ([[Status and open items]] punto 1, [[Code defects]] punto 9).

Collegati: [[Heartbeat and guard]] · [[Python SDK]] · [[LLM gateway]] · [[Security model]].
