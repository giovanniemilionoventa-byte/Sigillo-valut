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

## Strati da progettare dopo (in ordine proposto)
1. Privacy (chiave del cliente, pseudonimi). 2. Completezza (battito, passaggio obbligato verso i modelli). 3. Sicurezza delle chiavi (firma fuori dalla VPS). 4. Prodotto (console, account, export).

Schema: file di progetto `architettura/riprogettazione-1-scheletro.png`.
