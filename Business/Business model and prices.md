---
tags: [business]
lang: it
source: documenti + decisioni di Giovanni (non nel codice, salvo i prezzi del sito)
---
# Modello di business e prezzi

## Modello [documenti]
**Nessuna società per ora.** Sigillo è software dato **in licenza a un partner**, che lo rivende in abbonamento (licenziante → partner → clienti). Decisione di Giovanni del 2026-11-09: se le aziende pretendono un fornitore con società (SRL), il modello cambierà; non si apre la società prima. La partner è **Polina** (business e vendite, non tecnica, vedi [[Partners and market]]): i guadagni si dividono con lei, intorno al 50%, quota da decidere dopo i piloti. **Un solo prodotto: "la prova"**, non il "controllo".

## Clienti target [documenti]
Software house e consulenti che costruiscono agenti AI in Python per finanza, assicurazioni, risorse umane. Prima Italia e Spagna.

## Prezzi (dal sito, `site-v2.ts`) [codice]
| piano | prezzo | contenuto |
|---|---|---|
| Pilot | gratis | 10.000 ricevute al mese, in cambio di caso di studio e feedback |
| Standard | 190 € al mese + IVA | 100.000 ricevute, sistemi illimitati, fascicoli automatici, copia esterna cifrata 30 giorni |
| Custom | su richiesta (da circa 490 €/mese nei documenti) | server dedicato, ente di marca a scelta, SLA |

Nel codice esiste solo la quota di 10.000 al mese per organizzazione: **limite 100.000 e fatturazione non costruiti** ([[Limits and throttling]]).

## Cosa manca per vendere [documenti]
Contratto di licenza, avvocato e commercialista, accettazione dei termini alla registrazione, file LICENSE, rapporto d'uso e fatturazione, livello "partner" nella console, marchio bianco, campo codice chiave in `/verify`, convenzione per le approvazioni umane, coda locale nell'SDK, pubblicazione su PyPI, conservazione automatica.

Vedi [[Partners and market]], [[Legal and compliance]].
