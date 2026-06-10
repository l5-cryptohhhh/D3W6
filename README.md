# EpiShop — E-commerce Front-End

Progetto didattico sviluppato durante il corso Epicode (D3W6). Simula la pagina principale di un e-commerce con navbar responsive, carousel promozionale, griglia prodotti e tema scuro.

---

## Indice

1. [Tecnologie utilizzate](#1-tecnologie-utilizzate)
2. [Struttura dei file](#2-struttura-dei-file)
3. [HTML — Struttura della pagina](#3-html--struttura-della-pagina)
4. [SCSS — Stili personalizzati](#4-scss--stili-personalizzati)
5. [JavaScript — Interattività](#5-javascript--interattività)
6. [Come eseguire il progetto](#6-come-eseguire-il-progetto)

---

## 1. Tecnologie utilizzate

| Tecnologia | Versione | Utilizzo |
|---|---|---|
| HTML5 | — | Struttura semantica della pagina |
| Bootstrap | 5.3.3 (CDN) | Layout responsive, componenti UI (navbar, carousel, modal, badge) |
| SCSS | — | Stili personalizzati con variabili e nesting |
| CSS | — | Output compilato da SCSS |
| JavaScript (vanilla) | ES6+ | Toggle tema scuro |

Bootstrap viene caricato da CDN (`cdn.jsdelivr.net`) sia per il CSS che per il bundle JS (che include Popper.js, necessario per modal e carousel).

---

## 2. Struttura dei file

```
D3W6/
├── index.html              # Pagina principale
├── assets/
│   ├── scss/
│   │   └── style.scss      # Sorgente stili (da compilare)
│   ├── css/
│   │   ├── style.css       # Output compilato da style.scss
│   │   └── style.css.map   # Source map per il debug
│   └── js/
│       └── script.js       # Logica toggle dark mode
└── README.md
```

Il file `style.scss` è la sorgente: **non modificare `style.css` direttamente**, ma compilare sempre da SCSS.

---

## 3. HTML — Struttura della pagina

La pagina è costruita in ordine dall'alto verso il basso:

### 3.1 `<head>`
Carica Bootstrap CSS dal CDN e il foglio di stile personalizzato `assets/css/style.css`. Il custom CSS viene caricato **dopo** Bootstrap per poter sovrascriverne le regole dove necessario.

### 3.2 Navbar (`<nav>`)
Navbar Bootstrap con classe `navbar-expand-md`: è espansa su schermi `≥ 768px` e collassata (hamburger) su mobile. Contiene:
- Logo **EpiShop** (`navbar-brand`)
- Link di navigazione: *Prodotti*, *Offerte*, *Contatti*
- Bottone **"Tema scuro"** (`#btnTema`) — agganciato dal JS

### 3.3 Hero Carousel (`#caroselloSaldi`)
Carousel Bootstrap con attributo `data-bs-ride="carousel"` per l'avanzamento automatico. Include tre slide con sfondi di colore differente (`bg-primary`, `bg-success`, `bg-dark`) e messaggi promozionali testuali.

### 3.4 Contenuto principale (`<main>`)
Layout a due colonne tramite griglia Bootstrap (`row`):

- **Sidebar** (`col-lg-3`, `d-none d-lg-block`): lista categorie visibile **solo su desktop** (`lg` e superiori).
- **Sezione prodotti** (`#sezione-prodotti`, `col-lg-9`): griglia a tre card responsive (`col-12 col-md-6 col-lg-4`). Ogni card contiene:
  - Box immagine (`.box-immagine-card`)
  - Titolo, descrizione, prezzo
  - Bottone che apre il **Modal** corrispondente tramite `data-bs-toggle="modal"`

### 3.5 Modal (`#modalSneaker`, `#modalTshirt`, `#modalBorsa`)
Tre modal Bootstrap (`modal-dialog-centered modal-lg`), uno per prodotto. Mostrano immagine ingrandita, descrizione estesa, prezzo e bottone *Aggiungi al carrello*. Vengono attivati dai bottoni **Dettagli** delle card.

### 3.6 Footer
Footer scuro con copyright.

### 3.7 Script JS
Bootstrap Bundle JS (CDN) viene caricato in fondo al `<body>` prima di `script.js`, garantendo che i componenti Bootstrap (modal, carousel) siano già inizializzati quando il custom JS viene eseguito.

---

## 4. SCSS — Stili personalizzati

Il file `assets/scss/style.scss` è organizzato in sezioni:

### 4.1 Variabili
Variabili SCSS per colori e dimensioni riutilizzate nel file:
```scss
$colore-sfondo-card:   #f0f0f0;
$colore-sfondo-modal:  #e9e9e9;
$altezza-card:         180px;
$altezza-modal:        300px;
```
Variabili separate per il tema scuro (prefisso `$dark-`).

### 4.2 Scroll smooth
```scss
html { scroll-behavior: smooth; }
```
Fa sì che il link *Prodotti* nella navbar scorra fluentemente fino a `#sezione-prodotti`.

### 4.3 `.box-immagine-card` e `.box-immagine-modal`
Contenitori a dimensione fissa per le immagini dei prodotti. Usano `display: flex` con `align-items: center` e `justify-content: center` per centrare l'immagine, e `object-fit: contain` per non deformarla.

### 4.4 `.btn-tema`
Bottone trasparente con bordo bianco usato nella navbar per il toggle del tema. L'effetto hover aggiunge un leggero sfondo semitrasparente.

### 4.5 Tema scuro (`body.dark`)
Quando il JS aggiunge la classe `.dark` al `<body>`, una serie di regole SCSS con nesting sovrascrive i colori di sfondo e testo di: body, `<main>`, sidebar, card prodotti, box immagini e modal. Navbar e footer rimangono invariati. I valori `!important` sono necessari per battere le utility class di Bootstrap (es. `bg-white`, `text-dark`).

Per compilare:
```bash
sass assets/scss/style.scss assets/css/style.css
```
oppure in modalità watch:
```bash
sass --watch assets/scss/style.scss assets/css/style.css
```

---

## 5. JavaScript — Interattività

`assets/js/script.js` gestisce il toggle del tema scuro:

```js
const btnTema = document.querySelector('#btnTema');

btnTema.addEventListener('click', () => {
    document.body.classList.toggle('dark');
});
```

Al click sul bottone nella navbar, la classe `.dark` viene aggiunta o rimossa dal `<body>`. Tutte le variazioni visive sono gestite interamente dal CSS/SCSS (sezione `body.dark`): il JS non modifica inline style né manipola altri elementi.

---

## 6. Come eseguire il progetto

Il progetto è statico: non richiede un server né un build system obbligatorio.

1. Clonare il repository o aprire la cartella del progetto.
2. Aprire `index.html` direttamente nel browser **oppure** avviare un server locale (es. estensione *Live Server* di VS Code).
3. Per modificare gli stili, editare `assets/scss/style.scss` e ricompilare in CSS (vedi sezione 4.5).

> Bootstrap e le immagini dei prodotti vengono caricati da URL esterni: è necessaria una connessione internet per visualizzare la pagina correttamente.
