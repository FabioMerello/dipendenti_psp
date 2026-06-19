# Portale Interno — P&P Sport Management S.A.M.

Hub professionale riservato al personale di **P&P Sport Management S.A.M.**
Sito statico (HTML5 / CSS3 / JS vanilla), senza dipendenze backend, ottimizzato per **GitHub Pages**.

## Struttura del progetto

```
.
├── index.html              ← Pagina principale (Home, Dashboard, Contatti, Risorse)
├── privacy.html             ← Informativa Privacy & Policy
├── css/
│   └── styles.css           ← Design system "Corporate Monaco" (Navy · Bianco · Silver)
├── js/
│   └── main.js               ← Animazioni, contatori, menu mobile, ticker, orologio live
├── assets/
│   └── favicon.svg           ← Monogramma del brand
└── README.md
```

Nessuna immagine esterna: tutte le icone e il logo sono SVG inline o file SVG leggeri.
Le uniche risorse esterne sono i Google Fonts (Space Grotesk · Inter · JetBrains Mono),
caricati via CDN con `font-display: swap`.

## Come pubblicare su GitHub Pages

1. Crea un nuovo repository su GitHub (es. `pp-sport-portal`).
2. Carica **tutti** i file mantenendo la struttura sopra indicata, con `index.html` nella root.
3. Vai su **Settings → Pages**.
4. In **Source**, seleziona il branch `main` (o `master`) e la cartella `/ (root)`.
5. Salva: GitHub fornirà l'URL pubblico (es. `https://<utente>.github.io/pp-sport-portal/`).

> Nota: il sito include `<meta name="robots" content="noindex, nofollow">` per scoraggiare
> l'indicizzazione nei motori di ricerca, mantenendo comunque presente che GitHub Pages è
> per natura un hosting pubblico. Per un vero ambiente riservato si consiglia GitHub Pages
> con repository privato + Pages a pagamento (GitHub Enterprise) oppure un accesso
> autenticato (es. Cloudflare Access, password protetta lato server, VPN aziendale).

## Personalizzazione rapida

| Cosa modificare              | Dove                                                   |
|-------------------------------|---------------------------------------------------------|
| Numeri della dashboard        | `index.html` → attributi `data-counter` nella sezione `#dashboard` |
| Reparti e recapiti             | `index.html` → sezione `#contatti` (`.dept-card`)        |
| Link rapidi / documentazione   | `index.html` → sezione `#documentazione` (`.quick-card`) |
| Testo informativa privacy      | `privacy.html`                                           |
| Palette colori / tipografia    | `css/styles.css` → variabili in `:root`                  |

## Stack tecnico

- **HTML5** semantico, accessibile (focus visibili, `prefers-reduced-motion` rispettato)
- **CSS3** puro, mobile-first, custom properties per il design system
- **JavaScript vanilla** (nessuna libreria): `IntersectionObserver` per le animazioni allo
  scroll, `requestAnimationFrame` per i contatori numerici, orologio live, menu mobile

## Note legali

Il contenuto di `privacy.html` è un **modello informativo** ad uso interno: si raccomanda
la revisione da parte della funzione Legal & Compliance / DPO prima della pubblicazione
definitiva, come indicato nella nota a fondo pagina del documento stesso.
