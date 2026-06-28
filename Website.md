# RNV-TECH LLC Website — Project Reference

_Last updated: 2026-06-28. This is the single source of truth for the website project._
_(Supersedes the earlier HANDOFF.md.)_

## 1. Overview

Marketing site for **RNV-TECH LLC** and its flagship app **MySecretary: Voice to action**
(a voice-first AI productivity app). Static site — plain HTML/CSS/JS, no framework, no
build step, served directly.

- **Live domain:** https://rnv-tech.com
- **GitHub Pages URL:** https://mrrage1972.github.io/Website/
- **Repo:** `MrRage1972/Website`
- **Owner:** Michael Bettis — ragebettis@gmail.com / Support@rnv-tech.com

## 2. Architecture

Static multi-page site. Every page shares the same `<nav>` and `<footer>` markup
(copy-pasted, NOT templated — any nav/footer edit must be applied to all 5 pages).

```
/
├── index.html          Home (hero, about, MySecretary teaser, founder teaser, CTA)
├── mysecretary.html    Product page (hero, features, audience, privacy, MySuite grid)
├── founder.html        Founder story (Michael Bettis)
├── contact.html        Contact form + Launch List signup form
├── privacy-terms.html  Privacy Policy + Terms of Service (legal)
├── reset.html          Supabase password-reset page for the MySecretary app (see §5)
├── css/style.css       Single global stylesheet (CSS variables, no preprocessor)
├── js/main.js          Nav scroll, mobile menu, scroll-reveal, Formspree form submit
├── images/             logo.png, favicon.png, founder.jpg, mysecretary-logo.png,
│                       mysuite-logo.png, rnv-emblem.png
├── CNAME               Contains "rnv-tech.com" (GitHub Pages custom domain)
├── Website.md          THIS doc
└── README.md
```

