---
tags: [overview]
lang: it
---
# Sigillo in breve (per Giovanni)

**Cos'è.** Un registro di prove per agenti AI. Ogni cosa che fa un agente (chiama un modello, usa uno strumento, decide) diventa una **ricevuta firmata**. Ogni ricevuta contiene l'impronta della precedente, quindi formano una **catena**: se qualcuno toglie, cambia o sposta una ricevuta, la catena si rompe e si vede.

**Come arrivano le azioni.** L'agente del cliente usa la nostra piccola libreria (SDK Python o TypeScript). La libreria **non manda mai i testi**, solo le loro impronte (hash) con un "sale", così nessuno può indovinarli. Manda anche un battito ogni minuto: se l'agente si spegne o si stacca, resta scritto. In alternativa l'agente può chiamare i modelli (OpenAI, Anthropic e altri 16) **passando da Sigillo**: allora la chiamata viene registrata prima che arrivi la risposta.

**Chi firma.** Il server riceve e salva, ma **non sa firmare**. La chiave di firma sta in un contenitore separato, il *firmatario*, senza accesso a internet, che tiene una sua copia della catena e rifiuta qualsiasi buco o riscrittura. Le chiavi dei fornitori di modelli stanno in un terzo contenitore, il *custode*.

**Il sigillo orario.** Ogni ora il firmatario firma un riassunto della catena (checkpoint) e un ente esterno (FreeTSA e DigiCert) ci mette una **marca temporale**: prova che a quell'ora la catena era così. Non sono marche "qualificate eIDAS".

**La prova per il cliente.** Dalla console il cliente scarica un **fascicolo** (zip con ricevute, sigilli, rapporto PDF). Chiunque lo controlla con il **verificatore** gratuito, senza fidarsi di noi. Si può anche provare che un documento o un testo è proprio quello usato dall'agente.

**Privacy.** Niente testi salvati, persone sostituite da pseudonimi, cancellazione GDPR possibile senza rompere la catena. Server in Italia (Aruba).

**Cosa non prova.** Che l'agente abbia registrato tutto se aggira Sigillo; che ciò che dice sia vero; l'ora dichiarata dall'agente.

**Dove siamo.** Il prodotto funziona (1713 test). Mancano: contratto con il partner, avvocato, fatturazione, licenza nella repo. Dettagli in [[Status and open items]], difetti trovati in [[Code defects]], mappa completa in [[00 Map]].
