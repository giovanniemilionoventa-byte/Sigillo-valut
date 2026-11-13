---
tags: [report, security, completeness]
lang: it
source: codice letto il 2026-11-13 (sdk-python/src/sigillo/__init__.py, _stdlib.py, _watchdog.py, _gateway.py; apps/server/src/ingest/adapter.ts, connection/watch.ts, http/server.ts, check/tool-reconcile.ts, http/agent-setup.ts; docs/FORMAT.md 2.1). Codice non toccato.
---
# L'agente può mentire a Sigillo?

Domanda di Giovanni (2026-11-13): l'agente dichiara sempre, in automatico, quello che fa a Sigillo? Dove, come, perché? Può mentire? È uno strumento dentro il prompt?

## In una riga
**Non è uno strumento nel prompt.** Il modello (GPT, Claude, Mistral...) non sa nemmeno che Sigillo esiste e non decide niente. La registrazione la fa **codice Python** caricato dentro il programma dell'agente: il modello non lo può spegnere, ma **il programmatore (o chi controlla la macchina) sì**. Quindi la domanda giusta non è "il modello può mentire?" (no), ma "chi scrive o gestisce il programma dell'agente può far sparire delle azioni?" (sì, in parte, e Sigillo se ne accorge solo in alcuni casi).

## Tre attori, da non confondere
1. **Il modello** (il "cervello"): riceve testo e risponde testo. Non ha in mano nessun pulsante di Sigillo.
2. **Il programma dell'agente** (il codice Python del cliente): chiama il modello, esegue gli strumenti (leggere un file, mandare un'email, approvare un pagamento). Qui dentro gira il registratore di Sigillo.
3. **Il server Sigillo** (il notaio): riceve le impronte, mette in catena, firma, sigilla ogni ora con la marca esterna.

Esempio: un agente rimborsi chiede a GPT "approvo 1.240 euro?". GPT risponde "sì, usa lo strumento approva_pagamento". Il programma esegue approva_pagamento. Sigillo registra (a) la chiamata a GPT e (b) l'uso dello strumento, perché il codice Sigillo è "agganciato" alla libreria OpenAI e al framework (LangChain, CrewAI), non perché GPT glielo dice.

## Come funziona oggi, nel codice
- Il cliente aggiunge **tre righe** all'inizio del file dell'agente (`import sigillo` + `sigillo.init(...)`); la pagina "Carica il tuo agente" le scrive da sola (`agent-setup.ts`).
- `sigillo.init` **aggancia** (in gergo: "strumenta") le librerie che l'agente usa: LangChain, CrewAI, client OpenAI. Per un agente senza framework, `stdlib` aggancia le chiamate web (`urllib`, `requests`, `httpx`) e la scrittura dei file. Da quel momento ogni chiamata al modello e ogni strumento eseguito dal framework diventa un evento, **senza che il programmatore scriva altro e senza che il modello lo sappia**.
- Prima di uscire dalla macchina il testo diventa **impronta** (SHA-256 salata): a Sigillo arriva "c'è stata una chiamata a gpt-4o, impronta della domanda X, impronta della risposta Y", mai il testo.
- Gli eventi vengono **mandati a gruppi ogni pochi secondi** (coda in memoria, non su disco).
- In parallelo parte il **battito**: ogni 60 secondi l'agente dice al server "sono vivo", insieme all'impronta del proprio script e del codice Sigillo. Un **guardiano** (processo separato) avvisa se l'agente muore o il computer si spegne.
- L'**ora** che conta è quella del server (`ts_received`), non quella dichiarata dall'agente (`ts_event`, che il formato stesso dichiara "non affidabile").

