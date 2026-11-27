# Gestionale Bar

App per telefono e PC con gli incassi giornalieri della società.

- **Ogni giorno**: Incasso1 (fiscale), di cui il parziale della mattina, Incasso2 (altra attività) e note. Il parziale mattina è sul totale dei due incassi; il pomeriggio è calcolato: Incasso1 + Incasso2 − mattina.
- **Spese (Incasso2)**: giorno, tipologia (Spesa, Frutta/verdura, Pasticceria, Ghiaccio, Giornali, Pasti/staff, Stipendio, Varie; rinominabili), nome della spesa e importo; totali per tipologia e Incasso2 al netto delle spese. Collezioni `bar_spese` e `bar_impostazioni/tipologie`.
- **Fatture (Incasso1)**: fatture ricevute importate da CSV (separatore ;, colonne tipo, numero, data emissione, identificativo e denominazione fornitore, imponibile, imposta, SDI), senza doppioni (id = SDI); note di credito in negativo. Mese, anno e anni con Incasso1 al netto delle fatture (con IVA) e ripartizione per fornitore. Collezione `bar_fatture`. Più avanti arriveranno da Cassanova (Cassa in Cloud).
- **Stipendi**: busta paga del mese con stipendio 1 (attività di Incasso1) e stipendio 2 (Incasso2) per ogni dipendente e per i soci Jacopo Bugli e Claudio Manenti, più l'F24 del mese. Anno contabile da dicembre a novembre. "Riepilogo stipendi" (solo dipendenti): totali, per persona con confronto con l'anno prima, grafico mese per mese, tabella. Collezioni `bar_stipendi` e `bar_persone`. Gli stipendi 2 (dipendenti e soci, senza F24) contano da soli tra le spese di Incasso2, tipologia Stipendio.
- **Riepilogo** (anno, mese o media al giorno dell'anno): Ricavi (Incasso1, Incasso2, totale); Costi commerciali in 6 voci (Servizi, Materie prime, Affitto, Commercialista, Generali, Operativi) divise in spese, fatture e totale; Costi personale (dipendenti stipendio 1, stipendio 2 e F24; soci stipendio 1 e 2); Utile dell'attività 1 al netto dell'IVA (Incasso1 scorporato dell'aliquota media, di base 10% e modificabile toccandola, − fatture all'imponibile − stipendi 1 − F24; aliquota in `bar_impostazioni/iva`) e dell'attività 2 (Incasso2 − spese − stipendi 2). "Quanto costa il bar al giorno": tutti i costi ÷ giorni di calendario con dati in memoria. Sotto: mattina e pomeriggio con la disparità, note, tabella di tutti i mesi.
- **Password dell'app** (dopo il login Google): la prima volta si sceglie, poi ogni dispositivo la ricorda finché non si preme Esci; si cambia dal menu in alto a destra. Salvata solo come impronta PBKDF2 in `bar_impostazioni/accesso`. Con la parola "password" si apre la vista ridotta: niente attività 2 (tutto ciò che ha il pallino arancione: Incasso2, Spese, stipendi 2) e niente mattina/pomeriggio; si può inserire e i dati nascosti restano com'erano. È solo un filtro di visualizzazione: i dati arrivano comunque sul dispositivo.
- **Domande**: un centinaio di domande pronte (incassi, spese, fatture, personale, bilancio) con risposta calcolata dall'app sul mese o sull'anno scelto in alto, e un filtro per cercare la domanda. Nessuna AI e nessun costo: i dati non escono dall'app. Nella vista ridotta le domande sull'attività 2 non compaiono.

Accesso con Google, solo per i soci (lista `socio()` nelle regole). Dati in Firestore (progetto `le-mie-note-d18dd`), collezione `bar_giorni`: un documento per giorno con id `AAAA-MM-GG`.
Le regole sono in `../catalogo-app/firestore.rules` (blocco "Gestionale bar") e non vanno su GitHub.

Provarla sul PC: `node server.js` e apri
- http://localhost:5177/?prova  → dati finti, solo in quel browser
- http://localhost:5177         → app vera (login Google)

## Attivazione (una volta sola)
1. Console Firebase → Firestore → Regole: incolla tutto `catalogo-app/firestore.rules` e pubblica.
2. Per metterla online: repo GitHub con Pages, poi aggiungi il dominio in Firebase → Authentication → Settings → Domini autorizzati (jaccodo.github.io c'è già).

## Backup
Ogni giorno verso le 4 lo script `catalogo-app/backup-drive/Bar.gs` (progetto Apps Script "Backup Catalogo") sovrascrive il foglio "Backup Gestionale Bar" nella cartella Drive BackUp_App, con le schede Giorni, Spese Incasso2 e Mesi.

## Simulazione
https://jaccodo.github.io/gestionale-bar/?prova apre una simulazione con dati finti (20 spese su tre mesi), solo nel browser: il database vero non viene toccato.
