# arturpoghosyan.com — Deployment Guide

A handbook for getting your site live on the cheapest, fastest setup possible.

**Total cost:** $10/year (just the domain). Hosting is free forever.

---

## What you have

- `index.html` — the website
- `style.css` — the design
- `favicon.svg` — the little A·P icon in the browser tab
- `sitemap.xml` — tells Google what pages exist
- `robots.txt` — tells search engines they're welcome
- `README.md` — this guide

---

## Step 1 — Buy the domain on GoDaddy

You're already there. Just buy `arturpoghosyan.com`.

**Important:** at checkout, **uncheck everything except the domain**. GoDaddy will try to add Privacy ($10/yr — fine but optional), Email ($), Hosting ($$), Website Builder ($$$). You don't need any of it. We're hosting elsewhere for free.

If they push a multi-year deal at a discount, that's fine — same domain, just paid in advance.

---

## Step 2 — Make a free GitHub account (if you don't have one)

Go to **github.com** → Sign up. 2 minutes. Use your real name as the username if it's available (also good for SEO).

---

## Step 3 — Upload the site to GitHub

You have two ways. Pick whichever is more comfortable.

### Easy way (no command line)

1. On GitHub, click the **+** in the top-right → **New repository**
2. Repository name: `arturpoghosyan-site` (or anything you like)
3. Set it to **Public**
4. Check **Add a README file** → click **Create repository**
5. On the repo page, click **Add file** → **Upload files**
6. Drag in: `index.html`, `style.css`, `favicon.svg`, `sitemap.xml`, `robots.txt`
7. Scroll down → **Commit changes**

That's it. Your code is on GitHub.

### Command-line way (if you prefer)

```bash
cd /folder/with/the/files
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/arturpoghosyan-site.git
git push -u origin main
```

---

## Step 4 — Make a free Cloudflare account & deploy

Cloudflare Pages will host the site for free, with HTTPS, and serve it from servers all over the world (= fast = good for SEO).

1. Go to **dash.cloudflare.com/sign-up** → create an account
2. Once logged in, in the left sidebar click **Workers & Pages**
3. Click **Create** → choose the **Pages** tab → **Connect to Git**
4. Click **Connect GitHub**, authorize Cloudflare, and select your `arturpoghosyan-site` repo
5. On the build settings page:
   - Project name: `arturpoghosyan` (or anything)
   - Production branch: `main`
   - Framework preset: **None**
   - Build command: *leave blank*
   - Build output directory: *leave blank* (or just `/`)
6. Click **Save and Deploy**

About 30 seconds later, your site is live at something like `arturpoghosyan.pages.dev`. Open it. Make sure it looks right.

---

## Step 5 — Connect your GoDaddy domain to Cloudflare

This is the only slightly fiddly step. Here's exactly what to click.

### 5a. Add your domain to Cloudflare

1. In Cloudflare dashboard, top-left, click **Add a site** (or **Websites** → **Add a domain**)
2. Type `arturpoghosyan.com` → Continue
3. Pick the **Free plan** → Continue
4. Cloudflare will scan and show you any existing DNS records. Just click **Continue**.
5. Cloudflare will now give you **two nameservers** that look like `something.ns.cloudflare.com`. **Copy both.** You'll paste them into GoDaddy in a second.

### 5b. Point GoDaddy to Cloudflare's nameservers

1. Log in to **godaddy.com** → **My Products**
2. Find `arturpoghosyan.com` → click **DNS**
3. Find the **Nameservers** section → **Change**
4. Choose **I'll use my own nameservers**
5. Paste the two Cloudflare nameservers you copied. Save.

This handoff takes anywhere from 5 minutes to a few hours to propagate. Cloudflare will email you when it's done.

### 5c. Connect the domain to your Pages site

Once Cloudflare confirms the domain is active:

1. Cloudflare dashboard → **Workers & Pages** → click your `arturpoghosyan` project
2. Click the **Custom domains** tab → **Set up a custom domain**
3. Enter `arturpoghosyan.com` → click through the prompts
4. Repeat for `www.arturpoghosyan.com` so both work

Cloudflare automatically adds the right DNS records and issues a free HTTPS certificate. Wait a couple of minutes, then visit **https://arturpoghosyan.com** — you should see your site.

