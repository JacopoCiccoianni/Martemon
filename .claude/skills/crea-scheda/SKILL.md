---
name: crea-scheda
description: Crea una nuova scheda Pokémon per Martemon (bozza in "_bozze/"), clonando il template delle schede esistenti e salvando l'artwork in img/. Gestisce sia il caso con immagine fornita sia senza (segnaposto in attesa dell'artwork). Usare quando l'utente chiede di creare/inventare/scrivere una nuova scheda Pokémon del nord-est di Milano, o quando fornisce l'immagine per una scheda già creata senza artwork.
---

# Crea scheda Pokémon (Martemon)

Crea una **bozza** in `_bozze/<nome>.html`. La pubblicazione (navigazione, indice, README, push) è
compito della skill `pubblica-scheda`.

## 0. Raccogli i dati

Dal prompt servono almeno: **nome**, **concept** (animale, oggetto, personaggio, abitudine) e
**luogo del nord-est di Milano** di riferimento. Se manca uno dei tre, chiedere. Tutto il resto
(tipi, categoria, statistiche, mosse, lore) si inventa in modo coerente col concept: proporre
e procedere, Jacopo corregge.

Determinare il **numero di Pokédex**. Il dex è uno solo, la numerazione è **semantica**:

| Blocco | Destinazione |
|---|---|
| #001-#003 | Starter Acqua: Nutrì → Nutriotto → Sciutria ✅ (Lambro → Martesana → Gorla) |
| #004-#006 | Starter Erba: Ortighetto → Ortica → Palortica ✅ (ferrovia di Lambrate → quartiere Ortica → Cascina Sant'Ambrogio) |
| #007-#009 | Starter Fuoco: Bracina → Ghisandra → Salambretta ✅ (ex Innocenti → stazione di Lambrate → cavalcavia) |
| #010 in su | Libero |

