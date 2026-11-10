# Regole di lavoro con Giovanni

[DOCUMENTI] Giovanni Noventa è il proprietario, non tecnico, italiano.

1. Risposte e aggiornamenti a lui in **italiano**, semplici e brevi. Codice, commenti, commit e documentazione tecnica nella repo in **inglese**. Testi per clienti e partner in inglese.
2. Niente piani lunghi in file salvo che li chieda lui. Si spiega in chat in poche righe, si implementa dopo il suo OK. Se ha già detto di procedere, si procede.
3. Istruzioni da copiare e incollare: piccole, un passo alla volta. Dal telefono usa Android.
4. Le novità vanno direttamente a tutti gli account (regola "prima all'amministratore" sospesa dal 2026-10-20 finché non ci sono i primi clienti).
5. Dopo ogni merge il riavvio dello stack sul server lo fa lui.
6. Niente esce verso l'esterno senza il suo OK: email, pubblicazioni (PyPI, crates.io), nuove dipendenze fuori dalla lista approvata.
7. Mai rispondere a memoria sui fatti importanti.
8. Parole da usare: "tamper-evident", "independently verifiable". Parole da **non usare mai**: "tamper-proof", "compliant", "AI Act ready", "certified". Sigillo dà prove, non conformità.
9. Giovanni ha rifiutato test o "canarini" mostrati ai clienti dentro la console. Non riproporli.
10. Sospesi, da non riproporre: cassaforte delle chiavi, SDK Rust, caricamento Java/.NET, prodotto self-hosted, marca temporale qualificata eIDAS.
11. Un solo prodotto: "la prova". Non allargarsi al "controllo".
12. Regole della repo del codice (CLAUDE.md): test prima per il codice crittografico; mai salvare testo in chiaro; mai cambiare il formato della ricevuta senza aumentare `v`; `packages/core` resta puro; nessuna dipendenza fuori lista; mai push diretto su `main` della repo del codice.
