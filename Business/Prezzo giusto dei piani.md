---
tags: [business, prezzi]
lang: it
source: ricerca del 2026-11-15 (web, fonti in fondo) + stime dichiarate come tali
---
# Prezzo giusto dei piani (proposta del 2026-11-15)

Richiesta di Giovanni: rifare i prezzi **da zero**, ignorando quelli attuali del sito ([[Business model and prices]]), partendo da costi di 10-30 € al mese, dalla dimensione del mercato, da quanto possono pagare i clienti e da quanto sono abituati agli abbonamenti. Solo ricerca, nessun codice.

## In breve
| piano | prezzo (IVA esclusa) | per chi | cosa include |
|---|---|---|---|
| **Prova** | gratis | chi sta provando | 1 sistema, 10.000 ricevute al mese, dati tenuti 30 giorni |
| **Starter** | **49 €/mese** (490 €/anno) | freelance o piccolo team con 1 agente da un cliente | 1 sistema in produzione, 50.000 ricevute, dati tenuti 12 mesi, fascicolo prove |
| **Professionale** | **149 €/mese** (1.490 €/anno) | software house con più clienti | fino a 5 sistemi, 250.000 ricevute, link per il revisore, supporto email; sistema in più 25 €/mese |
| **Regolato** | **da 390 €/mese**, solo annuale (~4.700 €/anno) | banca, assicurazione, cliente finale grande | sistemi illimitati fino a 1 milione di ricevute, server o chiave dedicati, SLA, DPA firmato, aiuto durante un audit |

Pagando l'anno: due mesi gratis. Il piano da spingere è **Professionale**.

## 1. I costi non decidono il prezzo
Il server costa 10-30 € al mese: anche un solo cliente Starter lo paga. I costi veri sono altri:
- **metà dei ricavi va alla partner** (circa 50%, [[Partners and market]]);
- il **tempo**: la chiamata di avvio di 20 minuti, il supporto, le domande dei revisori;
- commissioni di pagamento (circa 1,5-3%), commercialista, in futuro avvocato e assicurazione.

Quindi il prezzo si decide sul **valore per il cliente** e sui **prezzi dei concorrenti**, non sul costo. Regola pratica: dopo il 50% alla partner, un cliente deve lasciare abbastanza da ripagare il tempo che chiede. Per questo il piano più economico è quasi senza assistenza e l'assistenza vera parte dal Professionale.

## 2. Quanti clienti si possono raggiungere (stime)
Numeri ufficiali precisi non trovati: sono **stime a ordine di grandezza**.
- Nell'UE ci sono circa **1,3 milioni** di imprese di informazione e comunicazione (Eurostat, 2021); la parte più grande è "programmazione e consulenza informatica" (codice 62). Circa il 95% sono micro imprese o persone sole.
- Imprese del codice 62: **Italia circa 50.000** (dato 2019), **Spagna circa 42.000** (IBISWorld).
- Con almeno 10 dipendenti (quelle che hanno clienti in finanza, assicurazioni, HR e un minimo di budget): circa il 5%, cioè **2.000-3.000 in Italia, 2.000 in Spagna**, **35.000-40.000 in tutta l'UE**.
- Di queste, quelle che **costruiscono agenti AI in Python per settori regolati**: stima 5-10%. Nel settore ICT più del 60% delle imprese usa già l'AI (Eurostat), ma costruire agenti per banche e assicurazioni è una nicchia.

| | aziende nel profilo |
|---|---|
| Italia + Spagna | **circa 250-500** |
| tutta l'UE | **circa 2.000-4.000** |

**Cosa si può raggiungere davvero** con due persone e vendita tramite partner, nel primo anno: contattare 200-300 aziende, 20-30 prove, **5-15 clienti paganti**. Le prove gratuite B2B diventano clienti in circa il 10-18% dei casi (mediana 18,5%), e chi paga ogni mese se ne va al ritmo di circa il 3-5% al mese.

