# CLAUDE.md

Guida per Claude Code su questo repository. Caricata a ogni sessione.

## Di cosa si tratta

**Martemon** è un Pokédex fan-made, in italiano, di Pokémon immaginari legati ai luoghi, alle
abitudini e ai personaggi del **nord-est di Milano**: il Lambro e il suo parco, il Naviglio
Martesana e l'alzaia, Gorla, Crescenzago, Cimiano, Cassina de' Pomm, Vimodrone e dintorni.
Il formato è quello enciclopedico di Pokémon Central Wiki.

È il fratello milanese di **Wikascolemon** (`../Wikascolemon/`, il Pokédex del Piceno): stessa
idea, stesso tono, ma struttura tecnica più semplice. Quando serve ispirazione su come si
scrive una scheda (biologia, voci Pokédex, curiosità, origine del nome) guardare lì; quando
serve la struttura HTML guardare **qui**, perché i due template non sono intercambiabili.

## Principi di lavoro

- **Pensare prima di scrivere.** Se un concept è ambiguo, proporre e chiedere; non scegliere in silenzio.
- **Minimo che funziona.** Niente build tool, framework, astrazioni o configurabilità non richieste.
- **Modifiche chirurgiche.** Non «migliorare» pagine vicine, non riformattare, imitare lo stile esistente.
- **Verificare.** Ogni scheda si controlla aprendola nel browser e con i controlli in fondo alle skill.

## Stack e struttura

Sito **statico puro**: HTML + un solo `style.css` condiviso. Nessuna dipendenza, nessun JS.
La radice del repo **è** il sito pubblicato (GitHub Pages dal branch `main`, cartella `/`).

```
index.html          Pokédex Martemon (card) + indice rapido (tabella)
<nome>.html         una scheda per specie, nome del Pokémon in minuscolo senza accenti (nutri.html)
style.css           stile condiviso in stile Pokémon Central: tipi (.t-<tipo>), accenti per pagina (body.<tipi>), infobox, barre
img/<nome>.png      artwork della specie (file esterno, NON base64 come in Wikascolemon)
_bozze/             schede in lavorazione, non ancora linkate dall'indice
.nojekyll           Pages serve i file così come sono (niente Jekyll)
.claude/skills/     crea-scheda (scrive la bozza) · pubblica-scheda (la integra e la mette online)
```

Differenze deliberate rispetto a Wikascolemon, da **non** riportare indietro:
- CSS esterno condiviso, non inline per pagina. Un colore di tipo si cambia in un posto solo.
- Artwork come file in `img/`, non incorporato. Le pagine restano leggibili e piccole.
- Un solo livello di cartelle: niente `schede _Pokemon/` separata dal sito.

## Flusso: bozza → pubblicata

1. `/crea-scheda` scrive `_bozze/<nome>.html` e salva l'artwork in `img/<nome>.png` (o segnala che manca).
2. Jacopo la legge e la corregge.
3. `/pubblica-scheda` sposta la pagina alla radice, aggiorna la navigazione delle schede
   adiacenti, `index.html` e `README.md`, poi committa e pusha su `main`.

Deploy = push su `main`. Sito: https://jacopociccoianni.github.io/Martemon/

## Pokédex Martemon: numerazione semantica

C'è **un solo dex**. I numeri sono **semantici, non progressivi**: i blocchi riservati non si
occupano con altro, e i buchi sono voluti. Mai assegnare «il più alto + 1» senza guardare la
tabella, che vive nella skill `crea-scheda` ed è l'unica autorevole. Riassunto:

| Blocco | Destinazione |
|---|---|
| #001-#003 | Starter Acqua: Nutrì → Nutriotto → Sciutria ✅ |
| #004-#006 | Starter Erba: Ortighetto → Ortica → Palortica ✅ |
| #007-#009 | Starter Fuoco: Bracina → Ghisandra → Salambretta ✅ |
| #010-#011 | Aironcello → Martesairone ✅ (l'airone della Martesana) |
| #012 in su | Specie libere, primo numero libero coerente col concept |

Quando una scheda viene pubblicata, aggiornare la tabella nella skill.

## Convenzioni delle schede

- Clonare **sempre** la struttura di una scheda esistente (`nutri.html` è il modello; `sciutria.html`, `palortica.html` e
  `salambretta.html` mostrano una specie a due tipi con una mossa esclusiva). Stesse classi, stesso ordine di sezioni.
- Ogni pagina dichiara `<body class="<tipi>">` e `style.css` ha la riga `body.<tipi>{--accent1;--accent2}`
  corrispondente: è il solo punto in cui una scheda nuova tocca il CSS.
- Design rifatto il 05/09/2026 sul modello di Pokémon Central (`.dexnav`, `.infobox`, `.effbox`,
  `.evochain`, `.statbars`, `.wtable`): le vecchie classi `.pagenav`/`.eff`/`.evo`/`.stats` non esistono più.
- Voci Pokédex nelle due versioni immaginarie: **Versione Lambro** e **Versione Martesana**.
- Ogni scheda ha una sezione «di luogo» (un `.notabene`) su un posto **reale** del quartiere, scritto
  con affetto e precisione: è la parte che rende il progetto locale e non generico.
- Categorie di mossa con lo **split fisico/speciale moderno**, per mossa. Le statistiche sono
  pensate su questa base.
- Barre delle statistiche: larghezza `round(valore / 150 · 100)%`.
- Efficacie dei tipi **calcolate** dalla type chart per la combinazione reale, non copiate.
- Link solo relativi e solo tra pagine locali; niente asset esterni, niente CDN.
- Locale italiano ovunque: accenti veri, «à» non «a'», virgola decimale (0,3 m · 4,5 kg).

## Tono

Enciclopedico ma affettuoso, con l'ironia di chi il quartiere lo vive: la sciura sull'alzaia,
il pane tirato alle nutrie, la Cassina de' Pomm. Le curiosità e l'«Origine» citano cose vere
(animali, luoghi, storia, dialetto). Niente battute sopra le righe, niente riferimenti che un
lettore di fuori non potrebbe verificare.

## Dipendenze esterne

- Repo: https://github.com/JacopoCiccoianni/Martemon
- Sito: https://jacopociccoianni.github.io/Martemon/ (Pages da `main`, cartella radice)
