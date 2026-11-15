---
tags: [business, legal]
lang: it
source: documenti + pagine del sito (site-legal.ts, site-privacy.ts)
---
# Legale e conformità

## Termini e DPA
Pagine `/terms` e `/dpa` v1.0 del 2 novembre 2026, **provvisorie**, mai viste da un avvocato. Incoerenze con il codice (vedi [[Code defects]]): i Termini dicono copia del database "ogni due ore" (cron ogni ora, 2 copie tenute); Termini, DPA e privacy citano solo FreeTSA, non DigiCert; il sito dice "open source" ma nella repo **non c'è un file LICENSE** (solo l'SDK Python dichiara Apache-2.0).

## AI Act
Sigillo dà **prove** utili per gli articoli 12 (registrazione), 14 (sorveglianza umana), 26 (obblighi di chi usa il sistema); **non rende conformi**. Sanzioni fino a 15 milioni o 3% del fatturato. Date del "Digital Omnibus": [non verificato]. Il rischio più vicino per i clienti è il GDPR.

## eIDAS
Le marche temporali usate (FreeTSA, DigiCert) **non sono qualificate eIDAS**. Una qualificata richiede una società; decisione del 2026-09-21: aspettare.

## Parole
Usare "tamper-evident", "independently verifiable". Mai "tamper-proof", "compliant", "AI Act ready", "certified".

Vedi [[Data protection]], [[Security model]].

## Idea: uno standard nostro con valore legale? (2026-11-15)
Domanda di Giovanni. Risposta: no da solo. «Qualificato» dipende da chi emette (prestatore qualificato, QTSP: società, audit ogni 2 anni da ente accreditato, vigilanza AgID in Italia, iscrizione nella lista di fiducia UE; costo stimato centinaia di migliaia di euro e 1-2 anni), non dal formato. Effetto: presunzione legale di data e integrità (art. 41 eIDAS); una marca non qualificata resta prova ammissibile ma valutata dal giudice. Uno standard privato non ha presunzione; diventa ufficiale solo tramite ETSI/CEN/IETF (anni; all'IETF ci sono già bozze sulle ricevute degli agenti). eIDAS 2 (reg. 2024/1183) ha introdotto i **registri elettronici qualificati** (presunzione di autenticità e ordine cronologico dei dati), molto vicini a Sigillo; regole tecniche ancora in preparazione.
**Consiglio**: non diventare QTSP; proporre Sigillo in licenza a un QTSP (InfoCert, Namirial, Aruba) come «registro qualificato per agenti AI». Intanto usare marche qualificate comprate (vedi [[Prezzo giusto dei piani]]). Solo idea, nessuna decisione.