---

## Step 6 — Submit to Google (so people can find you)

1. Go to **search.google.com/search-console**
2. Click **Add property** → **URL prefix** → enter `https://arturpoghosyan.com` → Continue
3. To verify, choose the **HTML tag** method. Google gives you a meta tag like
   `<meta name="google-site-verification" content="ABC123..." />`
4. Open `index.html`, paste that tag inside the `<head>` (anywhere near the top), commit & push to GitHub. Cloudflare auto-redeploys in ~30 seconds.
5. Back in Search Console → click **Verify**.
6. Once verified, in the left sidebar click **Sitemaps** → enter `sitemap.xml` → Submit.

Google will start crawling. Searches for your name should start showing your site within a few days to a couple of weeks.

**Bonus:** also do this for **Bing Webmaster Tools** (bing.com/webmasters) — same idea, takes 5 minutes, picks up traffic Google misses.

---

## Step 7 — Replace the placeholders with your real content

The site is fully working but has stand-in content in a few places. Open `index.html` in any text editor (VS Code is free, or even Notepad) and search for the comments — they tell you exactly what to swap.

### Photos for the café gallery and works
Search for `<div class="placeholder"` — each one is where an image goes. Replace it like this:

```html
<!-- Before: -->
<div class="placeholder" aria-label="Replace with photoshoot image 1"></div>

<!-- After: -->
<img src="images/cafe-opening.jpg" alt="Christian Fashion Café opening night" />
```

Then create a folder called `images/` next to `index.html`, drop your photos in there, and reference them like `images/yourfile.jpg`. Keep filenames lowercase, no spaces (use dashes).

### Press / influencer features
Search for `class="press-item"`. Each block is one feature — just replace outlet name, headline quote, and the link.

### Add new research papers
Search for `class="paper"`. Copy any `<li class="paper">` block and paste a new one. For DOIs, the link format is `https://doi.org/10.xxxx/yyyy`.

### Add new Medium articles
Search for `class="essay"`. Same idea — copy a block, paste it at the top, edit the URL/title/excerpt.

### Update your email
Search for `hello@arturpoghosyan.com` and change it to whatever email you want public.

### Updating the live site after edits
GitHub web upload route: open the file on github.com → click the pencil icon → edit → **Commit changes**. Cloudflare auto-redeploys in ~30 seconds.

---

## Optional polish (later)

- **Real OG share image** — make a 1200×630 image (your logo or a beautiful dessert shot), name it `og-image.jpg`, drop it in the root. It's what shows up when someone shares your link on LinkedIn / WhatsApp.
- **Custom email** — `hello@arturpoghosyan.com`. Free options: Cloudflare Email Routing forwards to your Gmail. Setup is in Cloudflare dashboard → Email.
- **Analytics** — Cloudflare's built-in Web Analytics is free, privacy-friendly, no banners required. Turn it on inside your Pages project.
- **Instagram / TikTok links** — search for the commented-out `<!-- Add Instagram -->` line in `index.html` and uncomment it.

---

## SEO checklist (already done in the build)

✅ Page title with your name + role
✅ Meta description
✅ Open Graph tags (LinkedIn, Facebook previews)
✅ Twitter Card tags
✅ JSON-LD structured data (Person schema with ORCID, social links, café affiliation)
✅ Semantic HTML (proper `<header>`, `<main>`, `<section>`, `<article>`)
✅ Mobile responsive
✅ Fast loading (no JS frameworks, no trackers)
✅ HTTPS (Cloudflare gives this free)
✅ sitemap.xml + robots.txt
✅ Canonical URL set
✅ Descriptive alt text on placeholders (replace with real ones when you swap photos)

---

## If something breaks

- **Site looks unstyled** → `style.css` didn't upload, or it's in a subfolder. All five files should sit in the root of the repo.
- **Domain doesn't resolve after 24 hrs** → double-check the nameservers in GoDaddy match Cloudflare's exactly.
- **Google not indexing** → it takes time. Submit the URL manually in Search Console (top search bar → "Request indexing").

That's everything. Walk through Step 1 → 6 once, then steps 7+ are just edits whenever you have new work to show.
