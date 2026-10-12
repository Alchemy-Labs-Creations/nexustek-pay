# NexusTek Pay

A small, branded, **static** page that sits between a NexusTek invoice and Square's checkout. The invoice email/PDF links here with the invoice details in the URL; the page shows a clean branded summary and a **Pay securely** button that goes to the real Square checkout. Nothing is stored or sent by this page - Square handles the payment, and the amount is fixed by the Square link.

## Files
- `index.html` - the page (self-contained: inline CSS + JS)
- `nexustek-icon.png`, `nexustek-wordmark.png` - brand images
- `README.md` - this file

## The URL it expects
`index.html?` + these query params (all optional; **`u` is what makes the Pay button appear**):

| param | meaning | example |
|-------|---------|---------|
| `u`   | the Square checkout URL (URL-encoded) | `https%3A%2F%2Fsquare.link%2Fu%2FAbC123` |
| `biz` | your business name (header + footer) | `NexusTek IT` |
| `n`   | invoice number | `INV-0001` |
| `a`   | amount (no symbol) | `150.00` |
| `c`   | currency symbol | `$` |
| `d`   | due date (any readable text; turns red if past) | `Oct 24, 2026` |
| `to`  | who it's billed to | `ACME Corp` |
| `e`   | contact email (footer + "no link" help) | `nexus.tek.it@outlook.com` |
| `p`   | contact phone | `555-0100` |

**Security:** the Pay button only ever opens an `https://` URL on a Square host (`square.link`, `squareup.com`, `squareupsandbox.com`, `*.square.site`). Any other `u` is ignored, so this page can't be turned into an open redirect.

## Publish it (GitHub Pages, free)
1. Create a repo under your **NexusTek** GitHub org (e.g. `pay`), make it **public**.
2. Upload these files to the repo root (or `git push`).
3. Repo **Settings > Pages**: Source = *Deploy from a branch*, Branch = `main` / `/root`. Save.
4. Live in a minute at `https://<org>.github.io/pay/`.

### Custom domain (optional, later)
Buy a domain, then in **Settings > Pages > Custom domain** enter e.g. `pay.yourdomain.com`, add the DNS record GitHub shows (a `CNAME` to `<org>.github.io`), and tick **Enforce HTTPS**. A `CNAME` file in the repo pins it.

## Wiring NexusTek to it (done in the app once this has a URL)
NexusTek's billing email currently links its **Pay now** button straight to the Square link. Once this page has its address, the app builds the button as `https://<pay-page>/?u=<square link>&biz=...&n=...&a=...&d=...&to=...&e=...` instead - a tiny change in `IT Desk/lib/billingDocs.js` (`invoiceEmail`) + a base URL in Billing settings. Tell the assistant the final URL and it will wire it.
