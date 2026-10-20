# Advantien Web Pages

Static campaign and landing pages, deployed to Netlify at **go.advantien.com**.
The main site stays on Squarespace; this repo hosts the pages Claude builds,
with automatic deploys on every push and preview URLs on every pull request.

## Pages

| Path | Purpose |
|---|---|
| `/impact/` | Financial Impact landing page — destination for the printed QR code (`advantien.com/impact` redirects here) |
| `/impact/thanks/` | Form confirmation page |

Each page is a single self-contained HTML file (styles scoped under `.adv-impact`,
fonts via Google Fonts, images embedded). No build step — what's in the repo is
what's served.

## One-time setup (~20 minutes, needs account logins)

### 1. Netlify

1. Create a Netlify account (or team) with a company email — free tier is fine.
2. **Add new site → Import an existing project → GitHub**, and install the
   Netlify GitHub App **on the `advantien` organization** (not a personal
   account — see the GitHub migration checklist), granting access to this repo.
3. Build settings: leave **Build command** empty, set **Publish directory** to `.`
   (repo root). `netlify.toml` already says this.
4. Deploy. The site gets a `*.netlify.app` URL immediately.

### 2. Forms

The `/impact/` form uses **Netlify Forms** (the `data-netlify="true"` attribute).
After the first deploy:

1. Site settings → Forms → verify the form **impact-lead** was detected.
2. Forms → **Form notifications → Add notification → Email** →
   `jcornely@advantien.com` (add others as needed).
3. Submit a test through the live page and confirm the email arrives.
   A honeypot field (`bot-field`) is in place for spam.

Free tier includes 100 submissions/month; usage above that prompts an upgrade.

### 3. Domain

1. Netlify: Site settings → Domain management → Add domain → `go.advantien.com`.
2. Squarespace (where advantien.com DNS lives): Settings → Domains → DNS →
   add a **CNAME** record: host `go`, value `<your-site>.netlify.app`.
3. Back in Netlify, confirm the domain and enable HTTPS (automatic, Let's Encrypt).

### 4. Pretty URL for print

Squarespace: Settings → Developer tools → **URL mappings**, add:

    /impact -> https://go.advantien.com/impact/ 301

The printed QR encodes `advantien.com/impact`, so this line is what makes it
resolve. Test the full chain before anything ships: scan → redirect → page → form.

## Day-to-day workflow

- **Publish a change:** push to `main` → Netlify deploys in ~30 seconds.
- **Review first:** open a pull request → Netlify posts a **Deploy Preview URL**
  on the PR → share it for sign-off → merge to go live.
- **New page:** add `<name>/index.html`, push, then (optionally) add a
  Squarespace URL mapping for a pretty `advantien.com/<name>` address.
- **Local preview:** open the HTML file in a browser, or `npx serve .`
  (Netlify Forms only work on the deployed site, not locally.)

## Conventions

- One directory per page, `index.html` inside, self-contained.
- Styles scoped under a page-specific class (`.adv-impact`) so pages can't
  bleed into each other.
- House style tokens (navy `#0f3251`, teal `#12798b`, orange `#c0682f`, Inter)
  — see the Advantien project doc `claude/house-style.md`.
- Disclaimers and figures must match the current printed collateral; the
  source of truth for numbers is the Financial Impact one-pager.
