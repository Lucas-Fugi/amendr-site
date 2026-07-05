# CLAUDE.md

## What this project is

Marketing website for **Amendr** — a done-for-you lead-response service for HVAC companies.
Amendr owns the "middle of the funnel": it answers new leads in under 60 seconds by text and
email, ranks each lead by urgency, follows up relentlessly, and books appointments automatically.
Core promise: **"No lead gets left behind."** Audience: busy HVAC owner-operators.

**Copy rules:** This is a service pitch, not a software pitch. Sell the outcome and the feeling
of relief, backed by real data (MIT/HBR lead-response study). Never say "AI-powered,"
"automation platform," or "lead management system." Speak like a human who understands
their business.

## Tech stack

- Plain HTML, CSS, and vanilla JS — **no frameworks, no build tools, no dependencies**
- Everything lives in a single `index.html` with embedded `<style>` and `<script>` blocks
- Hosted as static files on **Cloudflare Pages** (framework preset: None, no build command,
  output dir `/`), deployed from GitHub repo `Lucas-Fugi/amendr-site`, branch `main`

## Design system

- **Light theme** — white cards on a light-gray page. NOT dark mode.
- Page background `#f4f4f5`, alternating sections `#eaeaeb`, cards `#ffffff`
- Headlines near-black `#0c0c0d` (weight 900, tight negative letter-spacing);
  body text `#5d6168`; faint text `#8a8d93`
- Single accent color: **crimson `#b11116`** (hover `#8e0e12`) — used sparingly for CTAs,
  kickers, and emphasis. Rose tint `#fbeaea` with border `#f0cfd1` for highlight pills/banners.
  Hairline borders `#e4e3e3` / `#d8d7d7`.
- Font: **Archivo** from Google Fonts (weights 400–900)
- All tokens are CSS custom properties in `:root` at the top of the `<style>` block
- Motion: IntersectionObserver fade-ups (`.reveal` class, 90ms stagger via `[data-stagger]`
  groups), animated stat counters (`.stat-num[data-target]`, ~1.4s ease-out), nav gains a
  hairline + blur on scroll. All motion respects `prefers-reduced-motion`.

## Calendly integration

Inline widget in the `#book` section:

```html
<div class="calendly-inline-widget" data-url="https://calendly.com/fugi-amendr/15-minute-call" ...>
```

The widget script (`https://assets.calendly.com/assets/external/widget.js`) loads at the
bottom of `<body>`. To change the event, edit the `data-url` attribute.

## Lead form

Fallback form below the Calendly widget (`#leadForm`). Behavior:

- POSTs JSON to a **Google Apps Script web app** — the URL is the `ENDPOINT` const at the
  top of the `<script>` block in `index.html` (currently set to a live `/exec` URL)
- Uses `fetch` with `mode: "no-cors"` and `Content-Type: text/plain;charset=utf-8`
  (the standard Apps Script pattern — the response is opaque, so success is assumed
  when the fetch resolves)
- Payload fields: `name`, `phone`, `email`, `business`, `leads_per_month`, `problem`,
  `source: "amendr-site"`, `submitted_at`
- Spam protection: hidden honeypot input named `company_website` (off-screen, `tabindex="-1"`,
  `aria-hidden`) — if filled, the form silently shows success without sending
- On success the form hides and `#formSuccess` shows

## File structure

```
index.html   — the entire site (markup + embedded CSS + embedded JS)
README.md    — Cloudflare Pages deploy steps + integration notes
CLAUDE.md    — this file
```

Deliberate decision: **single-file site**. No separate .css/.js files, no assets folder —
icons are inline SVGs, favicon is a data URI, fonts come from Google Fonts. Keep it that way
unless the site grows enough to justify splitting.

Section anchor IDs used by nav and footer links: `#system` (funnel), `#features` (what you get),
`#proof` (stats), `#book` (CTA / Calendly / form).
