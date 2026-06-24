# Deploy mayurp.com — Cloudflare Pages

Your stack: **GoDaddy** (registrar) · **Cloudflare** (DNS) · **GitHub** (files) · **Cloudflare Pages** (host — this guide).
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

---

## Step 1 — Push the files to GitHub
Commit and push this whole folder to your GitHub repo (make sure the new `site/files/` and `site/images/author/` are included):

```
git add .
git commit -m "Prepare site for deploy: sticky nav + resume/og assets"
git push
```

## Step 2 — Create the Cloudflare Pages project
1. Cloudflare dashboard → **Workers & Pages** → **Create** → **Pages** → **Connect to Git**.
2. Authorize Cloudflare's GitHub app (first time only) and pick your repo.
3. Set the build settings exactly:
   - **Framework preset:** `None`
   - **Build command:** *(leave empty)*
   - **Build output directory:** `site`
   - **Production branch:** `main`
4. Click **Save and Deploy**.

## Step 3 — Test on the free URL
After ~1 minute you get a `https://<project>.pages.dev` link. Open it and check:
- Homepage loads, nav **stays pinned** when scrolling, theme toggle works.
- **Resume** button opens the PDF.
- Nav links jump to the right sections (landing just below the bar).

*(On `.pages.dev` the Resume/photo work because they're relative to the site root — same as they will on your domain.)*

## Step 4 — Attach your domain
1. In the Pages project → **Custom domains** → **Set up a domain**.
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
