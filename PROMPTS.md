# Global Swift Courier: Launch Task Archive and Prompt Pack

**The job (owner's request, 2026-11-04):** move this working logistics platform to **Global Swift Courier** on
**globalswiftcourier.com** (Hostinger Node.js app). Only the logo and name change, with what follows from them (tagline, domain, email,
ID prefix). **These stay the same:** colour palette, fonts, layout, every photo, every page and every feature.

> **Fresh site, fresh identity.** This site gets a **new GitHub repo**, a **new Hostinger Node.js app**, a **new empty
> database** and **new admin credentials**. The previous brand's name, domain, email, repo and app are not mentioned in
> code, docs or metadata, and nothing from this site is pushed to the previous repo or app.

---

## The request, as given

> Transfer the content of the current site to the new domain, globalswiftcourier.com. The site name will be Global Swift
> Courier; use GSC as the tagline. The new logo is `images/Global Swift Courier Logo.png`. No code changes, just change
> the logo and name.

## Project values

| Key | Value | Source |
|---|---|---|
| Name / legal name / short name | **Global Swift Courier** | Owner |
| Tagline | **GSC** (site metadata, footer, admin). Home headline stays "Fast • Safe • Reliable" | Owner |
| Domain | **globalswiftcourier.com** (`www.` redirects to the root) | Owner |
| Email | **info@globalswiftcourier.com** | Owner |
| Admin console | **https://globalswiftcourier.com/admin/** · locally `http://localhost:3000/admin/` and `http://localhost:3000/#/admin` | Owner |
| Admin console name | **Global Swift Courier Operations Console** (from `COMPANY_SHORT`) | Derived |
| Tracking ID | **`GS` + 6 characters = 8 total**, e.g. `GS7K2M9P`; piece labels `GS7K2M9P-01` | Derived, to confirm |
| Reference prefixes | `GS-TKT-######`, `GS-INV-######`, `GS-SL-######`; quotes keep `QR-YYYY-#####` | Derived, to confirm |
| Logo source | `images/Global Swift Courier Logo.png`: 2128×739, transparent PNG | Owner |
| Palette, fonts, photos | **Unchanged** | Owner |
| DB file (default) | `globalswiftcourier.db` (production uses `DB_PATH`). Fresh, empty. Table and column names unchanged. | Derived |
| Internal code names **kept** | CSS `--ngf-*`, classes `ngf-*`, storage keys `ngf_*`, session cookie `ngf.sid`. Users never see them. | Existing convention |
| Live chat | **On**, with this site's own key (`SMARTSUPP_KEY`); named in the privacy policy | Owner |
| Phone / WhatsApp / HQ address / social | **Not supplied. Empty in `brand.ts`, so the UI hides them.** Never invent them. | Needs owner |

---

## Status

Local work is on branch `gsc-main`, pushed to `main` of github.com/webspectron/globalswift. Details are in the change log of `docs/PROJECT_TRACKER.md`.

- [x] Logo, reversed logo, web logos, mark, favicons, app icons and OG image rebuilt from the new artwork.
- [x] Name, domain, email, IDs, references, DB file, metadata, documents, tests and docs swept.
- [x] Admin console stays at `/admin/`.
- [x] Production-mode check on a scratch database.
- [x] Owner confirmed the email and Home headline and supplied the Smartsupp key.
- [ ] Owner to confirm the `GS` prefix.
- [ ] Prompt 1: new admin password and session secret.
- [ ] Prompt 2: new GitHub repo and Hostinger deploy.

---

### Prompt 1: New admin password and session secret (mostly me)
```
Set a new admin password and session secret for Global Swift Courier. Configuration only; don't edit auth logic, and never ask me for the password.

1. Give me the two commands to run MYSELF in the project folder:
   - node -e "console.log(require('bcryptjs').hashSync('MY-NEW-PASSWORD', 12))"
   - node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
2. Tell me exactly what to put in my local .env (ADMIN_PASSWORD_HASH, SESSION_SECRET), whether the `$` characters in the hash need quoting, and remind me the same values go into Hostinger in Prompt 2. They must be NEW values, not ones used by any previous site.
3. After I say "done", restart and have me confirm: the old password is rejected, the new one works at http://localhost:3000/admin/, and logout/login works.
STOP.
```

### Prompt 2: New GitHub repo and deploy to globalswiftcourier.com on Hostinger (mostly me in hPanel)
```
Prepare the deploy per docs/DEPLOYMENT.md. Don't deploy anything yourself; give me exact steps. This is a NEW site with a NEW repo, app and database.

1. GitHub, fresh history: check .gitignore excludes node_modules, dist, dist-server, data, .env, then give me the commands: `git checkout --orphan gsc-main`, `git add -A`, commit "Global Swift Courier: initial release", `git branch -M main`, `git remote remove origin`, `git remote add origin <MY NEW EMPTY REPO URL>`, `git push -u origin main`. Ask me for the repo URL; don't guess it. Before pushing, confirm `git remote -v` shows ONLY the new repo and the new main has exactly one commit.
2. hPanel → new Node.js web app: connect the new repo, branch main, Node 22.x (≥ 22.5), install `npm install` (postinstall builds; don't build twice), start `npm start` (entry dist-server/server/index.js).
3. Environment variables: NODE_ENV=production, PORT only if hPanel requires it, ADMIN_PASSWORD_HASH and SESSION_SECRET (from Prompt 1), DB_PATH=/home/<user>/globalswiftcourier-data/globalswiftcourier.db and UPLOADS_DIR=/home/<user>/globalswiftcourier-data/uploads (a folder OUTSIDE the deploy directory; create it first), SEED_DEMO_DATA unset. The database starts empty.
4. Domain: globalswiftcourier.com on this app, www redirecting to the root (one host, so the admin session cookie works), SSL on both, force HTTPS; DNS records if the domain is registered elsewhere.
5. Smoke test on the live domain (DEPLOYMENT §7): new logo, name and tab icon; the share preview uses globalswiftcourier.com and the OG image loads; https://globalswiftcourier.com/admin/ shows the login and works with the new password; /admin redirects to /admin/; /#/admin shows Home; /admins shows the 404 page; robots.txt and sitemap.xml use globalswiftcourier.com and don't mention the admin path; log in, then open /api/diag/storage and confirm the data folder; create → track a GS shipment; redeploy once and confirm the shipment is still there; response headers show compression and the /assets cache rule.
6. Email: create the info@globalswiftcourier.com mailbox and add SPF/DKIM/DMARC.
STOP and wait for my results, then tick the launch tasks in the tracker and commit "docs: Global Swift Courier live".
```

---

## Utility prompts

### Prompt R: Resume in a new chat
```
Resume the Global Swift Courier launch. Read CLAUDE.md, PROMPTS.md and docs/PROJECT_TRACKER.md, run `git status`, `git branch` and `git log --oneline -10`, then tell me which prompt we're on and what's next. Don't change anything until I confirm.
```

### Prompt F: Fix something that broke
```
Something broke: [what you see, which page/URL, steps, any error message].
Find the root cause (check the latest commits first) and explain it before fixing. Apply the smallest fix that doesn't change design or unrelated behaviour. Confirm `npm run build` and `npm test` pass and that admin login at /admin/ → create → track (GS ID) still works, then commit "fix: …".
```

### Prompt O: Owner contact details arrived
```
Here are Global Swift Courier's contact details: [phone / WhatsApp / HQ address / social links].
Add them to src/config/brand.ts (and the JSON-LD in index.html), confirm the UI that was hidden now shows them correctly on the Header, Footer, Contact and documents, update the values table in PROMPTS.md and the tracker, build, and commit "content: add Global Swift Courier contact details".
```
