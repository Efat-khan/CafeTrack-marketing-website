# CafeTrack — marketing website

A single-page, bilingual (Bangla-first) marketing site whose only job is to get a
gaming-cafe owner to message you on WhatsApp and book a 10-minute demo.

It is one self-contained `index.html` plus an `assets/` folder. No build step, no
framework, no npm install. Open the file, or upload the folder anywhere.

```
index.html                    the whole site (markup + CSS + JS + translations)
assets/fonts.css              @font-face rules for the self-hosted fonts
assets/fonts/*.woff2          Noto Sans Bengali + Inter, Bangla/Latin subsets only
assets/img/*.svg              labelled screenshot placeholders — replace these
tools-regenerate-fonts.py     only needed if you change font weights
```

---

## 1. Fill these in before you publish

Everything you must supply lives in one `CONFIG` object near the bottom of
`index.html`. Search the file for `CONFIG = {`. Nothing else needs editing.

| Field | What to put | Status |
| --- | --- | --- |
| `whatsapp` | `8801XXXXXXXXX` — no `+`, no spaces | **Required.** Every WhatsApp button is dead until this is real. The browser console warns you while it is still a placeholder. |
| `phoneDisplay` | How the number reads on the page, e.g. `+880 1712-345678` | Required |
| `facebook` | Your Facebook page URL | Optional — left empty, the footer link greys out |
| `email` | The email you want shown | Optional — left empty, the footer row is removed |
| `site` | Your domain once pointed here | Required for correct SEO/OG tags |
| `formEndpoint` | Formspree / Netlify Forms target — see §4 | Optional — see the fallback below |

There are also four `TODO (Efat)` comments in the `<head>` where the placeholder
domain `cafetrack.example.com` appears in the canonical, OG and Twitter tags.
Replace those with your real domain.

### What I deliberately did **not** invent

Per your honesty rules, nothing on this page states a fact I could not verify:

- No testimonials, quotes, names, photos or cafe logos.
- No statistics, ratings, customer counts or "trusted by" logos.
- No claim of a mobile app — the FAQ says plainly it is a website that works on
  a phone.
- No bKash/Nagad or payment-gateway claim — copy says the system *records* that
  a payment arrived by phone.
- No SMS/push-notification claim. No AI claim.
- Where testimonials would sit, there is the **"শুরুর ক্যাফে হয়ে যান" /
  "Be an early cafe"** section, which says outright that the product is new and
  has no customers yet.

Two things I could not decide for you, so they are marked rather than filled:

1. **Pricing.** You had not fixed a price, so the pricing section renders the
   single fallback card from your brief — *"প্রথম ৫টি ক্যাফের জন্য ফ্রি — কথা বলে
   ঠিক করব"* — with the WhatsApp button. When you decide, edit the `pricing.*`
   keys in the translations object; if you want two or three cards instead of
   one, the `TODO (Efat)` comment above `<section id="pricing">` marks the spot.
2. **FAQ answers.** All eight are honest first drafts, each marked with a
   `TODO: confirm` comment in the markup above it. Read each one and correct it —
   especially **"ডাটা কোথায় থাকে?"**, which currently says the specifics get
   covered on the demo call, because I do not know your hosting arrangement.

---

## 2. Screenshots you need to take

Every image on the site is a placeholder SVG that states, on the image itself,
which screen to capture and at what width. Replace each file **keeping the same
filename**, and the site picks it up with no code change. Keep the same aspect
ratio (1440×900, or 390×844 for the phone shot) so nothing shifts.

Take them from your own running system with realistic but **not real** customer
names. Save as JPG or PNG, then rename to match — or keep the `.svg` filename by
updating the `src` in `index.html` if you prefer real extensions.

| # | File | Product screen | Capture at | Must show |
| --- | --- | --- | --- | --- |
| 1 | `shot-1-floor-board.svg` | Dashboard — floor board | 1440px | 3–4 devices busy, the rest free; player name, elapsed timer, running bill, progress meter |
| 2 | `shot-2-end-session-bill.svg` | End session dialog | 1440px | The itemised bill: time, block, rate, rounding, discount, **To collect** |
| 3 | `shot-3-station-rates.svg` | Stations | 1440px | One device's per-controller rates (৳100 / ৳120) and rounding settings |
| 4 | `shot-4-daily-summary.svg` | Daily summary sheet | 1440px | Takings by device type, expenses by category, drawer movements, payment split |
| 5 | `shot-5-heatmap.svg` | Analytics | 1440px | The 7 × 24 peak-hours heatmap plus device utilisation bars |
| 6 | `shot-6-shift-drawer.svg` | Shifts — drawer at close | 1440px | The expected-cash arithmetic line by line vs counted cash |
| 7 | `shot-7-qr-checkin.svg` | Public QR check-in page | **390px (phone)** | What a customer sees after scanning a device's QR |