## Cosa è automatico e cosa no
| | automatico? |
|---|---|
| chiamate al modello fatte con LangChain, CrewAI o client OpenAI | sì |
| strumenti eseguiti da LangChain / CrewAI | sì |
| agente "fatto a mano" con `stdlib`: chiamate web e scrittura file | sì, ma solo se la pagina ha messo `stdlib` |
| strumenti fatti a mano (es. una funzione che scrive su un database o manda un'email con `smtplib`) | **no**: il programmatore deve avvolgerli in `tracing.tool("nome")` |
| "decisioni" (tipo `decision`) | **no**: le segna il programmatore o un framework di guardrail, mai il modello |
| impronte dei documenti (`sigillo.artifact`) | **no**: le chiama il programmatore |
| risposta del modello con `stdlib` | **no**: si registra la domanda, non la risposta (il codice lo dice) |

## Chi può mentire, e come
**Il modello: no.** Non può saltare la registrazione, perché non passa da lui. Può dire cose false nella risposta, ma quella risposta viene registrata (come impronta) così com'è: è proprio ciò che Sigillo serve a provare dopo.

**Il programmatore o chi gestisce la macchina: sì, in vari modi.** Il registratore vive dentro il suo programma, quindi lui ha l'ultima parola:
1. **Togliere le tre righe** o non avvolgere uno strumento fatto a mano → quelle azioni semplicemente non esistono per Sigillo.
2. **Spegnere il registro a metà** dentro il programma (in memoria), lasciando acceso il battito → il server vede un agente vivo che "non fa niente".
3. **Modificare il codice Sigillo** installato sulla macchina perché scarti certe azioni e mandi le impronte "giuste" nel battito: l'impronta dello script e dell'SDK è **dichiarata dall'agente stesso**, quindi un programma truccato può mentire anche su quella.
4. **Inventare eventi** (mandare azioni mai avvenute con la sua chiave Sigillo) o mettere un'ora falsa in `ts_event` (ma l'ora del server resta vera).
5. **Agente "ingannato"** (prompt injection) che ha uno strumento per eseguire codice Python: in teoria potrebbe spegnere il registro come al punto 2. Caso raro, ma da sapere.

**Perdite non volute (senza malizia):** se il server non risponde per più di circa 10 secondi o il programma viene ucciso, gli eventi ancora in coda in memoria si perdono (nessuna coda su disco, già nel piano della [[Riprogettazione]]).

## Cosa Sigillo rileva comunque
| trucco | Sigillo se ne accorge? |
|---|---|
| cambiare il passato già registrato (cancellare, modificare, riordinare) | **sì, sempre**: catena firmata + sigillo orario con marca esterna; il verificatore lo trova |
| togliere del tutto Sigillo dall'agente | **sì**: il battito si ferma, dopo 3 minuti ricevuta "connessione persa" |
| agente che muore, computer spento, rete giù | **sì**: guardiano e battito scrivono il motivo (quando lo conoscono) |
| modificare lo script dell'agente o il codice Sigillo su disco | **sì, se non è truccato anche il battito** (ricevuta `script_changed`; oggi di base attivo solo sui sistemi dell'amministratore, `SIGILLO_SCRIPT_GUARD=operator`) |
| ora falsa | **sì**: conta l'ora del server, limitata dalla marca esterna |
| strumento fatto a mano non registrato | **solo con il gateway**: il gateway vede che il modello ha chiesto lo strumento; se l'agente non lo registra entro 10 minuti → allarme `tool_unrecorded` ([[Tool reconciler]]) |
| chiamata al modello che salta il registro | **solo con il gateway** (cloud) o la porta (modello locale): oggi entrambi "in attesa" nello scheletro |
| registro spento in memoria, battito acceso | **no** (senza gateway) |
| codice Sigillo truccato che mente anche nel battito | **no** |
| eventi inventati con la chiave giusta | **no**: Sigillo prova *chi* li ha mandati e *quando*, non che siano veri |

## Cosa promettere a un cliente (frase onesta)
"Sigillo prova che il registro **non è stato cambiato dopo**: nessuno, nemmeno noi, può cancellare o riscrivere un'azione già registrata senza che si veda. Prova anche **quando l'agente era collegato e quando no**. Che il registro sia **completo** dipende da come l'agente è collegato: con le tre righe registra in automatico le chiamate al modello e gli strumenti del framework; per avere la garanzia che nulla sfugga, l'agente deve passare per forza da Sigillo per parlare con il modello (gateway o porta)."

Mai dire "impossibile mentire" o "registra tutto, sempre".

## Cosa vuol dire per lo scheletro
Nello scheletro della [[Riprogettazione]] gateway, guardiano e porta sono "in attesa": il registratore lavora **solo da dentro l'agente**. Quindi lo scheletro garantisce l'**integrità** (il passato non si cambia) e la **presenza** (il battito), non la **completezza contro un cliente che vuole barare**. Va bene per il cliente tipo (una software house che vuole difendersi, non nascondere), ma va detto. Il primo passo che chiude il buco più grande senza toccare la privacy è la "strada C" della [[Proposta passaggio obbligato]] (ponte dal cliente), da tenere nel cassetto finché un cliente non la chiede.

Collegati: [[Python SDK]] · [[Heartbeat and guard]] · [[Security model]] · [[Flow - agent action to receipt]] · [[Cosa vede un attaccante sul server]].