## 3. Quanto possono pagare e come comprano
- Una software house italiana o spagnola da 10-50 persone spende per uno strumento per sviluppatori di solito **30-300 € al mese**; fino a circa 150 € decide il titolare senza troppe discussioni.
- Gli strumenti simili che già usano (osservare gli agenti, non provare) costano:

| strumento | piano base a pagamento | piano "squadra" |
|---|---|---|
| Langfuse | 29 $/mese | 199 $/mese (+300 $ per SSO) |
| LangSmith | 39 $ per persona al mese | su richiesta |
| Arize Phoenix (AX Pro) | 50 $/mese | su richiesta |
| Helicone | 79-100 $/mese | 799 $/mese |
| Braintrust | gratis | 249 $/mese |

- Gli strumenti di **conformità** (Vanta, Drata) costano **7.500-12.000 $ all'anno** per una piccola azienda; quelli di **governance AI** (Credo AI) 30.000-250.000 $ all'anno. Questo è il tetto: chi compra per un audit è abituato a cifre molto più alte.
- Sigillo sta **in mezzo**: tecnicamente è vicino agli strumenti da 29-249 $, ma il valore è quello della conformità. Il prezzo giusto quindi è sopra Langfuse Core e sotto Vanta.
- **Abitudini**: le piccole aziende italiane preferiscono **fattura e pagamento annuale** alla carta di credito mensile. Il piano annuale con sconto aiuta anche la partner (incassa prima, meno abbandoni).
- **Leva importante**: la software house può **rifatturare** Sigillo al suo cliente finale ("registro delle prove dell'agente: 30-50 € al mese"). Per questo il prezzo è per **sistema** (un agente presso un cliente), un'unità facile da girare al cliente. Le ricevute sono solo un limite, non il motivo del prezzo.

## 4. Perché questi prezzi
- **Prova gratis**: senza prova gratuita nessuno sviluppatore prova uno strumento nuovo. 30 giorni di dati bastano per provare, non per lavorare davvero.
- **Starter 49 €**: alla pari con Phoenix e Helicone, sopra Langfuse Core. Abbastanza basso per una carta di credito senza chiedere a nessuno; abbastanza alto da non essere "giocattolo". Serve come porta d'ingresso.
- **Professionale 149 €**: sotto Langfuse Pro (199 $) e Braintrust (249 $), sotto la soglia in cui il titolare chiede preventivi. Con 5 sistemi sono **30 € a sistema**, facili da girare al cliente finale.
- **Regolato da 390 €**: meno della metà di un Vanta minimo (circa 600-1.000 € al mese). Qui compra una banca o un'assicurazione, che vuole contratto, DPA, SLA e qualcuno al telefono durante un audit. Sotto i 390 € il tempo richiesto non sarebbe ripagato dopo la quota della partner.

## 5. Quanto può rendere (scenari, prima della quota della partner)
| scenario | clienti | ricavo al mese | all'anno | a testa dopo il 50% |
|---|---|---|---|---|
| anno 1 realistico | 4 Starter, 5 Professionale, 1 Regolato | circa 1.330 € | circa 16.000 € | circa 8.000 € |
| anno 2-3 buono | 10 Starter, 25 Professionale, 5 Regolato | circa 6.200 € | circa 74.000 € | circa 37.000 € |

Onestamente: con il mercato che c'è oggi (250-500 aziende in Italia e Spagna) Sigillo può essere un buon reddito extra, non un'azienda grande, a meno di allargarsi a tutta l'UE o di vendere molti piani Regolati. Questo si collega al dubbio di Giovanni sul valore di Sigillo (decisione del 2026-11-13): i prezzi non cambiano la dimensione del mercato.

## Come procedere (consiglio del 2026-11-15)
Mandare la proposta a Polina prima della riunione come idea da decidere insieme (messaggio in inglese semplice); decidere alla riunione; cambiare il sito (`site-v2.ts`, via PR) solo dopo l'accordo e quando si aggiorna la produzione.