**Shot 1 appears twice** (hero and feature 01), so it is the one worth getting
right first.

### The OG image

`assets/img/og-image.svg` is a 1200×630 placeholder for the link preview shown
when someone shares the site on Facebook or WhatsApp. Export a real one
containing: the CafeTrack wordmark, the headline *"আপনার গেমিং ক্যাফে চলছে
অনুমানে, নাকি হিসাবে?"*, and a cropped corner of the floor board. Save it as
`og-image.jpg` under 200KB and update the three `og:image` / `twitter:image`
tags in the `<head>`.

Also update each screenshot's **alt text** if your real screens differ — the alt
strings are the `shots.alt1`–`shots.alt7` keys in the translations object, and
they describe what the screen shows for anyone who cannot see the image.

---

## 3. Editing the wording

All copy — both languages, every visible string including alt text, button
labels and the pre-filled WhatsApp message — lives in a single `T` object in
`index.html`, marked:

```
   2) TRANSLATIONS — edit wording here, never in the markup above.
```

`T.bn` and `T.en` have identical keys. Change a value, save, reload. You never
need to touch the markup to fix wording. If you add a key to one language, add
it to the other or that string stays blank when the visitor switches.

The language toggle remembers the choice in `localStorage` and honours the
browser language on a first visit (Bangla for `bn-*` browsers, English
otherwise). It swaps the `<html lang>`, the page title and the meta description
along with the text.

---

## 4. Where the contact form goes

Right now `CONFIG.formEndpoint` is empty, and the form has a deliberate
fallback: on submit it opens WhatsApp with the four fields (নাম, ক্যাফের নাম,
ফোন, এলাকা) already typed into the message, so nothing a visitor entered is
lost. That works fine as a permanent choice.

If you would rather collect them by email:

- **Formspree** — sign up, create a form, paste the endpoint
  (`https://formspree.io/f/xxxxxxxx`) into `CONFIG.formEndpoint`. Submissions
  arrive in your inbox. Free tier is 50/month.
- **Netlify Forms** — if you host on Netlify (§5), instead add
  `netlify` and `name="contact"` attributes to the `<form>` tag and Netlify
  captures submissions with no endpoint at all.
- **Google Forms** — possible but fiddly (you need the `formResponse` URL and
  the `entry.NNNN` field IDs); Formspree is less work.

---

## 5. Hosting, and pointing a domain at it

Drag the whole folder onto [app.netlify.com/drop](https://app.netlify.com/drop) —
it is live in about ten seconds on a free HTTPS URL. Then, in **Site settings →
Domain management**, add your domain and set the two DNS records Netlify shows
you at your registrar. Cloudflare Pages and GitHub Pages work exactly as well;
the site is static files, so any of them is fine.

After the domain is live, update `CONFIG.site` and the four `cafetrack.example.com`
occurrences in the `<head>`, or the SEO and link-preview tags will point at the
placeholder domain.

---

## 6. What was built to your spec, and what I checked

Verified in a real browser at 360px and 1440px, in both themes and both
languages:

- **No horizontal scrolling** at 360px — `document.scrollWidth` equals the
  viewport at every scroll position.
- **Contrast** — every body/muted text pair measured against its actual
  background: lowest ratio is 4.99:1 in light and 5.31:1 in dark, both above the
  WCAG AA threshold of 4.5:1.
- **Semantics** — exactly one `<h1>`, no skipped heading levels, alt text on
  every image, visible focus rings, the FAQ accordion and language toggle
  operable by keyboard.
- **Weight** — about 385KB over the wire for the first view with gzip on the
  HTML (the HTML itself gzips to 28KB), inside your 500KB target. Images are
  lazy-loaded below the fold, there is no video and no JS library.
- **Bangla rendering** — confirmed Noto Sans Bengali actually loads and that
  conjuncts render correctly. Noto is in *both* font stacks on purpose: the
  header button stays "WhatsApp করুন" even in English mode, and Inter has no
  Bengali glyphs, so without that the label would drop to a system serif.

Two judgement calls worth knowing about:

- **Theme default.** Your brief asked for dark as the default *and* for the
  light theme to follow the OS setting. I read that as: dark is the identity and
  the fallback, but an OS that explicitly asks for light gets light. The header
  control overrides either way and the choice is remembered. If you want dark
  always regardless of OS, delete the `@media (prefers-color-scheme: light)`
  block in the CSS.
- **Fonts are self-hosted** rather than loaded from Google. Your brief allowed
  either; self-hosting removes two third-party DNS/TLS round trips on a mid-range
  Android over mobile data, and guarantees Bangla never falls back to a serif.

Per your closing caution, there is **no live demo login** anywhere on the site.
If you later want to show the product without a meeting, record a 90-second
screen video and link it — do not put shared credentials on a public page.
