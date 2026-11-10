# Il prodotto

Sigillo è un **registro di prove per agenti AI**. Ogni azione di un agente (chiamata a un modello, uso di uno strumento, passo, decisione) diventa una **ricevuta firmata**, legata alla precedente in una catena. Ogni ora la catena viene sigillata da un **checkpoint** con una **marca temporale** di un ente esterno. Il cliente scarica un **pacchetto di prove** (zip) e chiunque lo controlla con il **verificatore** indipendente, senza fidarsi di Sigillo.

Frase del sito: "Prove what your AI did. When it matters most." Dominio `get-sigillo.eu`, contatto `team@get-sigillo.eu`.

Per chi: software house e consulenti che costruiscono agenti in Python per finanza, assicurazioni, risorse umane. Prima Italia e Spagna. [DOCUMENTI]

## Cosa dimostra

- Una ricevuta non è stata modificata dopo la firma.
- Nessuna ricevuta è stata tolta, aggiunta o riordinata nella catena.
- La catena esisteva in quello stato a una certa ora (marca temporale esterna).
- Un documento o un testo corrisponde all'impronta registrata (comandi `doc` e `open` del verificatore).

## Cosa NON dimostra (da dire sempre)

- Che la registrazione sia completa: un agente che aggira il gateway o modifica l'SDK nel proprio processo non viene registrato.
- Che ciò che l'agente dichiara sia vero.
- L'esito reale di un'azione esterna.
- L'ora dichiarata dall'agente (`ts_event`): non è fidata, lo è l'ora di ricezione.
- Le marche temporali (FreeTSA, DigiCert) **non sono qualificate eIDAS**.
- Se il server è irraggiungibile per più di circa 10 secondi, le azioni di quel periodo si perdono (nessuna coda locale).
- Una coda di catena tagliata si scopre solo con `--previous`.
- La proprietà della chiave di firma si sa solo passando `--key-id`.
- Un server compromesso può aggiungere ricevute false in fondo, non riscrivere il passato.

## Concorrenza [DOCUMENTI, ottobre 2026]

Elydora, Agnys, Sigill.ai vendono anche loro ricevute firmate. Vantaggio di Sigillo: registra l'agente così com'è, l'agente non tiene la chiave né può zittire il registratore, guardia per modelli cloud e locali, collegamento in un passo, taglio europeo.
