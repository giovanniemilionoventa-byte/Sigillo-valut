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

8. **Quale agente ha fatto il danno** (2026-11-15): come legare il pacchetto all'agente giusto quando ci sono più agenti simili. Proposta: impronta del programma nella ricevuta "collegato", timbro per installazione, apertura del risultato (email, pagamento) nel verificatore, link per il revisore. Libreria `cryptography` solo se Giovanni dice sì. Vedi [[Quale agente ha fatto il danno]].

## Aperto: solo Giovanni può farlo
Revocare la chiave di prova dell'agente di audit; SSH solo con chiave; togliere l'aggiornamento automatico come root; copie fuori sede non cancellabili; provare un ripristino del backup; avvocato per DPA e contratto di licenza.

## Non verificato
Versione in produzione e suo `.env`; riavvio dopo le ultime PR; cron e backup che girano davvero; secondo fattore attivato; repo pubblica o privata; data center esatto; esito di incontri ed email di ottobre; dati sui concorrenti.

## Assetto definitivo (2026-11-15)
L'abbonamento Claude di Giovanni scade il 2026-11-16: il software resta fermo per un po'.
- **Codice (`main`)**: architettura di base, passo 1 (PR #154). Il passo 2 (timbro, API diretta, impronte dei documenti) NON si fa ora: il timbro cambia il formato delle ricevute e non va fatto all'ultimo momento.
- **Produzione**: consiglio di NON aggiornare il VPS adesso. La versione in produzione funziona, e senza assistenza un aggiornamento che va storto non si può sistemare. Il nuovo registratore Python (0.13, installato dal link della console) funziona anche con il server di oggi. Quando si aggiorna, usare `docker compose up -d --remove-orphans` per spegnere il vecchio contenitore keyholder.
- **PR aperte**: #129 (zod) superata, main ha già zod 4.6.7, da chiudere; #144 (simulatore di chiamate) e #153 (primi passi nella console) restano aperte, da riprendere dopo.
- **Messaggio per Polina**: [[Polina - final setup]] (in inglese, come da regola 14).

