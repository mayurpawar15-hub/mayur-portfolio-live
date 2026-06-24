# Deploy mayurp.com — Cloudflare

Your stack: **GoDaddy** (registrar) · **Cloudflare** (DNS) · **GitHub** (files) · **Cloudflare Workers** (host — this guide).
Every future change goes live by doing `git push` — Cloudflare rebuilds automatically.

---

## Already done for you
The deployable site lives in the **`site/`** folder, with all hardcoded links resolved:

```
site/
├── index.html                     → served at https://mayurp.com/
├── files/resume.pdf               → the "Resume" button link
└── images/author/mayur.jpeg       → social-share / SEO preview image
```

Only the `site/` folder gets published. Everything else in the repo (source file, screenshots, uploads) stays private.

There's also a **`wrangler.jsonc`** at the repo root — it's the one-line config that tells Cloudflare Workers to serve the `site/` folder. Don't delete it.

---

## Step 1 — Push the files to GitHub
Commit and push this whole folder to your GitHub repo (make sure the new `site/files/` and `site/images/author/` are included):

```
git add .
git commit -m "Prepare site for deploy: sticky nav + resume/og assets"
git push
```

## Step 2 — Deploy (Workers flow — the screen you're on)
Your repo `mayurpawar15-hub/mayur-portfolio-live` is connected in the **Workers** flow. That's fine — `wrangler.jsonc` handles the rest. On the "Set up your application" screen:
- **Build command:** *(leave empty)*
- **Deploy command:** `npx wrangler deploy`  ← leave as-is
- Then click **Deploy**.

(Make sure `wrangler.jsonc` was pushed in Step 1, or the deploy won't know what to serve.)

> **No-config alternative (Pages):** click **Back** → choose **Pages** instead of Workers → **Connect to Git** → set **Build output directory = `site`** → **Save and Deploy**. Either path works — you only need one.

## Step 3 — Test on the free URL
After ~1 minute you get a `https://<project>.pages.dev` link. Open it and check:
- Homepage loads, nav **stays pinned** when scrolling, theme toggle works.
- **Resume** button opens the PDF.
- Nav links jump to the right sections (landing just below the bar).

*(On `.pages.dev` the Resume/photo work because they're relative to the site root — same as they will on your domain.)*

## Step 4 — Attach your domain
1. Open your project → **Settings** → **Domains & Routes** → **Add** → **Custom domain**.
   *(On the Pages path it's the **Custom domains** tab instead — same idea.)*
2. Add `mayurp.com`, then repeat for `www.mayurp.com`.
3. Because your DNS is already on Cloudflare, it **creates the DNS records and the HTTPS certificate automatically** — no manual A/CNAME records, no IP addresses. Give it a few minutes for the padlock to go green.

## Step 5 — GoDaddy (probably nothing to do)
GoDaddy is only your registrar. **If Cloudflare already manages your DNS** (you see `mayurp.com` as an active site in Cloudflare with its DNS records), you don't touch GoDaddy at all.

Only if Cloudflare shows the domain as "pending / not active": in GoDaddy → your domain → **Nameservers** → change to the two Cloudflare nameservers shown in your Cloudflare dashboard, then wait for it to activate (up to a few hours).

---

## Final go-live checklist
- [ ] `https://mayurp.com` loads over HTTPS (green padlock)
- [ ] `https://www.mayurp.com` works (or redirects to the apex)
- [ ] Resume button → `https://mayurp.com/files/resume.pdf` opens
- [ ] Sticky nav + light/dark toggle work
- [ ] (Optional) Paste your URL into a link preview tester to confirm the social image shows

## Notes / optional polish
- **Contact email mismatch:** the buttons use `hello@mayurp.com` but the SEO data uses `mayurpawar15@gmail.com`. Pick one and I can make them consistent.
- **Minor:** the page has no favicon and no `<html lang>` — small SEO/branding nicety I can add if you want.
- To update the site later: edit, `git push`, done — Cloudflare redeploys in ~1 min.
