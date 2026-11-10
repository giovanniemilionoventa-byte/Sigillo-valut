# Stato, decisioni, cose aperte

Obiettivo del progetto (2026-11-09): portare Sigillo da prototipo funzionante a prodotto vendibile sul mercato europeo.

## Stato tecnico

Tutte le tappe M1-M9, N1-N6 e le fasi pilota 1-11 sono fatte. Ultimo conteggio annotato: 1270 test Node (non rieseguiti il 2026-11-09). `main` della repo del codice dopo la PR #147.

## Decisioni di Giovanni

- 2026-11-09: un solo prodotto, licenza al partner, società solo se i clienti la chiedono. Backup fuori sede su Google Drive. Comet venerdì 13 novembre. Niente file da caricare dal vecchio progetto.
- 2026-11-09: seconda memoria su GitHub (questo vault), aggiornata di default.

## Aperto: aspetta Giovanni

1. Porta del modello locale: porta casuale più allarme.
2. Conservazione automatica a 6 o 12 mesi.
3. Campo codice chiave nella pagina `/verify`.
4. PR #144 (simulatore di chiamata): unire o chiudere. PR #129 (zod): chiudere, già dentro la #145.
5. Licenza da mettere nella repo, oppure togliere "open source" dal sito.

## Aperto: solo Giovanni può farlo

Revocare la chiave di prova dell'agente di audit; SSH solo con chiave; togliere l'aggiornamento automatico come root; copie fuori sede non cancellabili; provare un ripristino del backup; avvocato per DPA e contratto di licenza.

## Non verificato

Versione su `get-sigillo.eu` e suo `.env`; riavvio dello stack dopo le PR #145-147; cron e backup che girano davvero; aggiornamento automatico attivo; secondo fattore attivato; repo GitHub pubblica o privata; data center esatto; esito di incontri ed email di ottobre; dati sui concorrenti (di ottobre); accettazione dei termini alla registrazione.

## Documenti della repo da riallineare al codice

`docs/API.md` (gateway, fornitori, scrypt), `docs/STORAGE-INVENTORY.md` (scrypt), `docs/PROJECT-KNOWLEDGE.md` (caricamento TypeScript), Termini (backup), DPA (DigiCert), sito ("open source").
