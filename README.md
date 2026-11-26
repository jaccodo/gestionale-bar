# Gestionale Bar

App per telefono e PC con gli incassi giornalieri della società.

- **Ogni giorno**: Incasso1 (fiscale), di cui il parziale della mattina, Incasso2 (altra attività) e note. Il parziale mattina è sul totale dei due incassi; il pomeriggio è calcolato: Incasso1 + Incasso2 − mattina.
- **Spese (Incasso2)**: giorno, tipologia (a…g, nomi modificabili), nome della spesa e importo; totali per tipologia e Incasso2 al netto delle spese. Collezioni `bar_spese` e `bar_impostazioni/tipologie`.
- **Riepilogo del mese**: totale, Incasso1 e Incasso2 in euro e in %, mattina e pomeriggio in euro e in %, disparità in euro, in % rispetto all'altra fascia e in punti di quota; giorno migliore e più basso, medie, note del mese.

Accesso con Google, solo per i soci (lista `socio()` nelle regole). Dati in Firestore (progetto `le-mie-note-d18dd`), collezione `bar_giorni`: un documento per giorno con id `AAAA-MM-GG`.
Le regole sono in `../catalogo-app/firestore.rules` (blocco "Gestionale bar") e non vanno su GitHub.

Provarla sul PC: `node server.js` e apri
- http://localhost:5177/?prova  → dati finti, solo in quel browser
- http://localhost:5177         → app vera (login Google)

## Attivazione (una volta sola)
1. Console Firebase → Firestore → Regole: incolla tutto `catalogo-app/firestore.rules` e pubblica.
2. Per metterla online: repo GitHub con Pages, poi aggiungi il dominio in Firebase → Authentication → Settings → Domini autorizzati (jaccodo.github.io c'è già).
