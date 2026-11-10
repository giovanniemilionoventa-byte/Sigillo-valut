# Sigillo: seconda memoria

Questa è la memoria esterna di Sigillo. Serve a un'intelligenza artificiale (o a una persona) che deve capire tutto del progetto partendo da zero. È divisa in note brevi, una per argomento. Non copia la repo del codice: riassume ciò che conta e dice dove guardare.

Repo del codice: `giovanniemilionoventa-byte/sigillo`. Il codice vince sempre su queste note.

## Come leggere (per un'AI)

1. Leggi questo file, poi [[00 Map]] (mappa con schemi) e [[Panoramica]] (riassunto in italiano).
2. Note tecniche in inglese, divise in cartelle: `Concepts/` (formato e crittografia), `Components/` (pezzi del codice), `Flows/` (percorsi con diagrammi), `Operations/` (installazione, configurazione, sicurezza, privacy).
3. `Business/` (in italiano): modello, prezzi, legale, partner, regole di lavoro, stato e cose aperte.
4. `Reports/Code defects.md`: difetti trovati nel codice il 2026-11-10.
5. `Registro/registro-lavori.md`: cosa è stato fatto davvero, giorno per giorno.

Le note tecniche sono state riscritte il 2026-11-10 leggendo **solo il codice**; dove un fatto viene dai documenti è segnato *[docs]* o *[documenti]*. Sui fatti importanti (privacy, dati, prezzi, legale, server) non rispondere a memoria. Nessuna chiave, password o segreto va scritto qui.

## Regola di aggiornamento (obbligatoria, decisa da Giovanni il 2026-11-10)

Questo vault è la **memoria fondante** di Sigillo, un secondo cervello: ogni AI che lavora sul progetto lo legge prima di rispondere e lo tiene aggiornato.

1. **Si aggiorna sempre, in automatico, senza chiedere a Giovanni.** Ogni cambiamento, aggiunta o novità va qui: una decisione (es. "rendiamo il prodotto open source"), un nuovo cliente o partner, un incontro, un prezzo, una PR unita, una cosa messa online, un fatto che cambia.
2. **Si modifica la nota giusta**, non si accumula: se un fatto cambia, si corregge dove è scritto (e si toglie quello vecchio). Si crea una nota nuova se l'argomento non esiste (es. una nota per cliente in `Business/Clienti/`), collegata con `[[link]]` alla mappa [[00 Map]].
3. **Si aggiunge una riga in `Registro/registro-lavori.md`** con ciò che è stato fatto o deciso davvero (non idee).
4. Le idee non ancora decise vanno in `Business/Status and open items.md` come "aperto".
5. **Push diretto su `main` di questo repo, senza pull request.** Solo il repo del codice (`sigillo`) usa le pull request.
6. Mai cancellare il vault in blocco; mai scrivere chiavi, password o segreti.

Ultimo aggiornamento: 2026-11-10.