## Marca temporale qualificata eIDAS (domanda di Giovanni, 2026-11-15)
Oggi ogni ora, se ci sono ricevute nuove, il riassunto della catena di ogni sistema riceve una marca temporale **non qualificata** (FreeTSA, DigiCert, gratis). Una marca **qualificata eIDAS** ha valore legale in tutta l'UE: si presume vera la data e che i dati non siano cambiati dopo (art. 41 eIDAS). Prova **quando** esisteva il registro, non che l'agente abbia registrato tutto.
- **Costo di una marca** (listini, da ricontrollare): SK ID Solutions da 0,036 € a 0,006 € con volumi alti; Lleida.net da 0,03 €; Disig circa 0,10-0,12 € in pacchetti; rivenditori italiani (InfoCert, Aruba, Namirial) circa 0,15-0,99 €.
- **Quante ne servono**: al massimo 1 all'ora per sistema, cioè circa 720 al mese se l'agente lavora sempre, di solito molte meno (solo le ore con attività). Costo per sistema: circa 7-70 € al mese nel caso peggiore con i fornitori economici, pochi euro nel caso normale. Alternativa più economica (richiede codice): una sola marca all'ora per tutti i sistemi insieme, circa 720 marche al mese in totale.
- **Proposta**: non nello Starter (resta la marca gratuita); **inclusa nel Regolato**, ogni ora; per il Professionale come **aggiunta a 29 €/mese** per sistema.
- **Ostacoli**: i fornitori qualificati di solito vogliono un contratto con un'azienda (decisione del 2026-09-21: aspettare la società); il codice è fermo e oggi parla solo con FreeTSA e DigiCert; va scelta un'autorità e provata. Era tra i punti sospesi ([[Working rules]], regola 10): è Giovanni a riaprirlo il 2026-11-15, solo come idea di prezzo.

## 6. Cosa manca per vendere questi piani [codice]
Oggi nel codice c'è solo la quota di 10.000 ricevute al mese per organizzazione ([[Limits and throttling]]). Mancano: limiti per piano e per numero di sistemi, conservazione automatica a 12 mesi, fatturazione, link per il revisore ([[Quale agente ha fatto il danno]]). Il codice è fermo, quindi all'inizio i piani si gestiscono a mano (fattura annuale, limiti impostati dall'amministratore).

## Fonti
- Eurostat, imprese del settore informazione e comunicazione: https://ec.europa.eu/eurostat/statistics-explained/index.php/Businesses_in_the_information_and_communication_services_sector
- Eurostat, 20% delle imprese UE usa l'AI (2025): https://ec.europa.eu/eurostat/web/products-eurostat-news/w/ddn-20251211-2
- Imprese codice 62 in Italia (aggregatore, 2019): https://d3fqfiq2w1ez7u.cloudfront.net/nmx/timeseries/italy-number-of-enterprises-of-computer-programming-and-consultancy
- IBISWorld Spagna: https://www.ibisworld.com/spain/industry/computer-programming-consultancy-and-related-activities/200076/
- Langfuse: https://langfuse.com/pricing
- LangSmith: https://inference.net/content/langsmith-pricing/
- Braintrust, Phoenix, Helicone: https://costbench.com/software/ai-observability/
- Vanta e Drata: https://soc2auditors.org/insights/vanta-pricing/ e https://soc2auditors.org/insights/drata-pricing/
- Credo AI: https://www.techjockey.com/detail/credo-ai
- Conversione delle prove gratuite: https://www.poweredbysearch.com/learn/b2b-saas-trial-conversion-rate-benchmarks/

Molti prezzi vengono da siti di confronto, non dalle pagine ufficiali: vanno ricontrollati prima di usarli con i clienti. I prezzi dei concorrenti diretti (Elydora, Agnys, Sigill.ai) non sono stati trovati.
