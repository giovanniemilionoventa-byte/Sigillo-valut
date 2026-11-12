---
tags: [status]
lang: it
---
# Stato e cose aperte

Obiettivo (2026-11-09): da prototipo funzionante a prodotto vendibile sul mercato europeo.

## Stato tecnico [codice, 2026-11-10]
Tappe M1-M9, N1-N6 e fasi pilota 1-11 fatte. 1713 test Node. Formato ricevuta v4. Schema DB v10. SDK Python 0.12.0, SDK TypeScript 0.1.0. Revisione completa del codice fatta il 2026-11-10; documenti corretti nella PR #149 (unita il 2026-11-10); difetti in [[Code defects]].

## Aperto: aspetta una decisione di Giovanni
1. **Riprogettazione teorica** di Sigillo (2026-11-12): partire dallo scheletro (registratore, notaio, verificatore), poi aggiungere gli strati. Solo teoria finché Giovanni non dice altrimenti; il passaggio obbligato per i modelli si rivede dentro questo lavoro. Vedi [[Riprogettazione]].
2. Conservazione automatica a 6 o 12 mesi.
3. Campo codice chiave nella pagina `/verify`.
4. PR #144 (simulatore di chiamata): unire o chiudere.
5. Licenza nella repo, oppure togliere "open source" dal sito.
6. Quali difetti di [[Code defects]] correggere.
7. Rischi se un hacker entra nel server: quali raccomandazioni di [[Cosa vede un attaccante sul server]] fare (prima di tutto `customer_key` obbligatoria negli SDK).

## Aperto: solo Giovanni può farlo
Revocare la chiave di prova dell'agente di audit; SSH solo con chiave; togliere l'aggiornamento automatico come root; copie fuori sede non cancellabili; provare un ripristino del backup; avvocato per DPA e contratto di licenza.

## Non verificato
Versione in produzione e suo `.env`; riavvio dopo le ultime PR; cron e backup che girano davvero; secondo fattore attivato; repo pubblica o privata; data center esatto; esito di incontri ed email di ottobre; dati sui concorrenti.
