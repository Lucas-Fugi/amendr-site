# Amendr — marketing site

Single-file static site: `index.html` (embedded CSS + JS, no build step, no dependencies).

## Deploy to Cloudflare Pages

1. Push this folder to a Git repo (or use direct upload).
2. In Cloudflare Pages: **Create project → connect repo** (or **Upload assets** and drop the folder).
3. Build settings: framework preset **None**, build command **empty**, output directory **/** (root).
4. Deploy. Done — it's just static files.

## Before going live

Two placeholders to wire up in `index.html`:

1. **Calendly** — find the comment `Replace CALENDLY_URL with your Calendly event link` and set the
   `data-url` on the `.calendly-inline-widget` div to your event link
   (e.g. `https://calendly.com/yourname/intro-call`).

2. **Lead form endpoint** — at the top of the `<script>` block, replace:
   ```js
   const ENDPOINT = "PASTE_YOUR_APPS_SCRIPT_EXEC_URL_HERE";
   ```
   with your Google Apps Script web-app `/exec` URL (or any endpoint that accepts a POST).
   The form sends JSON as `text/plain;charset=utf-8` with `mode: "no-cors"`, which is the
   standard pattern for Apps Script. Payload fields: `name`, `phone`, `email`, `business`,
   `leads_per_month`, `problem`, `source`, `submitted_at`. Spam is filtered by a hidden
   `company_website` honeypot field.
