# CLAUDE.md — FXdiario

## Project Overview

FXdiario (FX Diário) is a **Forex trading blog and analysis platform** targeting Brazilian traders. The application is a single-file static website written in Portuguese (pt-BR) that provides daily forex market analysis, trading setups, educational content, and market data.

**Author persona**: Sérgio Batalha — independent forex analyst and trader (+20 years experience).

## Architecture

This is a **single-file static site** — everything lives in `fxdiario.html` (HTML + embedded CSS + vanilla JavaScript). There is no build system, no package manager, no backend, and no external JS dependencies.

```
FXdiario/
└── fxdiario.html    # Entire application (HTML, CSS, JS in one file)
```

### Page Structure (top to bottom)

| Section | CSS class | Description |
|---------|-----------|-------------|
| Ticker | `.tw`, `.ti` | Sticky scrolling forex pair prices at top |
| Header | `header`, `.hd` | Navigation, logo, Premium/Alerts buttons |
| Hero | `.hero`, `.hero-in` | Landing area with stats + featured setup card |
| Main Content | `.main > main` | Daily setup card + recent analysis posts |
| Sidebar | `.main > aside.sb` | Author bio, market snapshot, calendar, newsletter, categories |
| Footer | `footer` | Links, risk disclaimer, social links |
| Popup Modal | `.pop#ep` | Exit-intent email capture overlay |

## Technology Stack

- **Language**: HTML5, CSS3, vanilla JavaScript (ES6+)
- **Fonts**: Google Fonts (CDN) — `Inter` (body), `Space Grotesk` (headings)
- **No frameworks or libraries** — zero npm dependencies
- **No build step** — the HTML file is served directly

## Design System

### CSS Custom Properties (`:root`)

```css
--gold: #F5A623      /* Brand accent, primary CTA */
--dark: #0A0D14      /* Page background */
--card: #111420      /* Card/section background */
--border: #1E2235    /* Border color */
--surf: #161924      /* Surface/elevated background */
--tp: #F0F2FF        /* Primary text */
--ts: #8892B0        /* Secondary text */
--tm: #4A5568        /* Muted text */
--green: #00E5A0     /* Positive/bullish */
--gdim: rgba(0,229,160,.1)  /* Green background tint */
--red: #FF4D6A       /* Negative/bearish */
--rdim: rgba(255,77,106,.1) /* Red background tint */
--blue: #4E9EFF      /* Info/secondary accent */
--pur: #7C6AF7       /* Tertiary accent (purple) */
```

### Typography

- **Headings**: `Space Grotesk` (weights 400–700)
- **Body**: `Inter` (weights 300–900)

### Visual Style

- Dark theme throughout
- Glassmorphism effects (`backdrop-filter: blur`)
- Gradient backgrounds on hero/buttons
- Gold (#F5A623) as the primary brand color
- Green/red for bullish/bearish market indicators

### Responsive Breakpoints

- **900px**: Hero switches to single column, nav hidden, hero card hidden, main becomes single column
- **500px**: Reduced heading size (32px), footer becomes single column

## CSS Naming Conventions

The project uses **short, abbreviated class names** (not BEM, not utility-first). This is a deliberate choice for compactness in a single-file project:

| Pattern | Examples | Meaning |
|---------|----------|---------|
| Section prefixes | `.tw`, `.ti`, `.hd`, `.sb` | ticker-wrap, ticker-inner, header, sidebar |
| Tag types | `.t-s`, `.t-a`, `.t-e`, `.t-p` | tag-setup, tag-analysis, tag-education, tag-pensamentos |
| Value markers | `.v-e`, `.v-sl`, `.v-tp` | value-entry, value-stop-loss, value-take-profit |
| Directional | `.up`, `.dn`, `.cu`, `.cd` | up, down, change-up, change-down |
| State | `.active`, `.on`, `.has`, `.today` | Active nav, visible popup, calendar day with post, current day |
| Buttons | `.btn-o`, `.btn-g`, `.bs` | button-outline, button-gold, button-subscribe |

**When adding new classes, follow this pattern**: use short 2–4 character abbreviations with a section prefix when helpful.

## JavaScript

All JS is in a `<script>` block at the end of the `<body>`. Current functionality:

1. **Ticker animation** — `pairs` array of forex data rendered into `.ti` div, duplicated for infinite scroll effect
2. **Exit-intent popup** — triggers on `mouseleave` when cursor moves above viewport (fires once)
3. **Nav active state** — click handler toggles `.active` class on nav links

### Data Model (hardcoded)

Market data is hardcoded in the `pairs` array:
```js
{ p: 'EUR/USD', v: '1.0847', c: '+0.31%', u: 1 }
// p = pair name, v = price value, c = change %, u = 1 (up/green) or 0 (down/red)
```

All article content, setup data, author info, and market snapshot prices are hardcoded in HTML.

## Content Language

All user-facing content is in **Brazilian Portuguese (pt-BR)**. The `<html>` tag uses `lang="pt-BR"`. When adding new content or UI text, maintain Portuguese.

## Key Conventions for AI Assistants

### Do

- Keep everything in **one HTML file** unless explicitly asked to split
- Use the existing **CSS custom properties** for colors — never hardcode hex values
- Follow the **abbreviated class naming** convention
- Maintain the **dark theme** aesthetic
- Use `Space Grotesk` for headings and `Inter` for body text
- Keep the file well-organized with CSS comment section markers (`/* TICKER */`, `/* HEADER */`, etc.)
- Write user-facing text in **Brazilian Portuguese**
- Use semantic HTML elements where appropriate (`<header>`, `<main>`, `<aside>`, `<footer>`, `<article>`)

### Don't

- Don't add npm/package.json or build tooling unless explicitly requested
- Don't add external JS libraries without discussion — the project is intentionally vanilla
- Don't break the single-file architecture by splitting into separate CSS/JS files unless asked
- Don't use BEM, Tailwind, or other naming conventions — stick to the existing abbreviated style
- Don't introduce English UI text (code comments in English are fine)
- Don't remove the risk disclaimer in the footer — it is legally relevant

## Deployment

The site can be served as a static file on any hosting platform:

- Open `fxdiario.html` directly in a browser
- Host on GitHub Pages, Netlify, Vercel, S3, or any static file server
- No build, compile, or install step needed

## Future Evolution Notes

The current structure is an early-stage static prototype. Likely next steps include:

- Replacing hardcoded market data with a real-time API
- Adding a CMS or backend for article management
- User authentication for premium features
- Splitting into multiple pages or adopting a static site generator
- Adding real form submission for the newsletter signup