**Key CSS facts**
- Brand color via `--gold` (#d4af37 family); dark theme on `--black`.
- Fonts: Playfair Display (headings) + Inter (body), from Google Fonts.
- Icon system: **inline SVG line icons (Lucide family) in brand gold**, sized per
  container via `.<container> svg { width/height }` rules. No icon font, no emoji.
- Footer social icons (`.footer-social`) + contact-card social icons use the same SVG style.
- Nav collapses to a hamburger menu below ~1100px (mobile menu stacks vertically).

**Key JS facts (`js/main.js`)**
- `FORMSPREE_SIGNUP` and `FORMSPREE_CONTACT` both = `'xeedkoyl'` (same Formspree form;
  submissions distinguished by hidden `_subject` field on each form).
- Forms POST to `https://formspree.io/f/xeedkoyl` via fetch; button shows inline
  success/error state.

## 3. Hosting & DNS (current, working)

- **Hosting:** GitHub Pages, deploying from `main` branch, `/ (root)`.
- **Domain:** rnv-tech.com — GoDaddy DNS A records → GitHub Pages IPs
  (185.199.108–111.153); `www` CNAME → `mrrage1972.github.io`. Custom domain set in
  Pages settings; `CNAME` file in repo.
- **Cutover confirmed:** rnv-tech.com serves the new site. WordPress no longer serves it.
- The old site was **GoDaddy Managed WordPress** (locked platform — couldn't upload
  static files, which is why we moved to GitHub Pages).

## 4. Forms (Formspree)

- Both the Launch List signup (contact.html `#launch`) and the contact message form route
  through one Formspree form, ID `xeedkoyl`, separated by their hidden `_subject` fields.
- Chosen over Web3Forms for the export dashboard (good for the Launch List).
- **Activation note:** Formspree confirms the form on the FIRST real submission via an
  email to the owner address — that link must be clicked once for live delivery.

## 5. MySecretary password reset (reset.html + Supabase)

`reset.html` is the web page where MySecretary app users land to set a new password.
It is part of the WEBSITE repo (served at `https://rnv-tech.com/reset.html`) even though
the app itself lives in a separate project.

- **Supabase project:** `my-suite` (project ref `fefmkvhpetlzcxgddqgh`).
- In `reset.html`: `SUPABASE_URL` = `https://fefmkvhpetlzcxgddqgh.supabase.co`,
  `SUPABASE_ANON_KEY` = the **publishable** (public) key `sb_publishable_...`. This is the
  public key and is safe in a web page — NEVER put the `service_role` secret here.
  - Gotcha fixed: the key had once been pasted with a trailing `ERE` left over from the
    `PASTE_YOUR_..._HERE` placeholder, which broke it. Key must end clean.
- **Supabase Auth → URL Configuration (working config):**
  - **Site URL** = `https://rnv-tech.com/reset.html`  ← password-reset emails fall back
    here. This is required because the app does NOT pass a `redirectTo`, so Supabase uses
    the Site URL. (Setting Site URL to the homepage made reset links open the homepage —
    that was the bug.)
  - **Redirect URLs** allow-list includes `https://rnv-tech.com/reset.html`.
  - Safe because MySecretary is a native app — signup/login happen in-app, so the only
    web auth flow is this password reset.
- **Verified end-to-end:** Forgot-password email → link opens reset.html → new password →
  "Password updated" success. Working as of 2026-06-28.

## 6. Current State — DONE

Site is **live and fully cut over**. All work merged to `main` via squash PRs (#1–#16).
- Removed all Kickstarter content; replaced with Launch List / early-access messaging.
- Added brand assets (LLC logos, founder portrait, favicon, emblem).
- Built dedicated founder page.
- Footer: full brand tagline + Privacy & Terms link on every page.
- Source-level copyright comments in all HTML/CSS/JS.
- Instagram (@rnv_tech) + TikTok (@rnv_tech2026) — contact cards + footer icons.
- Replaced ALL emoji site-wide with gold Lucide line icons.
- Formspree forms wired up (ID `xeedkoyl`).
- Product renamed to **"MySecretary: Voice to action"** in EVERY instance (no short refs).
- All trademark (™) symbols removed site-wide.
- MySecretary password-reset page (reset.html) live and verified with Supabase.

## 7. Open Items / Needs User Action

1. **Formspree first-submission activation** — submit once and click the confirmation
   email so live form submissions deliver. (Verify done.)
2. **"Enforce HTTPS"** in GitHub Pages settings — confirm the checkbox is ticked.
3. **Cancel WordPress hosting renewal** — safe to stop renewing the GoDaddy Managed
   WordPress plan. **KEEP the domain registration.** (Not yet done.)
4. **ClickUp logging** — owner wanted today's work logged to ClickUp, but the ClickUp MCP
   tool returned "MCP tool call requires approval" and failed. Needs the integration
   authorized before it can be written to.

## 8. Next Steps (planned)

- **Swap the app mockup for real screenshots.** The "MySecretary — Recording voice note…"
  card (`.app-mockup` in index.html + mysecretary.html) is a stylized placeholder. Owner
  will provide real app screenshots to replace it. Mockup title bars read the full
  "MySecretary: Voice to action".
- Log project status to ClickUp once the integration is authorized.

## 9. Key Design Decisions

- **Static site over WordPress** — GoDaddy Managed WordPress is locked; GitHub Pages is
  free, version-controlled, and serves hand-built HTML.
- **Product name "MySecretary: Voice to action"** — used EVERYWHERE the product is named
  (page titles, meta, hero, headings, body, nav, footer, CTA buttons, mockup titles,
  MySuite card). Only the lowercase file/asset names `mysecretary.html` /
  `mysecretary-logo.png` keep the short form. (An earlier revision kept nav/footer/buttons
  short; owner later asked for the full name everywhere.)
- **No trademark symbols** — all ™ removed (MySecretary and MySuite). Do not reintroduce.
- **Icons:** inline SVG (Lucide) in gold — scalable, recolorable via `currentColor`, no
  external assets. SVG is the standard for any future custom icons.
- **Forms:** Formspree, single form ID for both, separated by `_subject`.
- **Footer tagline (all pages):** "RNV-TECH LLC builds executive-grade software and AI
  productivity apps that turn voice input into faster business execution and clearer daily
  output."
- **Supabase reset:** Site URL points at reset.html (not homepage) because the app sends
  reset emails without a `redirectTo`. See §5.

## 10. Workflow Conventions

- **Working branch:** `claude/relaxed-clarke-v7x46o`. Develop here → open PR to `main` →
  **squash-merge**. (Squash makes branch commit SHAs diverge from main; content is
  identical — expected. Periodic `git merge origin/main` into the branch resolves the
  recurring "merge conflict" on PR merge; take the branch's version when resolving.)
- **Shared nav/footer are duplicated per page** — apply any nav/footer change to all 5
  HTML files.
- **Verification:** before pushing visual changes, render locally with
  `python3 -m http.server 8077` + Playwright (`playwright-core` + headless Chromium at
  `/opt/pw-browsers/.../headless_shell`) and screenshot to confirm.
- **Editing assets:** Python Pillow for image optimization/format conversion.
- GitHub operations go through the GitHub MCP tools (no `gh` CLI in this environment).
- **Note:** docs at repo root (this file, README) are also served by GitHub Pages
  (e.g. rnv-tech.com/Website.md). Nothing secret is in them, but they are publicly
  fetchable. Move them out of the deploy if that's ever a concern.
