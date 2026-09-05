---
name: pubblica-scheda
description: Pubblica una scheda Pokémon già pronta su Martemon. Sposta la bozza da _bozze/ alla radice, aggiorna la navigazione delle schede adiacenti, index.html e README.md, aggiorna la tabella dei numeri nella skill crea-scheda, poi committa e pusha su main (GitHub Pages). Usare quando l'utente chiede di pubblicare/aggiungere/mettere online una scheda.
---

# Pubblica scheda su Martemon

Integra una scheda pronta nel sito e la mette online. Repo: `JacopoCiccoianni/Martemon`,
deploy automatico da `main` su https://jacopociccoianni.github.io/Martemon/.

## 0. Individua la scheda e verifica che sia pronta

1. File da pubblicare: dal prompt, o la bozza più recente in `_bozze/`. Il nome pubblicato è
   `<nome>.html` alla radice.
2. Ricavare dal file: **numero** (`Pokédex Martemon #0NN`), **nome**, **categoria**, **tipi**,
   **territorio** (da Habitat) e una frase per la card dell'indice.
3. Controlli minimi. Se falliscono, fermarsi e segnalare (sistemare è compito di `crea-scheda`):
   - `img/<nome>.png` esiste. Se manca, avvisare che uscirebbe senza artwork e chiedere conferma;
   - nessun asset esterno oltre a `style.css`, `img/…` e il link GitHub del footer;
   - numero **non duplicato** e coerente coi blocchi riservati (tabella in `crea-scheda`); i
     buchi sono voluti e restano;
   - le due `.pagenav` coerenti col numero; anchor del `.toc` = `id` delle sezioni.

## 1. Sposta alla radice

`git mv _bozze/<nome>.html <nome>.html` (o `mv` se la bozza non era tracciata). Se la bozza
usava percorsi `../style.css` o `../img/`, riportarli a `style.css` e `img/`.

## 2. Aggiorna la navigazione delle schede adiacenti

Nella scheda **precedente** (#0NN-1), nelle **due** `.pagenav` (alto e basso), sostituire il
segnaposto del successivo con `<span><a href="<nome>.html">#0NN: Nome</a> →</span>`.
Se esiste già la scheda **successiva** (#0NN+1), fare lo stesso per il suo link «precedente».
Se il precedente non esiste (buco voluto), non c'è nulla da toccare.

Aggiornare «Vedi anche» delle schede vicine solo se il legame ha senso narrativo.

## 3. Aggiorna `index.html`

Una sola griglia `.dexgrid` e una sola tabella `.quick`, entrambe **in ordine di numero**: la
voce nuova va al posto giusto, non in coda.

Card, sul modello delle esistenti:
```html
<div class="card"><span class="num">#0NN</span><h3>Nome</h3><div class="cat">Pokémon Categoria</div><span class="type t-tipo1">Tipo1</span> <span class="type t-tipo2">Tipo2</span>
<p>Frase descrittiva di due-tre righe, con luogo ed evoluzione.</p>
<a class="go" href="nome.html">Vai alla scheda →</a></div>
```
Riga dell'indice rapido:
```html
<tr><td>#0NN</td><td><a href="nome.html"><b>Nome</b></a></td><td>Pokémon Categoria</td><td>Tipo1 Tipo2</td><td>Territorio</td></tr>
```
La riga finale `class="gap"` («specie non ancora catalogata») mostra il **primo numero libero**
dopo l'ultimo pubblicato: aggiornarla. Se il nuovo numero lascia un buco prima di sé, va bene:
il buco non si elenca.

Se è la prima scheda di una linea starter Erba o Fuoco, aggiornare anche il testo del riquadro
`.hero` e del paragrafo «Pokédex Martemon», che oggi dicono che esiste solo la linea Acqua.

## 4. Aggiorna `README.md`

Aggiungere il file all'elenco delle schede nella sezione «Struttura».

## 5. Aggiorna la tabella dei numeri

In `.claude/skills/crea-scheda/SKILL.md` e nel riassunto di `CLAUDE.md`: riga nuova o ✅ sul
blocco appena occupato. La tabella è autorevole solo se viene tenuta aggiornata.

## 6. Deploy

Dalla radice del repo:
```
git add <nome>.html <precedente>.html index.html README.md img/<nome>.png .claude/skills/crea-scheda/SKILL.md CLAUDE.md
git status          # SOLO quello che ci si aspetta
git commit -m "Aggiunta scheda #0NN Nome"
git push
```
Prima del push mostrare il riepilogo di ciò che si sta per pubblicare. Dopo il push ricordare che
Pages impiega un paio di minuti e dare il link diretto:
`https://jacopociccoianni.github.io/Martemon/<nome>.html`.

## 7. Chiusura

Riepilogare: file toccati, numero assegnato, link live, eventuale artwork mancante.
