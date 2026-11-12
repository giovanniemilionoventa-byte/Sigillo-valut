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

## Strati da progettare dopo (in ordine proposto)
1. Privacy (chiave del cliente, pseudonimi). 2. Completezza (battito, passaggio obbligato verso i modelli). 3. Sicurezza delle chiavi (firma fuori dalla VPS). 4. Prodotto (console, account, export).

Schema: file di progetto `architettura/riprogettazione-1-scheletro.png`.
