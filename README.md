# joinrealsingles.com — early access page

A single static page. No build step, no framework, no dependencies. Deploys to
Vercel as-is.

```
index.html    the page (CSS and JS inlined)
privacy.html  what we collect and how to be removed
logo.png      Real Singles lockup, transparent
favicon.png   heart mark, 256x256
og.png        1200x630 social preview
vercel.json   security headers + clean URLs
```

---

## Before it goes live — set the form endpoint

Open `index.html` and find:

```js
var ENDPOINT = "";
```

Put a URL there that accepts a `POST`. The page sends JSON:

```json
{ "name": "...", "email": "...", "source": "joinrealsingles.com",
  "list": "early-access", "ts": "2026-10-14T20:00:00.000Z" }
```

Options, best first:

| Where | Why |
|---|---|
| **GoHighLevel inbound webhook** | You already pay for it, and signups land as contacts in the CRM where you'd work them anyway |
| **Formspree** (`https://formspree.io/f/xxxxxxxx`) | Two minutes to set up, emails you each signup, free tier is fine for a waitlist |
| **Your own serverless function** | Most control, most work, and you still need somewhere to store the records |

Left empty, the form tells the visitor it isn't connected rather than silently
dropping a signup. Do not ship it that way.

## Deploy

1. Push this folder to the repo root
2. Vercel → New Project → import the repo
3. Framework preset: **Other**. No build command, no output directory — it is static
4. Deploy, then Settings → Domains → add `joinrealsingles.com` and `www.joinrealsingles.com`
5. At Namecheap, point the domain at Vercel (Vercel prints the exact A / CNAME records)

**HTTPS provisions itself** once the domain resolves to Vercel — usually within a few
minutes. That is the whole reason for doing this: the domain currently sits on a
Namecheap parking IP with no certificate, so `https://joinrealsingles.com` fails its
SSL handshake, and the QR code on the showcase video resolves to nothing on a phone.

## After deploying — check these

- [ ] `https://joinrealsingles.com` loads over **HTTPS** with no warning
- [ ] `www` redirects to the apex (or vice versa — just pick one)
- [ ] Submit a test signup and confirm it arrives wherever ENDPOINT points
- [ ] Scan the QR from the showcase video with a phone and confirm it lands here
- [ ] Open it on an actual phone — most visitors arrive by scanning a code

## Copy constraints

Wording follows `Real Singles/CLAIMS.md`. The app is not released, so the page says
**early access list**, never "download". It makes no promises about outcomes, quotes
no statistics, names no competitor, and mentions no pricing, speed dating, rewards or
AI features — none of which are live. If you change the copy, re-check it against that
file first.
