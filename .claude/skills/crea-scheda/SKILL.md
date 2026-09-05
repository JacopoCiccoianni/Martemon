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
| #004-#006 | Starter Erba (riservato, da inventare) |
| #007-#009 | Starter Fuoco (riservato, da inventare) |
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
contenuti; **non toccare `style.css`** e non aggiungere `<style>` nella pagina. I colori dei 18
tipi esistono già come `.t-<tipo>` (acqua, terra, erba, fuoco, elettro, ghiaccio, acciaio, veleno,
roccia, normale, lotta, psico, volante, spettro, buio, drago, folletto, coleottero).

Il nome file è il nome del Pokémon in minuscolo, senza accenti né apostrofi (`Nutrì` → `nutri`).

## 2. Anatomia della pagina (ordine obbligatorio)

1. `<title>NOME - Pokédex Martemon</title>`, `lang="it"`, `<link rel="stylesheet" href="style.css">`.
   Scrivere la bozza già con i percorsi «da radice» (`style.css`, `img/…`): per l'anteprima
   basta copiarla temporaneamente alla radice, e alla pubblicazione non c'è nulla da riscrivere.
2. `.top` identica alle altre schede.
3. `<p class="notice">` identica alle altre schede.
4. `.pagenav` (in alto): `← link al precedente` · `<b>#0NN Nome</b>` · `successivo →`. Se il
   vicino non esiste ancora: `<span class="gap">#0NN: specie non ancora catalogata</span>`. Il
   precedente della #001 e il successivo dell'ultimo puntano a `index.html` («Indice»).
5. `.infobox`:
   - `.head`: `Pokédex Martemon #0NN<b>Nome</b>Pokémon <Categoria>`
   - `<figure><img src="img/<nome>.png" alt="Artwork di NOME"><figcaption>Artwork di NOME</figcaption></figure>`
   - `.names`: `EN: … · JA: カタカナ <i>Romaji</i>`
   - `<table>` con le righe in quest'ordine: Tipo, Abilità (prima<br>*speciale*), Sesso, Altezza,
     Peso, Tasso di cattura (valore e percentuale), Uovo (gruppi<br>cicli e passi), Pokédex
     regionali, Tasso di allevamento, Esperienza base ceduta, Punti base ceduti, Colore Pokédex,
     Affetto di base.
6. `<h1>Nome</h1>` + due `<p>` di apertura: tipo e ruolo («starter», «pseudo-leggendario»…), poi
   evoluzioni con link.
7. `.toc` con anchor **identici** agli `id` delle sezioni.
8. `<h2 id="Biologia">` con `<h3 id="Fisionomia">` (+ `<h4>Differenze tra i sessi</h4>`),
   `<h3 id="Comportamento">`, `<h3 id="Habitat">`, `<h3 id="Dieta">`.
9. `<h2 id="Dati">Dati di gioco` con:
   - `<h3 id="Resistenze">` — `.eff` con `<div>` Debolezze / Resistenze / (Immunità se ci sono),
     badge `.type` + `<small>2×</small>`, `½×`, `¼×`, `4×`, `0×`. **Calcolare** dalla type chart.
   - `<h3 id="Evoluzioni">` — `.evo` con `.stage` (img + nome + tipi) e `.arrow` (testo =
     condizione). Specie singola: una sola `.stage` + frase «non si evolve».
   - `<h3 id="Statistiche">` — `table.stats`, 6 righe + `tr.tot`. Larghezza barra =
     `round(valore/150·100)%`. Segue un `<p>` di lettura del ruolo competitivo.
   - `<h3 id="Mosse">` — `<h4>Aumentando di livello</h4>` (Lv./Mossa/Tipo/Cat./Pot./Prec./PP) e
     `<h4>Tramite MT/DT (selezione)</h4>`; mosse con STAB in **grassetto**; `<p>` che spiega il
     grassetto e la mossa di famiglia. Le mosse inventate (es. *Michettata*) si descrivono lì.
   - opzionale `<h3 id="NomeMossa">` per una **mossa esclusiva** dello stadio (come *Franargine*).
   - `<h3 id="Voci">` — `.dex` con due `<p>`: `<b>Versione Lambro</b>` e `<b>Versione Martesana</b>`,
     testo in corsivo, tono da Pokédex, una-due frasi.
10. `<h2 id="<Luogo>">` — sezione di luogo con `.place`: un posto **reale** (Parco Lambro, alzaia,
    Cassina de' Pomm, Villa Finzi, Naviglio a Crescenzago…) raccontato con precisione e affetto.
11. `<h2 id="Curiosita">` — `<ul>` di 3 curiosità + `<h3>Origine</h3>` (riferimenti reali: animale,
    storia, dialetto milanese) + `<h4>Origine del nome</h4>` (etimologia IT e JA).
12. `<h2 id="Lingue">` — `table.langs` Lingua/Nome/Origine (Giapponese, Inglese, Francese,
    Spagnolo, Tedesco).
13. `<h2>Vedi anche</h2>` — link relativi alle schede correlate.
14. `.pagenav` in basso, identica a quella in alto.
15. `.cats` — «Pokémon · Pokémon della regione Martemon · [Pokémon iniziali] · Pokémon di tipo X ·
    Gruppi Uova … · Pokémon di colore …».
16. `<footer>` — link al repo, nota fan-made, frase sul luogo reale citato.

## 3. Artwork

### Caso A — immagine fornita
Salvare in `img/<nome>.png`. Se pesa più di ~800 KB o supera 1408 px sul lato lungo, ridurla
(PowerShell System.Drawing o strumento disponibile). Sfondo bianco, soggetto centrato, come le
tre esistenti. La stessa immagine serve anche nelle `.evo .stage` (il CSS la ritaglia a cerchio).

### Caso B — nessuna immagine
Lasciare gli `<img>` con `src="img/<nome>.png"` e `alt` corretto e dirlo esplicitamente a
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
- Le due `.pagenav` identiche e con numeri e link giusti.
- Aperta nel browser accanto a `nutri.html`: stesso aspetto.

Chiudere ricordando: per metterla online, `/pubblica-scheda`.
