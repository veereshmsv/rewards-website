# Deploying Kids Rewards to Cloudflare Pages

This `site/` folder is a complete, self-contained static website. There is **no build step** —
just upload the folder as-is. It only makes one external request (Google Fonts); everything
else (icons, logo, styles, scripts) is bundled locally.

## What's in this folder

| File | Purpose |
|------|---------|
| `index.html` | Home / landing page |
| `privacy.html` | Privacy policy (Australia, India, UAE, UK, US) |
| `favicon.png` | Browser tab icon |
| `icon.png` | Social/Open-Graph share image |
| `icons/` | App icons (192/512, maskable) |
| `_headers` | Security headers applied by Cloudflare Pages |
| `robots.txt`, `sitemap.xml` | SEO |

---

## Option A — Dashboard upload (recommended, no tools needed)

1. Go to **https://dash.cloudflare.com/** and sign in.
2. In the left sidebar, open **Workers & Pages**.
3. Click **Create application** → **Pages** tab → **Upload assets**.
4. Give the project a name, e.g. **`kidsreward`** (this becomes `kidsreward.pages.dev`).
5. **Drag the entire `site` folder** (or its contents) into the upload area, then click
   **Deploy site**. Wait for the green success screen.
6. **Add your custom domain:**
   - Open the project → **Custom domains** tab → **Set up a custom domain**.
   - Enter **`kidsreward.in`** and follow the prompts, then repeat for **`www.kidsreward.in`**.
   - If `kidsreward.in` is already on Cloudflare, DNS is configured automatically. If the domain
     is registered elsewhere, Cloudflare will show the DNS records (usually a `CNAME`) to add at
     your registrar.
7. Cloudflare issues an SSL certificate automatically. Once it's active, your site is live at
   **https://kidsreward.in/** and the policy at **https://kidsreward.in/privacy.html**.

### Updating the site later
Re-open the Pages project → **Create deployment** (or the **Upload assets** flow) and drag the
updated `site` folder again. Each upload is a new versioned deployment you can roll back.

---

## Option B — Wrangler CLI (optional)

If you prefer the command line and have Node.js installed:

```bash
npx wrangler login
npx wrangler pages deploy site --project-name=kidsreward
```

The first command opens a browser to authorize your Cloudflare account; the second uploads the
folder. Add `--branch=main` for the production branch.

---

## Things to review before going live

- **Google Play link.** The download buttons point to
  `https://play.google.com/store/apps/details?id=com.gunavs.kidsrewards`
  (derived from the app's package name). If your published listing uses a different URL, update
  the three `play.google.com/...` links in `index.html`. If the app isn't on the Play Store yet,
  you may want to change the button text to "Coming soon" or point it at the contact email.
- **Contact email.** The site uses `veereshmsv@gmail.com` throughout. Change it if you'd rather
  publish a different support address.
- **Effective date.** The privacy policy is dated **30 August 2026**. Update it whenever you
  revise the policy.

---

## Optional: Android App Links association

Your app project contains `Digital Asset Links.json`. If you want tapping a `kidsreward.in` link
to open the app (App Links) and to strengthen Google Sign-In domain association, host that file at:

```
https://kidsreward.in/.well-known/assetlinks.json
```

To do that, create a folder `.well-known/` inside `site/`, copy the JSON in as `assetlinks.json`,
and re-deploy. (Not required for the website itself — only for app↔domain linking.)
