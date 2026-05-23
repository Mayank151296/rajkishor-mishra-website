# rajkishormishra.com — Deployment Guide
## GitHub + Cloudflare Pages + GoDaddy DNS

---

## STEP 1 — Push to GitHub

1. Go to https://github.com/new
2. Repository name: `rajkishor-mishra-website`
3. Set to **Public** → Click **Create repository**

Then open **Terminal** on your Mac:

```bash
cd /Users/mayankjoshi/Desktop/rajkishor_mishra_website
git init
git add .
git commit -m "Initial commit — rajkishormishra.com"
git branch -M main
git remote add origin https://github.com/Mayank151296/rajkishor-mishra-website.git
git push -u origin main
```

> If it asks for a password, use a GitHub Personal Access Token.
> Generate one at: https://github.com/settings/tokens/new (check `repo` scope)
> Paste the token as the password in Terminal — it won't show as you type.

---

## STEP 2 — Deploy on Cloudflare Pages

1. Go to https://dash.cloudflare.com
2. **Workers & Pages** → **Create application** → **Pages** → **Connect to Git**
3. Authorise GitHub if prompted → Select `rajkishor-mishra-website`
4. Build settings:
   - **Framework preset**: `None`
   - **Build command**: *(leave completely blank)*
   - **Build output directory**: *(leave completely blank)*
5. Click **Save and Deploy**

✅ Site will be live at something like `rajkishor-mishra-website.pages.dev` within 60 seconds.

---

## STEP 3 — Add Custom Domain on Cloudflare Pages

1. In Cloudflare, go to your Pages project → **Custom domains** tab
2. Click **Set up a custom domain**
3. Enter: `rajkishormishra.com` → Click **Continue**
4. Also add: `www.rajkishormishra.com` → Click **Continue**
5. Cloudflare will show you the DNS records needed — **keep this tab open**

---

## STEP 4 — Point GoDaddy DNS to Cloudflare

> **Two options below. Option A is strongly recommended — it gives you Cloudflare's CDN, DDoS protection, and free SSL automatically.**

---

### ✅ OPTION A — Transfer DNS management to Cloudflare (Recommended)

This moves DNS control to Cloudflare while GoDaddy keeps the domain registration.

#### 4A-1: Add your domain to Cloudflare
1. In Cloudflare dashboard → **Add a Site** → enter `rajkishormishra.com`
2. Choose the **Free plan**
3. Cloudflare will scan existing DNS records — click **Continue**
4. Cloudflare gives you **2 nameservers**, e.g.:
   - `ada.ns.cloudflare.com`
   - `bart.ns.cloudflare.com`
   *(yours will be different — copy them)*

#### 4A-2: Update nameservers on GoDaddy
1. Log in to https://account.godaddy.com/products
2. Click **DNS** next to `rajkishormishra.com`
3. Scroll to **Nameservers** → click **Change**
4. Select **Enter my own nameservers**
5. Replace both nameservers with the two Cloudflare ones above
6. Save

#### 4A-3: Add DNS records in Cloudflare
Once the nameservers propagate (can take 10 min – 2 hrs), go to Cloudflare → **DNS** tab for `rajkishormishra.com` and add:

| Type  | Name | Content                                      | Proxy |
|-------|------|----------------------------------------------|-------|
| CNAME | `@`  | `rajkishor-mishra-website.pages.dev`         | ✅ ON |
| CNAME | `www`| `rajkishor-mishra-website.pages.dev`         | ✅ ON |

Make sure the **orange cloud (proxy)** is ON for both — this enables CDN + SSL.

---

### OPTION B — Keep DNS on GoDaddy (simpler but no Cloudflare CDN)

If you don't want to move nameservers, add these directly in GoDaddy's DNS settings:

1. Log in to GoDaddy → **DNS** for `rajkishormishra.com`
2. Delete any existing A or CNAME records for `@` and `www`
3. Add:

| Type  | Name  | Value                                        | TTL  |
|-------|-------|----------------------------------------------|------|
| CNAME | `@`   | `rajkishor-mishra-website.pages.dev`         | 1hr  |
| CNAME | `www` | `rajkishor-mishra-website.pages.dev`         | 1hr  |

> ⚠️ GoDaddy may not allow a CNAME on `@` (root domain). If it throws an error, use an **A record** instead:
> - Type: `A` | Name: `@` | Value: `192.0.2.1` *(placeholder — Cloudflare Pages requires CNAME flattening, so Option A is better)*
> In that case, switch to Option A.

---

## STEP 5 — SSL Certificate

- **Option A**: Cloudflare auto-provisions SSL. Padlock appears within minutes of nameserver propagation.
- **Option B**: Cloudflare Pages auto-provisions SSL once CNAME resolves. Can take up to 24 hours.

No action needed from you — it's fully automatic.

---

## STEP 6 — Verify Everything

Once DNS propagates (check at https://dnschecker.org — search `rajkishormishra.com`):

✅ https://rajkishormishra.com — loads the site  
✅ https://www.rajkishormishra.com — redirects correctly  
✅ SSL padlock visible in browser  
✅ https://rajkishormishra.com/disclaimer.html — legal page loads  
✅ https://rajkishormishra.com/images/rajkishor-mishra.jpeg — image loads  

---

## UPDATE SITE LATER

Any time you edit files, just run in Terminal:

```bash
cd /Users/mayankjoshi/Desktop/rajkishor_mishra_website
git add .
git commit -m "Update: describe what you changed"
git push
```

Cloudflare Pages auto-redeploys in ~30 seconds. No manual steps needed.

---

## FILE STRUCTURE

```
rajkishor_mishra_website/
├── index.html                         ← Full one-pager
├── disclaimer.html                    ← Legal — Disclaimer
├── privacy-policy.html                ← Legal — Privacy Policy
├── terms.html                         ← Legal — Terms of Use
├── images/
│   ├── rajkishor-mishra.jpeg          ← Founder portrait (nav + hero)
│   ├── ostwal-imperial-hero.jpeg      ← Project full-bleed hero
│   ├── ostwal-imperial-card.jpeg      ← Card thumbnail
│   ├── ostwal-aerial.jpeg             ← Gallery slide
│   ├── ostwal-night.jpeg              ← Gallery slide
│   ├── ostwal-garden.jpeg             ← Gallery slide
│   ├── ostwal-elevation-day.jpeg      ← Gallery slide
│   ├── ostwal-impression.jpeg         ← Gallery + sidebar
│   └── omnr-logo.png                  ← Footer logo
├── qr/
│   └── ostwal-imperial-mrera-qr.png   ← MahaRERA QR
└── DEPLOYMENT_GUIDE.md                ← This file
```

---

## CONTACT
- Email: sales@rajkishormishra.com
- WhatsApp: +91 82628 85023
- Address: Plot No. 13, Jay Gurudev Bunglow, Vajali Pada, Devisha Road, Palghar West — 401404