Regola: scansionare `*.html` alla radice e in `_bozze/` per i numeri già presi, e assegnare il
**primo numero libero coerente col concept** (una linea starter va nel suo blocco; una linea
evolutiva occupa numeri consecutivi; una specie qualsiasi prende il primo libero da #010).
Mai «il più alto + 1» senza guardare la tabella. Dopo la pubblicazione, **aggiornare questa
tabella** con la riga nuova e la spunta ✅.

⚠️ Prima di assegnare, `git fetch origin` e controllare che su `origin/main` il numero sia
ancora libero: se lavorano più sessioni, la tabella locale invecchia.

## 1. Clona il template

Base: `nutri.html` (specie a un tipo, primo stadio) oppure `sciutria.html` (due tipi, stadio
finale, mossa esclusiva con la sua sotto-sezione). Copiare il file intero e sostituire i
contenuti; non aggiungere `<style>` nella pagina. Le classi `.t-<tipo>` dei 18 tipi esistono già
in `style.css`: acqua, terra, erba, fuoco, elettro, ghiaccio, acciaio, veleno, roccia, normale,
lotta, psico, volante, spettro, buio, drago, folletto e **`t-coleot`** (non `t-coleottero`).

**Colore di pagina.** Ogni scheda ha `<body class="<tipo>">` o `<body class="<tipo1>-<tipo2>">`
(es. `acqua`, `acqua-terra`): la classe imposta `--accent1`/`--accent2`, i due colori del
gradiente di infobox, tabelle e card. Se la combinazione non esiste ancora in `style.css`,
**aggiungere una riga** accanto a `body.acqua{...}`, con accent1 = colore del primo tipo e
accent2 = una tinta del secondo (o una variante più scura del primo). È l'unica modifica
ammessa a `style.css` per una scheda nuova.

Il nome file è il nome del Pokémon in minuscolo, senza accenti né apostrofi (`Nutrì` → `nutri`).

## 2. Anatomia della pagina (ordine obbligatorio)

Tutto dentro `<div class="page">`. Scrivere la bozza già con i percorsi «da radice»
(`style.css`, `img/…`): per l'anteprima basta copiarla temporaneamente alla radice.

1. `<title>NOME - Pokédex Martemon</title>`, `lang="it"`, `<link rel="stylesheet" href="style.css">`,
   `<body class="<tipi>">`.
2. `.sitenotice` identica alle altre schede.
3. `.dexnav` (in alto): `<span>← <a>precedente</a></span><span class="mid">#0NN Nome</span><span><a>successivo</a> →</span>`.
   Se il vicino non esiste ancora: `<span class="gap">#0NN: ??? →</span>`. Il precedente della
   #001 e il successivo dell'ultimo puntano a `index.html` («Indice»).
4. `<h1>Nome</h1>`, **prima** dell'infobox.
5. `.infobox`:
   - `.head` con `.num` («Pokédex Martemon #0NN»), `.name`, `.cat` («Pokémon <Categoria>»)
   - `.art`: `<img src="img/<nome>.png" alt="Artwork di NOME">` + `<div class="capt">Artwork di NOME</div>`
   - `.lang`: `<span>EN: …</span><span>JA: カタカナ <i>Romaji</i></span>`
   - `<table>` con le righe in quest'ordine: Tipo, Abilità (prima<br>*speciale*), Sesso, Altezza,
     Peso, Tasso di cattura (valore e percentuale), Uovo (gruppi<br>cicli e passi), Pokédex
     regionali, Tasso di allevamento, Esperienza base ceduta (`td.num`), Punti base ceduti,
     Colore Pokédex, Affetto di base (`td.num`).
6. `<p class="intro"><b class="pkmn">Nome</b> è un Pokémon di tipo …` + un `<p>` sulle evoluzioni con link.
7. `.toc` con `.toctitle` e anchor **identici** agli `id` delle sezioni.
8. `<h2 id="Biologia">` con `<h3 id="Fisionomia">` (+ `<h4>Differenze tra i sessi</h4>`),
   `<h3 id="Comportamento">`, `<h3 id="Habitat">`, `<h3 id="Dieta">`.
9. `<h2 id="Dati">Dati di gioco` con:
   - `<h3 id="Resistenze">` — `.effbox` di `.effrow` (`.efflabel` Debolezze / Resistenze / Immunità,
     `.effcells` con badge `.type` + `<span class="effx">2×</span>`, `½×`, `¼×`, `4×`, `0×`).
     **Calcolare** dalla type chart. Segue un `<p>` di commento.
   - `<h3 id="Evoluzioni">` — `.evochain` di `.evocard` (img + `.nm` + tipi) e `.evoarrow`
     (`<div class="ar">→</div><div>Livello 17</div>`). Specie singola: una sola card + frase «non si evolve».
   - `<h3 id="Statistiche">` — `.statbars` con 6 `.statrow` (classi `s-ps s-att s-dif s-asp s-dsp s-vel`;
     dentro `.sn` nome, `.sv` valore, `.sb` con `<i style="width:NN%">`) + riga Totale con
     `.sb` a `visibility:hidden`. Larghezza = `round(valore/150·100)%`. Segue un `<p>` di lettura
     del ruolo competitivo.
   - `<h3 id="Mosse">` — `<h4>Aumentando di livello</h4>` e `<h4>Tramite MT/DT (selezione)</h4>`,
     tabelle `.wtable` (Lv./Mossa/Tipo/Cat./Pot./Prec./PP; `td.lvl` sul livello, `td.num` sui
     numeri, `td.stab` sulle mosse col bonus di tipo). `<p>` che spiega il grassetto e la mossa
     di famiglia (es. *Michettata*).
   - opzionale `<h3 id="NomeMossa">` + `.notabene` per una **mossa esclusiva** (come *Franargine*).
   - `<h3 id="Voci">` — `.dexentries` con due `<p>`: `<span class="ver">Versione Lambro</span>` e
     `<span class="ver">Versione Martesana</span>`, testo in corsivo, una-due frasi.
10. `<h2 id="<Luogo>">` — sezione di luogo in un `.notabene`: un posto **reale** (Parco Lambro,
    alzaia, Cassina de' Pomm, quartiere Ortica…) raccontato con precisione e affetto.
11. `<h2 id="Curiosita">` — `<ul>` di 3 curiosità + `<h3>Origine</h3>` (riferimenti reali) +
    `<h4>Origine del nome</h4>` (etimologia IT e JA).
12. `<h2 id="Lingue">` — `.wtable` Lingua/Nome/Origine (Giapponese, Inglese, Francese, Spagnolo,
    Tedesco), `td.stab` sulla lingua.
13. `<h2>Vedi anche</h2>` — link relativi alle schede correlate.
14. `.dexnav` in basso, identica a quella in alto.
15. `.cats` — «Pokémon · Pokémon della regione Martemon · [Pokémon iniziali] · Pokémon di tipo X ·
    Gruppi Uova … · Pokémon di colore …».
16. `.footer` — link al repo, nota fan-made, frase sul luogo reale citato.

## 3. Artwork

### Caso A — immagine fornita
Salvare in `img/<nome>.png`. Se pesa più di ~800 KB o supera 1408 px sul lato lungo, ridurla
(PowerShell System.Drawing o strumento disponibile). Sfondo bianco, soggetto centrato, come le
tre esistenti. La stessa immagine serve anche nelle `.evocard` della catena evolutiva.

### Caso B — nessuna immagine
Lasciare gli `<img>` con `src="img/<nome>.png"` e `alt` corretto (in `.art` e in `.evocard`) e dirlo esplicitamente a
Jacopo: la scheda è pronta ma senza artwork, e `pubblica-scheda` chiederà conferma prima di
mandarla online così.

### Caso C — arriva l'immagine per una scheda già scritta
Salvare il file in `img/<nome>.png` e basta: la pagina lo trova da sola. Se la scheda è già
online, serve un commit + push.

## 4. Verifica prima di consegnare

- Nessun `src`/`href` verso l'esterno (solo `style.css`, `img/…`, pagine locali, anchor, il link
  GitHub nel footer).
- UTF-8 e accenti veri.
- Anchor del `.toc` = `id` delle sezioni.
- Barre coerenti coi valori; totale = somma delle sei statistiche.
- Efficacie giuste per la combinazione di tipi.
- Le due `.dexnav` identiche e con numeri e link giusti.
- `body` con la classe del tipo, e la riga `body.<tipi>{...}` presente in `style.css`.
- Aperta nel browser accanto a `nutri.html`: stesso aspetto.

Chiudere ricordando: per metterla online, `/pubblica-scheda`.
