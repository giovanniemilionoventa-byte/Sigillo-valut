---
tags: [report, identity, design]
lang: it
source: domanda di Giovanni del 2026-11-15; codice letto su main dopo la PR #154 (apps/server/src/connection/watch.ts, packages/core/src/manifest.ts). Solo progettazione: codice fermo, nessuna PR.
stato: proposta, aspetta la scelta di Giovanni
---
# Quale agente ha fatto il danno? Collegare il pacchetto all'agente giusto

## La domanda (Giovanni, 2026-11-15)
Un'azienda ha 5 agenti simili. Succede un problema. Il revisore riceve un pacchetto di prove. Come fa a sapere che:
1. il pacchetto è proprio quello **dell'agente che ha causato il problema**, e non di un altro agente simile;
2. l'agente che ha causato il problema è davvero **quello registrato** in Sigillo;
3. il pacchetto **non viene da un'altra parte** (scaricato da un altro account, o preparato da qualcun altro)?

Oggi l'unica cosa che distingue gli agenti è il **nome** (`system_id`), che sceglie l'azienda. Bisogna fidarsi, e ci si può confondere.

## Cosa c'è oggi [codice]
- Ogni agente ha la sua **chiave Sigillo** (API key): il server sa da quale agente arriva ogni evento e lo mette nella sua catena. Ma questo legame lo conosce **solo il server**: nel pacchetto c'è il nome, non una prova legata all'agente.
- La **firma** sulle ricevute è di Sigillo, una per tutto il server: prova che il registro non è stato cambiato, non *quale programma* lo ha scritto.
- Il battito manda ogni minuto l'**impronta del file dell'agente** (e della libreria), ma il server la tiene solo nella sua tabella delle connessioni: **non finisce nelle ricevute**, quindi il revisore non la vede.
- La **chiave del cliente** (dal passo 1, creata da sola in `~/.sigillo/<agente>/customer.key`) è diversa per ogni agente, ma oggi serve solo a salare le impronte; il **timbro** su ogni evento è nel passo 2, non fatto.

Quindi oggi la risposta onesta è: **il revisore si fida del nome e di Sigillo**.

## Tre domande, tre prove diverse
Conviene separare il problema in tre anelli. Ognuno ha la sua prova.

```
 il danno (email, pagamento, decisione)
        │  anello 3: questa azione è in questa catena?
        ▼
 la catena / il pacchetto
        │  anello 2: questa catena è di questo agente (programma e installazione)?
        ▼
 l'agente sul server dell'azienda
        │  anello 1: il pacchetto viene davvero da Sigillo, intero?
        ▼
 Sigillo
```

### Anello 1: il pacchetto è autentico e intero (c'è già quasi tutto)
- Il verificatore controlla firme e marche temporali con la **chiave pubblica di Sigillo**, pubblicata sul sito: un pacchetto preparato da altri non passa.
- Il firmatario non permette due storie diverse per lo stesso agente: due pacchetti dello stesso agente devono combaciare.
- **Da aggiungere (piccolo):** un **link per il revisore**: il revisore scarica il pacchetto direttamente da Sigillo, non dalla copia dell'azienda. Così sa da dove viene.

### Anello 2: la catena appartiene a questo agente. Due soluzioni, da usare insieme

