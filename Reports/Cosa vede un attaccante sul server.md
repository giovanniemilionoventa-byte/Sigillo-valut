---
tags: [report, security, privacy]
lang: it
source: codice letto il 2026-11-10 (apps/server/src/assurance/privacy-check.ts, storage/store.ts, sdk-python/src/sigillo/__init__.py, packages/sdk-typescript/src/filter.ts, apps/server/src/http/sdk-bundle.ts, pages.ts, agent-setup.ts, deploy/docker-compose.yml, docs/DATA-PROTECTION.md). Codice non toccato.
---
# Cosa vede (e cosa può fare) un attaccante che entra nel server

Domanda di Giovanni (2026-11-10): il server di Sigillo ha accesso alle "hash" dei clienti? Se un hacker entra, cosa vede?

## In breve
- **Sì, il server ha le impronte (hash)** di ogni input e output: sono dentro le ricevute firmate, è il prodotto stesso. Un attaccante che copia il database le ha tutte.
- **Il contenuto (prompt, argomenti, risposte) no**: non arriva mai al server se l'agente usa l'SDK.
- **Un'impronta da sola non serve a niente** se è "chiusa" con la chiave del cliente (`customer_key`): senza quella chiave non si può nemmeno indovinare il contenuto. Questo è il caso degli agenti creati dalla console (la pagina "Carica il tuo agente" mette la chiave in automatico).
- **Ci sono 5 casi in cui invece l'attaccante può indovinare o leggere** (sotto). Sono la vera risposta alla domanda.

## Cosa c'è nel database, in chiaro
Fonte: `STORAGE_INVENTORY` in `privacy-check.ts` (generato anche in `docs/STORAGE-INVENTORY.md`).
- Nomi di agenti, strumenti, modelli, tipo di azione, esito (ok/errore), orari, etichette dei documenti ("curriculum"), tipo di file.
- Email di chi fa login e nome dell'azienda.
- Tabella `subjects`: l'identificativo della persona per cui ha agito l'agente (`user.id`) accanto al suo pseudonimo, **se** il cliente non usa `sigillo.pseudonym()` lato suo.
- Impronte (hash) di input, output, documenti, script dell'agente.
- Tabella `openings`: i "sali" (nonce) delle impronte, **quando il cliente non ha la sua chiave** (vedi caso 1).
- Chiavi dei provider (OpenAI ecc.) del gateway: cifrate; la chiave per aprirle è nel container `keyholder`, non nel database.
- Chiave privata di firma: **non** nel database, solo nel container `signer`.

## I 5 casi in cui un'impronta si può indovinare o il contenuto si vede
1. **SDK senza `customer_key`** (è il valore predefinito di `sigillo.init()` se si scrive il codice a mano). L'SDK sala l'impronta con un nonce casuale e **manda il nonce al server**, che lo salva in `openings`. Chi ha il database ha impronta + nonce, quindi può provare valori: importi, sì/no, punteggi, nomi di beneficiari noti si trovano in secondi. Il sale qui protegge solo da chi ha l'export senza `openings`, non da chi ha il server.
2. **Impronte "plain"** (API nativa delle ricevute, o SDK con `salt_content=False`): SHA-256 semplice, indovinabile da chiunque abbia le ricevute, anche senza il server.
3. **OpenTelemetry senza SDK Sigillo** (attributi grezzi): il contenuto arriva in chiaro al server, che lo sala e lo butta. Un attaccante **già dentro** può leggerlo mentre arriva; dal database può indovinarlo (il nonce è in `openings`).
4. **Gateway LLM**: il prompt passa in chiaro nella memoria del server verso il provider. Chi controlla il server mentre gira vede tutto. Inoltre, con accesso root alla macchina, può leggere la chiave del `keyholder` e aprire le chiavi dei provider dei clienti.
5. **Il server distribuisce lui stesso l'SDK e la pagina che crea la chiave**: l'SDK TypeScript si scarica dal server (`sdk-bundle.ts`), il comando `pip install` è scritto dalla pagina del server (`pages.ts`), e la `customer_key` è generata da JavaScript servito dal server (`agent-setup.ts`). Un attaccante che modifica il server può far scaricare **ai nuovi clienti** un SDK o una pagina modificati che mandano fuori contenuto o chiave. Non tocca gli agenti già installati.

## Cosa può alterare
- **Il passato firmato, no senza essere scoperto**: le ricevute sono firmate e concatenate, e ogni ora la radice è marcata da un'autorità di tempo esterna (FreeTSA, DigiCert). Riscrivere il passato rompe firme o marche temporali; il verificatore lo segnala.
- **Aggiungere ricevute false in coda, sì** (chiedendo al signer di firmarle). Il signer rifiuta solo ciò che non allunga la sua catena.
- **Tagliare la coda (cancellare le ultime ricevute)**: si vede solo se il cliente confronta con un export precedente (`--previous`).
- **Con root sulla macchina** (tutti e 4 i container sono sulla stessa VPS Aruba): può copiare la chiave privata di firma e firmare ciò che vuole d'ora in poi, e riscrivere l'ultima ora non ancora marcata. Una chiave nuova si scopre solo con `--key-id`.
- **Account e console**: può leggere email e nomi aziende, cancellare pseudonimi o nonce (perdita della possibilità di "aprire" le impronte), scaricare export (senza contenuti).

## Raccomandazioni (non ancora decise, vedi [[Status and open items]])
1. Rendere la `customer_key` **obbligatoria** (o almeno predefinita) in entrambi gli SDK: senza chiave l'SDK si rifiuta di partire o avvisa forte. Toglie i casi 1 e 3.
2. Vietare o segnalare le impronte `plain` (caso 2) nell'API nativa, o salarle col nonce del cliente.
3. Gateway: dirlo chiaro nei contratti (già scritto in `docs/DATA-PROTECTION.md`) e consigliarlo solo a chi accetta che il prompt passi dal server.
4. SDK da una fonte che non sia il server: pacchetto firmato su PyPI e npm, con hash pubblicato; la pagina della chiave come file statico verificabile.
5. Chiave di firma fuori dalla VPS (chiave hardware/HSM o un secondo server), e i clienti verificano sempre con `--key-id` e `--previous`. Pubblicare la chiave pubblica in un posto che il server non controlla (sito separato, contratto).
6. Sicurezza della macchina: SSH solo con chiave, secondo fattore, niente aggiornamento automatico come root (già in [[Status and open items]]).

Collegati: [[Security model]] · [[Data protection]] · [[Salted digests and openings]] · [[Signatures and keys]] · [[LLM gateway]] · [[Keyholder]].
