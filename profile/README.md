# Deploy to Cloudflare Pages

How to get the landing page live at `cryptobasis.ai`. Estimated time: **15 minutes**.

You'll need the contents of the `landing-page/` folder (everything in it). Treat that folder as the entire repo.

---

## Option A · Direct upload (fastest, no GitHub repo needed)

This is the fastest way to get the page live. You can switch to a GitHub-connected deploy later if you want.

1. **Sign in** to [dash.cloudflare.com](https://dash.cloudflare.com).
2. In the left sidebar, find **Workers & Pages** (or just **Pages** depending on your dashboard version).
3. Click **Create application** → **Pages** tab → **Upload assets**.
4. **Project name**: `cryptobasis-ai` (lowercase; this is just an internal Cloudflare label).
5. Drag the entire contents of the `landing-page/` folder into the upload zone — `index.html`, the `assets/` folder, `_headers`, `robots.txt`, `sitemap.xml`, `favicon.ico`, and `README.md`.
6. Click **Deploy site**.
7. After a few seconds, you'll get a preview URL like `cryptobasis-ai.pages.dev`. Click it. You should see the landing page.

Now connect it to your real domain:

8. In the project page, click **Custom domains** (tab near the top).
9. Click **Set up a custom domain** → enter `cryptobasis.ai` → click **Continue**.
10. Cloudflare detects the domain is already on your account and offers to add the DNS record automatically. Click **Activate domain**.
11. Wait 30–60 seconds for SSL provisioning. Visit `https://cryptobasis.ai` — the page should load.

Also set up `www.cryptobasis.ai` to redirect:

12. In the project's Custom domains, click **Set up a custom domain** again → `www.cryptobasis.ai` → **Continue** → **Activate**.

---

## Option B · GitHub-connected deploy (recommended for the long term)

If you want changes to auto-deploy when you push to a GitHub repo, do this instead. Slightly more setup, but worth it once the site has more than one page.

### B.1 — Create the GitHub repo

1. **github.com/cryptobasis-ai** → **+** (top right) → **New repository**.
2. **Repository name**: `cryptobasis-ai` (or `landing-page`; your call).
3. Description: `Source for cryptobasis.ai`.
4. Visibility: **Public** (free Cloudflare Pages requires public, or you can use private with paid Cloudflare).
5. Initialize with: **Add a README file**.
6. Create.

### B.2 — Upload the files

You can either use the GitHub web uploader (drag and drop) or, if you're comfortable with `git`, push from your local machine.

**Web uploader:**
1. On the new repo page, click **Add file** → **Upload files**.
2. Drag the entire contents of the `landing-page/` folder in.
3. Commit message: `Initial landing page`.
4. Commit.

GitHub will preserve the `assets/` folder structure automatically when you upload via drag-and-drop (unlike single-file uploads, which strip paths).

### B.3 — Connect Cloudflare Pages

1. **dash.cloudflare.com** → **Workers & Pages** → **Create application** → **Pages** → **Connect to Git**.
2. Authorize Cloudflare to access your GitHub account if prompted. Select the `cryptobasis-ai` org.
3. Pick the repo (`cryptobasis-ai` or whatever you named it).
4. **Set up builds and deployments**:
   - **Project name**: `cryptobasis-ai`
   - **Production branch**: `main`
   - **Framework preset**: **None** (it's just static HTML)
   - **Build command**: leave empty
   - **Build output directory**: leave empty (or `/`)
5. Click **Save and Deploy**.
6. After deploy completes, follow steps 8–12 from Option A to attach the custom domain.

From now on, any commit to `main` triggers a new deploy in ~30 seconds.

---

## Verification checklist

Once the page is live at `cryptobasis.ai`:

- [ ] Page loads at `https://cryptobasis.ai` (not just the `.pages.dev` preview)
- [ ] Page loads at `https://www.cryptobasis.ai` (should redirect or serve)
- [ ] Wordmark renders (check both light and dark by toggling your system theme)
- [ ] Favicon shows in the browser tab (might take a hard reload — Cmd-Shift-R)
- [ ] Open Graph preview works: paste `https://cryptobasis.ai` into Bluesky's "compose post" box and confirm the card shows the wordmark + tagline
- [ ] HTTPS is active (lock icon in the address bar, no warnings)
- [ ] Mobile renders correctly — load on your phone

---

## Common issues

**The page loads but the wordmark is broken.**
Check the browser dev console (right-click → Inspect → Console). If it's a 404 on `/assets/wordmark-light.svg`, the `assets/` folder didn't upload correctly. Re-upload making sure the folder structure is preserved.

**OG image preview doesn't work on Bluesky / X / LinkedIn.**
Each platform caches OG images aggressively. After the first time someone scrapes your URL, the result is cached for hours-to-days. Use a debugger to force re-scrape:
- Bluesky: post the link, then delete and repost — usually refreshes
- LinkedIn: [linkedin.com/post-inspector](https://www.linkedin.com/post-inspector/)

**SSL certificate warning.**
Cloudflare's edge cert can take up to 15 minutes to provision after attaching the custom domain. Wait, then hard-reload.

**`www.cryptobasis.ai` doesn't work.**
Make sure both apex (`cryptobasis.ai`) and `www` subdomain are added as custom domains in the Pages project. Cloudflare handles the redirect between them automatically.

---

## Updating the page later

**Option A (direct upload):** Replace files via dash.cloudflare.com → your project → **Create deployment** → upload again.

**Option B (GitHub-connected):** edit files in the repo, commit to `main`, deploy happens automatically.
