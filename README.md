# Password-protected Bose case studies — Vercel Hobby

This kit protects BOTH Bose page HTML and private images through one Vercel Function. Login uses an environment-variable password, an 8-hour signed HttpOnly Secure cookie, and no-store responses.

## Files provided

- `api/bose.js`: login/status/logout and private file delivery.
- `vercel.json`: uses `public/` as the static website directory and routes protected Bose pages through the function.
- `public/bose-password.html` + `.css`: your previously designed gate and project tiles, now connected to the server.
- `private-bose/bonbon.html` + `.css`: your BonBon case study.
- `private-bose/media/`: **YOU must copy the image files from your existing assets folder here.**

## Installation in your EXISTING portfolio repository

1. **Backup/commit your site before changes.**
2. Copy this kit's `api/`, `private-bose/`, and `vercel.json` into your repo root.
3. Create `public/` and move your existing PUBLIC website files there: `index.html`, `styles.css`, `assets/`, other public HTML, fonts, and scripts. Add the supplied `bose-password.html` and `bose-password.css` to `public/`. Note that if your current Vercel project already has its own `vercel.json`, MERGE configurations rather than overwriting other rewrites/redirects.
4. Move your **private** Bose image files (including all BonBon landing-page exports) from `public/assets/bose/` into `private-bose/media/`. The BonBon HTML is already edited to load these from `/bose/media/<filename>`. **Remove public duplicates, old publicly reachable case-study HTML, and confidential media** from public paths. Public thumbnail/banner images displayed on the public gateway will remain public; use only imagery approved for public preview. If you want those banner images private, replace the public banner with a neutral abstract/typographic treatment.
5. Add your second case study as `private-bose/in-the-home.html` with its supporting images in `private-bose/media/`; change its image references to `/bose/media/<filename>` and its CSS reference to `/bose/in-the-home.css`, and put its CSS at `private-bose/in-the-home.css`. Until you add it, the second card will lead to a 404 after login.
6. In Vercel: Project → Settings → Environment Variables. Add **`BOSE_ACCESS_PASSWORD`** (a unique 12+ character shared password) and **`BOSE_SESSION_SECRET`** (a random 32+ character signing key, generated using `openssl rand -hex 32`). Set them for Production and Preview as appropriate; NEVER put them into Git or an HTML file.
7. Verify your Vercel project uses root directory `/` and deploy via Git. If a Vercel project setting overrides Output Directory, ensure it is `public` (the supplied `vercel.json` sets it).
8. Link your homepage's Bose card to `/bose-password.html`.
9. Test in a **private/incognito window** on the actual Vercel deployment:
   - `/bose/bonbon` redirects to password page before login.
   - `/bose/media/bonbon-landing-hero.png` returns 401 before login (substitute an actual uploaded filename).
   - Wrong password fails; correct password shows the two tiles.
   - After login, both the HTML and private images load.
   - Old public paths like `/case-study-bonbon.html` and `/assets/bose/bonbon-landing-hero.png` must be 404 (unless you explicitly made a preview image public).
   - API `/api/bose?path=media/../../etc/passwd` must not expose files.

## Important precautions

- **Security is only as good as your public-file cleanup.** Anything left under `public/` is publicly available, even if the gateway is locked. Anyone with links to old deployments might still access content that was previously public. Evaluate Bose intellectual-property permissions before posting.
- This is suitable for low-volume recruiter review, not a high-security employee portal. It has no centralized per-user accounts, audit logs, or robust IP-based rate limiting. A shared password can be forwarded. For sensitive assets use a commercial auth service or authorized Vercel deployment access instead.
- You should add rate limiting via a Vercel WAF rule or an external durable store before sharing widely; basic function-only counters are unreliable on serverless infrastructure. Prefer a strong password and rotate it when needed.
- Example campaign metrics in the case study are **not verified Bose analytics** and must not be presented as factual business results until replaced.
- Environment variables must be redeployed after changes to take effect.
