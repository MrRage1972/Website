# RNV-TECH LLC Website — Technical Handoff

_Last updated: 2026-06-16_

## 1. Overview

Marketing site for **RNV-TECH LLC** and its flagship app **MySecretary: Voice to action™**
(a voice-first AI productivity app). Static site, no framework, no build step — plain
HTML/CSS/JS served directly.

- **Live domain:** https://rnv-tech.com
- **GitHub Pages URL:** https://mrrage1972.github.io/Website/
- **Repo:** `MrRage1972/Website`
- **Owner contact:** ragebettis@gmail.com / Support@rnv-tech.com

## 2. Architecture

Static multi-page site. Every page shares the same `<nav>` and `<footer>` markup
(copy-pasted, not templated — edits must be applied to all pages).

```
/
├── index.html          Home (hero, about, MySecretary teaser, founder teaser, CTA)
├── mysecretary.html    Product page (hero, features, audience, privacy, MySuite grid)
├── founder.html        Founder story (Michael Bettis)
├── contact.html        Contact form + Launch List signup form
├── privacy-terms.html  Privacy Policy + Terms of Service (legal)
├── css/style.css       Single global stylesheet (CSS variables, no preprocessor)
├── js/main.js          Nav scroll, mobile menu, scroll-reveal, Formspree form submit
├── images/             logo.png, favicon.png, founder.jpg, mysecretary-logo.png,
│                       mysuite-logo.png, rnv-emblem.png
├── CNAME               Contains "rnv-tech.com" (GitHub Pages custom domain)
└── README.md
```

**Key CSS facts**
- Brand color via `--gold` (#d4af37 family); dark theme on `--black`.
- Fonts: Playfair Display (headings) + Inter (body), loaded from Google Fonts.
- Icon system: **inline SVG line icons (Lucide family) in brand gold**, sized per
  container via `.<container> svg { width/height }` rules. No icon font, no emoji.
- Footer social icons (`.footer-social`) + contact-card social icons use the same SVG style.

**Key JS facts (`js/main.js`)**
- `FORMSPREE_SIGNUP` and `FORMSPREE_CONTACT` both = `'xeedkoyl'` (same Formspree form;
  submissions distinguished by hidden `_subject` field on each form).
- Forms POST to `https://formspree.io/f/xeedkoyl` via fetch; button shows inline
  success/error state.

## 3. Hosting & DNS (current, working)

- **Hosting:** GitHub Pages, deploying from `main` branch, `/ (root)`.
- **Domain:** rnv-tech.com — GoDaddy DNS A records point to GitHub Pages IPs
  (185.199.108–111.153); `www` CNAME → `mrrage1972.github.io`. Custom domain set in
  Pages settings; `CNAME` file in repo.
- **Cutover confirmed:** rnv-tech.com serves the new site. WordPress no longer serves it.
- The old site was **GoDaddy Managed WordPress** (locked platform — could not upload
  static files, which is why we moved to GitHub Pages).

## 4. Current State

Site is **live and fully cut over**. All work merged to `main` via squash PRs (#1–#10).
Completed so far:
- Removed all Kickstarter content; replaced with Launch List / early-access messaging.
- Added brand assets (LLC logos, founder portrait, favicon, emblem).
- Built dedicated founder page from the live-site story.
- Footer: full brand tagline + Privacy & Terms link on every page.
- Source-level copyright comments in all HTML/CSS/JS.
- Instagram (@rnv_tech) + TikTok (@rnv_tech2026) links — contact cards + footer icons.
- Replaced ALL emoji site-wide with gold Lucide line icons.
- Wired up Formspree forms (live ID `xeedkoyl`).
- Rebranded product to **"MySecretary: Voice to action™"** across all pages.

## 5. Blockers / Needs User Action

1. **Formspree first-submission activation** — Formspree emails the owner address to
   confirm the form on the FIRST real submission. Until that link is clicked, live
   submissions won't deliver. User needs to submit once and click the confirmation email.
2. **"Enforce HTTPS"** in GitHub Pages settings — confirm the checkbox is ticked now that
   the domain has verified.
3. **Cancel WordPress hosting renewal** — safe to stop renewing the GoDaddy Managed
   WordPress plan. **Keep the domain registration.** (Not yet done.)
4. **ClickUp logging** — user wanted today's work logged to ClickUp, but the ClickUp MCP
   tool returned "MCP tool call requires approval" and failed. Needs the integration
   authorized before it can be written to.

## 6. Next Steps (planned)

- **Swap the app mockup for real screenshots.** The "MySecretary — Recording voice
  note…" card (`.app-mockup` in index.html + mysecretary.html) is a stylized placeholder.
  User will provide real app screenshots to replace it. The mockup title bars currently
  read "MySecretary™" (short, intentional).
- Log project status to ClickUp once the integration is authorized.

## 7. Key Design Decisions

- **Static site over WordPress** — GoDaddy Managed WordPress is locked; GitHub Pages is
  free, version-controlled, and lets us serve hand-built HTML.
- **Product name "MySecretary: Voice to action™"** — applied to page titles, meta
  descriptions, hero, section headings, and body copy. **Kept SHORT as "MySecretary™"**
  in: nav menu links, footer links, "Explore MySecretary™" CTA buttons, app-mockup title
  bars, and the MySuite product-card title (keeps menus/buttons/grid compact). ™ placed at
  the end of the full phrase per owner preference.
- **Trademark style:** `MySecretary: Voice to action™` (™ at end of full phrase).
- **Icons:** inline SVG (Lucide) in gold, not emoji and not an icon font — scalable,
  recolorable via `currentColor`, no external assets. SVG is the standard for any future
  custom icons.
- **Forms:** Formspree (free tier, dashboard for exporting the Launch List). Single form
  ID for both forms, separated by `_subject`. Chosen over Web3Forms for the export dashboard.
- **Footer tagline (all pages):** "RNV-TECH LLC builds executive-grade software and AI
  productivity apps that turn voice input into faster business execution and clearer daily
  output."
- **Legal page wording:** privacy-terms.html WAS initially kept on the short name, then
  (per owner request) updated to the full "MySecretary: Voice to action™" everywhere except
  nav/footer links.

## 8. Workflow Conventions

- **Working branch:** `claude/relaxed-clarke-v7x46o`. Develop here, then open a PR to
  `main` and **squash-merge**. (Squash means branch commit SHAs diverge from main but
  content is identical — this is expected, not a problem.)
- **Shared nav/footer are duplicated per page** — apply any nav/footer change to all 5
  HTML files.
- **Verification:** before pushing visual changes, render locally with
  `python3 -m http.server 8077` + Playwright (`playwright-core` + headless Chromium at
  `/opt/pw-browsers/.../headless_shell`) and screenshot to confirm.
- **Editing assets:** Python Pillow used for image optimization/format conversion.
- GitHub operations go through the GitHub MCP tools (no `gh` CLI in this environment).
