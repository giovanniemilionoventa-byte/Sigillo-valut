---
tags: [report, architecture, redesign]
lang: it
stato: SOLO TEORIA (decisione di Giovanni, 2026-11-12): niente codice finché Giovanni non lo dice
---
# Riprogettazione di Sigillo (teorica)

Richiesta di Giovanni (2026-11-12): il sistema di oggi è troppo complicato ([[Proposta passaggio obbligato]], file di progetto `architettura/Sigillo-problemi.docx`). Riprogettarlo più lineare, partendo dalle fondamenta (funzione base), poi aggiungere a strati privacy, battito e le altre garanzie. **Solo riprogettazione teorica, non pratica, finché Giovanni non dice altrimenti.**

## Passo 1: lo scheletro (proposto il 2026-11-12, aspetta l'OK)
Funzione base: dimostrare a chiunque cosa ha fatto un agente AI, senza doversi fidare né del cliente né di Sigillo.

Tre pezzi:
1. **Registratore** (dal cliente): cattura l'azione, calcola l'impronta del testo lì, scrive l'evento su disco locale, poi lo spedisce. Non firma, non decide, non blocca.
2. **Notaio** (da Sigillo): un solo ingresso, un solo formato (l'evento); mette in catena, firma, conserva, sigilla ogni ora con marca esterna, consegna il pacchetto. Vede solo impronte.
3. **Verificatore** (dal revisore): controlla il pacchetto da solo. Più l'ente del tempo esterno.

Cinque regole: un solo percorso; il testo non esce dal cliente; prima si scrive poi si spedisce (l'agente non si ferma mai); firma e catena in un solo posto; prova verificabile senza fidarsi di nessuno.

Corrispondenza con oggi: SDK + porta + guardiano → registratore; server + firmatario + checkpointer → notaio; verificatore uguale. Oggi gli ingressi sono 5 (OTLP, SDK, API, gateway, porta).

## Idea di Giovanni (2026-11-12): tutto sul server del cliente (self-hosted)
Fattibile: il deploy è già Docker Compose (4 contenitori). Pro: nessun dato esce, adatto a banche e assicurazioni, si sposa con la licenza al partner. Contro: se il cliente tiene anche la chiave di firma può riscrivere la propria storia (prova più debole verso terzi); aggiornamenti, supporto e conteggio dell'uso più difficili. Le marche temporali esterne proteggono il passato già marcato, ma non impediscono due catene parallele. Variante consigliata: **self-hosted + testimone Sigillo**, cioè tutto dal cliente e solo il sigillo orario (una impronta all'ora) mandato a Sigillo, che lo conserva e lo marca. Da decidere.

## Schema semplice (2026-11-12)
Giovanni ha chiesto di restare il più semplice possibile e di ripartire dallo scheletro (l'idea self-hosted resta solo come curiosità). Schema PNG: file di progetto `architettura/sigillo-architettura-semplice.png` (agente AI e registratore dal cliente in Python, notaio su get-sigillo.eu, ente del tempo, pacchetto, verificatore).

## Parti tolte dallo scheletro, da rimettere (elenco del 2026-11-12)
- Registratore: chiave del cliente, pseudonimi, impronte dei documenti, battito, guardiano, strict (da togliere), porta per modello locale, altri ingressi (SDK TypeScript, OTLP, API).
- Tra agente e modelli: gateway + custode delle chiavi.
- Notaio: firmatario separato, Caddy, chiave Sigillo per agente, controllo strumenti, sali e aperture, cancellazione GDPR, quote.
- Prodotto: console e account, "Carica il tuo agente", rapporto PDF ed export notturno, backup, sito e demo.
- Aggiunta nuova: coda su disco nel registratore.

## Lavagna e punti decisi (2026-11-12)
Lavagna infinita (artefatto): https://claude.ai/artifact/PcV9vbKJhwu8mrk5mSAkt8 . Si procede un punto alla volta (elenco 1-17 nella lavagna), Giovanni decide e il punto viene aggiunto allo schema.
- **Punto 1, chiave del cliente: deciso.** Sta nel registratore (SDK), non va mai al server, che riceve solo impronte non leggibili. Si propone: chiave creata dalla pagina "Carica il tuo agente" nel browser e scritta nel file dell'agente (una per agente, automatica, sempre presente). **Aggiunta (2026-11-12, Giovanni ha detto sì):** la stessa chiave timbra ogni evento nel registratore, così il cliente prova che le azioni sono del suo agente e non di estranei (ricevute false in coda). Da fare più avanti: il verificatore deve controllare che i timbri corrispondano alla chiave del cliente. Oggi la chiave serve solo a salare le impronte.

- **Punto 2, pseudonimi: tolto dallo scheletro (2026-11-12, Giovanni conferma).** Oggi il campo "per conto di chi" lo compila a mano il programmatore, quasi nessuno lo usa. Si aggiunge solo se un cliente lo chiede (campo facoltativo, codice calcolato dal registratore con la chiave del cliente).

- **Punto 3, impronte dei documenti: in attesa** (Giovanni, 2026-11-12): solo se un cliente lo chiede.
- **Punto 4, battito: deciso.** Evento come gli altri, timbrato, nella catena dallo stesso ingresso; coda su disco se la rete cade, ricarica da sola al ritorno online. Chiusura normale = evento "chiuso"; crash = "battiti interrotti, motivo non noto".
- **Punto 5, guardiano (processo separato): in attesa** (Giovanni conferma, 2026-11-13). Serve solo a sapere sempre il motivo di un crash; se ne riparla se un cliente lo chiede.
- **Punto 6, porta per il modello locale: in attesa** (Giovanni, 2026-11-13). Il registratore registra le chiamate al modello locale come tutte le altre; la porta (Docker, modello isolato) solo se un cliente con modelli locali chiede la garanzia che nulla sfugga.
- **Punto 7, altri modi di collegarsi: in attesa** (Giovanni, 2026-11-13). Solo libreria Python e un solo ingresso; TypeScript, OpenTelemetry e API diretta solo se un cliente reale li usa.
- **Punto 8, gateway per i modelli cloud: in attesa** (Giovanni, 2026-11-13). Perché: per imporre il passaggio il gateway deve tenere la chiave del fornitore e quindi leggere la richiesta in chiaro; cifrare con la chiave del cliente non funziona (il fornitore non la capisce). Strade vere: gateway sul server del cliente (preferita), enclave protetta (costosa, non su Aruba), tunnel cieco (non impone il passaggio). Nello scheletro il registratore registra le chiamate da dentro l'agente.
- **Punto 9, firmatario separato: deciso**, parte interna del notaio, com'è oggi (chiave fuori dalla VPS: miglioramento successivo).
- **Punto 10, chiave Sigillo per agente: deciso.** Una per agente, solo per dire al server chi scrive, revocabile dalla console. Chi ha il file dell'agente ha entrambe le chiavi e può mandare eventi a nome dell'agente (con il timbro giusto) ma non cambiare il passato firmato né toccare altri agenti o la chiave di firma: il file va trattato come una password.
- **Punto 11, controllo strumenti: in attesa (proposto 2026-11-13)**, dipende dal gateway (punto 8).

**Limite dello scheletro (2026-11-13):** senza gateway e porta il registratore lavora solo da dentro l'agente: prova che il passato non si cambia e quando l'agente era collegato, non che il registro sia completo se il programmatore vuole barare. Vedi [[L'agente può mentire a Sigillo]].
- **Punto 12, aperture: deciso come proprietà del punto 1** (2026-11-13). Con testo e chiave del cliente si dimostra il contenuto dietro un'impronta; manca solo il comando nel verificatore (da fare insieme al controllo dei timbri).
- **Punto 13, cancellazione GDPR: ridotta a una funzione** (2026-11-13): eliminare l'account di un cliente che se ne va, con ricevute e chiavi. Il resto sparisce perché il server non tiene dati personali (niente pseudonimi, nessuna chiave del cliente).
- **Punto 14, quote e limiti: proposto** (aspetta il sì): limite al minuto per agente + contatore mensile per account; se il limite scatta l'agente non si ferma (coda su disco).

## Versione finale dello scheletro (proposta 2026-11-13, per il cliente tipo: software house con agenti Python, poi banche e assicurazioni; KISS)
Lavagna aggiornata: https://claude.ai/artifact/PcV9vbKJhwu8mrk5mSAkt8 (schema, cosa garantisce, cosa non garantisce, cosa cambia rispetto a oggi).

Contraddizioni trovate tra i punti già decisi e come si risolvono:
1. **Il battito (4) mangiava la quota (14):** un evento al minuto sono circa 43.000 ricevute al mese per agente, più dei 10.000 gratuiti, e gonfia la catena. Risolto: "sono acceso" solo se l'agente è fermo da 5 minuti (se lavora bastano le azioni); più "chiuso" e "ripartito". Il battito non conta nella quota.
2. **Quota (14) contro "mai perdere eventi":** bloccare oltre la quota farebbe perdere prove. Risolto: oltre la quota si continua a registrare e si segna l'account (questione commerciale); il limite al minuto risponde "riprova", il registratore riprova dalla coda.
3. **Promessa di completezza di prima ("la timbratura e il battito rendono visibile il buco"): troppo forte.** Chi ha il file dell'agente ha entrambe le chiavi e può mandare battiti e azioni false. Corretto: la prova è autentica e le interruzioni accidentali si vedono, ma non si prova che l'agente non abbia saltato azioni. Scritto nella lavagna come "cosa non garantisce".
4. **Timbro che solo il cliente sa controllare (1) contro il revisore esterno:** per verificare il timbro il revisore dovrebbe avere la chiave del cliente. Possibile soluzione: una sola coppia di chiavi (privata dal cliente, pubblica al server e al revisore); richiede una libreria di firma nuova nel Python (non è nella lista approvata): in attesa, serve l'ok di Giovanni.
5. **Coda su disco (4) negli ambienti senza disco persistente** (container, serverless): la coda si perde al riavvio. Limite da dichiarare; fallback in memoria con avviso.
6. **Chiave del cliente (1): tre modi di averla** (pagina, registratore, variabile d'ambiente). Regola unica: se c'è, si usa; se manca, il registratore la crea in un file locale e avvisa di farne una copia.

Semplificato o tolto: console ridotta (crea agente, stato, pacchetto, revoca chiavi, elimina account); export notturno in attesa; caricamento del file dalla console in attesa (per sviluppatori basta la riga di codice); regole speciali di sessioni e battiti tolte. Aperti fuori dallo scheletro: pubblicare `sigillo` su PyPI (servono gli account), marca temporale qualificata per le banche, revisione di sicurezza (thread a parte dopo l'implementazione).

- **Punto 16, copie di sicurezza: nello scheletro**, come dice Giovanni (2026-11-13): ogni notte alle 3, file cifrati `.age` su un drive esterno. Sulla lavagna è **fuori da Sigillo**, in una zona Google Drive a parte. Le zone esterne sono quattro: marca temporale (richiesta via API/Internet, ogni ora), Google Drive (backup notturno), download dal cliente (pacchetto di prove, freccia "download"), computer del revisore (verificatore).

## Strati da progettare dopo (in ordine proposto)
1. Privacy (chiave del cliente, pseudonimi). 2. Completezza (battito, passaggio obbligato verso i modelli). 3. Sicurezza delle chiavi (firma fuori dalla VPS). 4. Prodotto (console, account, export).

Schema: file di progetto `architettura/riprogettazione-1-scheletro.png`.
