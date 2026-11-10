# Privacy e sicurezza

## Regola di base

Sigillo **non salva mai il testo** di prompt, argomenti o risposte, solo impronte SHA-256 (regola permanente). Si può dire "non conserviamo mai il contenuto" per la versione ospitata. "Non conserviamo niente" solo per un'installazione gestita dal partner o dal cliente. [DOCUMENTI]

## Cosa si conserva [CODICE + docs]

Etichette (strumenti, agenti, modelli), tipo di azione, esito, orari, email e nome azienda dell'account, pseudonimi `psn_...`, impronte dei documenti (SHA-256 semplice, non protette dalla chiave del cliente). Con il gateway il contenuto passa in memoria e non resta. Log: una riga JSON per richiesta, senza query né corpo. Comandi: `sigillo-server privacy inventory` e `privacy scan`.

## Cancellazione

Un sistema con ricevute oltre la genesis non si cancella, si archivia. Fine contratto: `sigillo-server org erase`. Una persona: `subject erase`. I backup tengono i dati cancellati fino alla rotazione. **Non esiste conservazione automatica con scadenza** (proposta 6 mesi pilota, 12 Standard in `docs/PROPOSTA-CONSERVAZIONE.md`, non costruita).

## Accessi

Chiavi API in SHA-256 (dal 2026-11-07). Amministratore: password di almeno 12 caratteri, blocchi progressivi, secondo fattore TOTP facoltativo (se attivo in produzione: [NON VERIFICATO]). Sessione con cookie HttpOnly, SameSite=Strict, CSRF. Account clienti via Firebase, i nuovi attendono l'approvazione dell'operatore. Quote: 10.000 ricevute al mese per organizzazione, 3000 al minuto per sistema, 100 sistemi per organizzazione. Caddy: CSP stretta, HSTS, X-Frame-Options DENY.

## Audit del 2026-11-07

Corretto nella PR #145 (unita): custode delle chiavi separato, verificatore su GitHub con SHA-256, seconda marca temporale, guardiano esteso, allarme `tool_unrecorded`, limiti per l'amministratore, nomi riservati. **Ancora aperto**:

1. Porta del modello locale senza autenticazione (serve l'OK di Giovanni per la correzione).
2. Una sola chiave Sigillo per sistema, in chiaro nel file dell'agente.
3. Modifica dell'SDK dentro il processo spegne la registrazione.
4. Cose che deve fare Giovanni sul server: SSH solo con chiave, togliere l'aggiornamento automatico come root, copie fuori sede non cancellabili.
5. Revocare la chiave di prova dell'agente di audit.

Mai fatti: revisione indipendente del trattamento dati, penetration test.