**A. Impronta del programma nella catena** (l'idea di Giovanni di "caricare il file Python")
- Come: il battito già calcola l'impronta del file dell'agente. Basta **scriverla nella ricevuta "collegato"** (e in "script cambiato"), invece di tenerla solo nel server. Così ogni versione del programma che ha girato resta nella catena, con l'ora.
- Il revisore prende il file `.py` **dal server dell'azienda** dove gira l'agente che ha fatto il danno, e il verificatore dice: "questo file è quello che girava in questa catena dal giorno X al giorno Y".
- Esempio: 5 agenti di rimborsi; il file `rimborsi_nord.py` del server di Milano ha impronta `7a3f...`; nel pacchetto "rimborsi-nord" la ricevuta "collegato" del 3 novembre ha `7a3f...`. Combaciano.
- Pro: semplice, nessuna libreria nuova, il file non esce mai (solo impronta), il revisore capisce.
- Limiti: (1) due agenti con **lo stesso codice** e configurazioni diverse hanno la stessa impronta; (2) conta solo il file principale, non le impostazioni, le variabili d'ambiente o il prompt in un altro file (si possono aggiungere, ma la lista cresce); (3) l'impronta la calcola il programma stesso, quindi un'azienda che vuole barare può mandare quella di un altro file.

**B. Una chiave segreta per ogni installazione** (il timbro del passo 2)
- Come: ogni agente ha la sua chiave, creata sulla sua macchina, che non esce mai. Il registratore **timbra ogni evento** con quella chiave. Due agenti con lo stesso codice hanno comunque chiavi diverse.
- Il revisore va sulla macchina dell'agente che ha fatto il danno e controlla che la **chiave trovata lì** sia quella che ha timbrato gli eventi della catena.
- Due modi:
  - **B1, chiave che c'è già (chiave del cliente, HMAC):** nessuna libreria nuova. Per controllare, l'azienda **consegna la chiave** al revisore (poi la cambia). Più semplice, ma il revisore vede la chiave.
  - **B2, coppia di chiavi (firma vera, Ed25519):** la parte privata resta sulla macchina, quella pubblica sta nella prima ricevuta della catena. Il revisore chiede alla macchina di firmare una frase a caso ("firma: verifica-15-novembre-8841") e controlla con la chiave pubblica: la chiave non esce mai. È la soluzione migliore, ma in Python serve una **libreria nuova** (`cryptography`), non nella lista approvata: serve il sì di Giovanni (stessa questione della contraddizione 4 della [[Riprogettazione]]).
- Pro: distingue anche agenti identici; più difficile da confondere.
- Limiti: chi ha la macchina ha la chiave; se la chiave viene copiata su un'altra macchina, si sposta anche l'identità. Il file della chiave va trattato come una password.

### Anello 3: il danno è dentro questa catena (la prova più forte)
- Il danneggiato ha in mano il **risultato**: l'email ricevuta, l'importo pagato, la lettera di rifiuto.
- Con la chiave del cliente si fa l'**apertura**: si dimostra che il testo di quell'email ha l'impronta salvata nella ricevuta delle 14:31 di questa catena. Questo esiste già nel formato (punto 12 della [[Riprogettazione]]); manca solo il comando nel verificatore.
- **Idea in più, molto semplice: il numero di ricevuta nel risultato.** L'agente scrive in fondo a ogni email o documento un riferimento corto, es. "Rif. Sigillo rimborsi-nord #1842". Chi riceve il danno ha già in mano il riferimento: va dritto alla ricevuta giusta, nel pacchetto giusto, senza confusione tra agenti. Facoltativo, deciso dall'azienda.
- Pro: lega il danno alla catena senza fidarsi dei nomi. Limite: serve che l'azienda conservi la chiave del cliente (se la perde, l'apertura non si fa più).

## Cosa nessuna soluzione può fare (da dire sempre)
Se l'azienda **vuole barare** sulla propria macchina (fa girare il codice dell'agente A con la chiave dell'agente B), nessuna prova dall'interno della macchina lo impedisce del tutto. Le soluzioni sopra **eliminano gli errori e le confusioni** e rendono un imbroglio deliberato **difficile e visibile** (bisogna falsificare apposta impronte e chiavi), non impossibile. La difesa vera contro un'azienda disonesta sta fuori: il risultato in mano al danneggiato (anello 3) e le marche temporali.

## Consiglio
In ordine di costo, tutto compatibile con lo scheletro:
1. **Subito quando il codice riparte, piccolo:** impronta del programma nella ricevuta "collegato" (A) + comando del verificatore "confronta questo file con il pacchetto". Nessuna libreria nuova, nessun cambio di formato (l'impronta va in un campo che le ricevute hanno già).
2. **Con il passo 2:** timbro per installazione (B). Partire con B1 (nessuna libreria nuova); B2 solo se Giovanni approva `cryptography`.
3. **Comando di apertura nel verificatore** (anello 3), già previsto; il riferimento nel risultato come consiglio ai clienti.
4. **Link per il revisore** (anello 1).

Con 1 + 3 si risponde già alla domanda di Giovanni per il caso normale (errore o confusione tra agenti); 2 serve per i clienti che chiedono di più (banche).

Collegati: [[Riprogettazione]] · [[L'agente può mentire a Sigillo]] · [[Evidence pack]] · [[Salted digests and openings]] · [[Heartbeat and guard]] · [[Verifier]].
