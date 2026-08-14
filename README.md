# CafeTrack — marketing website

A single-page, bilingual (Bangla-first) marketing site whose only job is to get a
gaming-cafe owner to message you on WhatsApp and book a 10-minute demo.

It is one self-contained `index.html` plus an `assets/` folder. No build step, no
framework, no npm install. Open the file, or upload the folder anywhere.

```
index.html                    the whole site (markup + CSS + JS + translations)
assets/fonts.css              @font-face rules for the self-hosted fonts
assets/fonts/*.woff2          Noto Sans Bengali + Inter, Bangla/Latin subsets only
assets/img/*.png              the product screenshots shown on the page
assets/img/video-poster.svg   branded fallback frame if a YouTube thumbnail fails
assets/img/og-image.svg       1200×630 link-preview placeholder
tools-regenerate-fonts.py     only needed if you change font weights
```

---

## 1. Fill these in before you publish

Everything you must supply lives in one `CONFIG` object near the bottom of
`index.html`. Search the file for `CONFIG = {`. Nothing else needs editing.

| Field | What to put | Status |
| --- | --- | --- |
| `whatsapp` | `8801XXXXXXXXX` — no `+`, no spaces | **Set** to `8801404571271` (01404571271). All six WhatsApp buttons, both tel: links and the JSON-LD now use it |
| `phoneDisplay` | How the number reads on the page | **Set** to `01404-571271` — local form, since that is what a cafe owner here recognises. The `tel:` link still uses `+8801404571271`, so it dials from anywhere |
| `facebook` | Your Facebook page URL | Optional — left empty, the footer link greys out |
| `email` | The email you want shown | Optional — left empty, the footer row is removed |
| `site` | Your domain once pointed here | Required for correct SEO/OG tags |
| `video.items[].youtubeId` | **Set** to `pz9iCrws_FY`. Add another entry and the picker reappears with both | Done — see §2b |
| `video.items[].label` | Picker button name, and the embedded player's title | Optional while there is only one video — the picker is hidden |
| `video.items[].length` | Running time under the play button | Optional — empty hides the line. YouTube reports this video as **9:09** if you want it shown |
| `pricing.oneTime` | The one-time charge, written how you want it read, e.g. `৳25,000` | Optional — empty reads "কথা বলে ঠিক করব" |
| `pricing.yearlyHosting` | The yearly hosting charge, e.g. `৳6,000` | Optional — empty reads "কথা বলে ঠিক করব" |

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

Two things still need your input, so they are marked rather than filled:

1. **The two amounts.** The pricing model is now the one you described — a
   one-time charge to own the product, plus a yearly hosting charge, and no
   monthly billing at all. The section states that structure plainly; it just
   does not state figures, because you have not given me any. Put them in
   `CONFIG.pricing` and they appear. Until then each one reads *"কথা বলে ঠিক
   করব"* / *"We'll agree it on the call"*, which is true and sells fine.
2. **FAQ answers.** All eight are honest first drafts, each marked with a
   `TODO: confirm` comment in the markup above it. Read each one and correct it —
   especially **"ডাটা কোথায় থাকে?"**, which currently says the specifics get
   covered on the demo call, because I do not know your hosting arrangement.

### How pricing is worded

One cafe = one product. The card shows the one-time charge and the yearly
hosting charge side by side with a `+` between them, a green line reading
*"মাসিক ফি নেই · সেটআপ ফি নেই · লুকানো খরচ নেই"*, and a footnote saying that if
you have more than one branch, each branch is its own product.

That footnote is an **assumption I made** and you should check it: you said the
product serves a single cafe rather than a whole system, so I priced the unit as
one cafe and treated extra branches as extra products. If you actually meant one
purchase covers every branch an owner has, change `pricing.foot` in the
translations object and the copy is fixed.

Note this sits alongside the multi-branch feature, which is still described in
the features list — the software supports branches, the *licence* is per cafe.
If that is wrong, tell me and I will reconcile the two.

---

## 2. Screenshots

Every screenshot on the page is a real capture now — the labelled placeholder
SVGs and the swap-them-in-automatically machinery are gone, and each `<img>`
points straight at its file. Six files cover seven slots, because the dashboard
capture is used twice:

| # | File | Product screen | Shown in |
| --- | --- | --- | --- |
| 1 | `admin.png` | **Dashboard** → THE FLOOR | hero + feature 01 |
| 2 | `station.png` | **Stations** — per-controller rates | feature 02 |
| 3 | `report.png` | **Dashboard** → End session dialog | feature 03 |
| 4 | `summary.png` | **Summary** — the daily/monthly sheet | feature 04 |
| 5 | `cafetracker-qr.png` | The public QR check-in page (phone) | feature 05 |
| 6 | `analysis.png` | **Analytics** — peak hours + utilisation | feature 06 |

To swap one out, save the new capture over the same filename — or point the
`src` at a new one and update its `width`/`height` to the file's real pixel
size. Those two attributes only reserve the right space while the image loads;
if they disagree with the file, the page jumps as it loads.

### They are cropped on the page, full size on click

The captures are whole-page and very tall (`analysis.png` is 1910×2525). Shown
at full height, one screenshot would run for a screen and a half and shrink its
own detail to nothing, so each sits in a frame cropped from the top —
`--shot-max`, 440px by default, 520px for the hero — with a fade at the cut and
a **View full size** button. Clicking opens the whole image in a dialog that
closes on Escape, on the close button, or on a click outside it.

Nothing about that needs configuring. A shorter capture that already fits under
the cap simply never gets cropped.

### Two things worth fixing when you re-capture

**1. Start some sessions before you capture the floor.** The Dashboard shot on
the page has *0 of 9 devices in play* — every tile reads "Free" with a Start
session button, and the In-play table says "Nothing in play." But the sentence
printed next to that image on the site promises the tile shows *who is playing,
the elapsed timer, the running bill, and the progress against booked time*. An
empty floor shows none of those, so the picture would quietly contradict the
copy. Start three or four sessions with plausible names, let a couple of minutes
run so the timers and costs are non-zero, then re-capture.

**2. Capture in Dark mode.** Your product's default is light with a violet
accent; this site is dark. There is a **Dark mode** toggle at the bottom of your
sidebar — screenshots taken with it on will sit properly inside the dark browser
frames instead of glowing white against them. The captures on the page now are
light-mode, so this is the single biggest visual upgrade available. Do them all
the same way.

### The rate ladder on the site is real

The example rates shown in feature 02 — 1 controller ৳100, 2 → ৳120, 3 → ৳160,
4 → ৳200 — are your PS4 Booth rates, read off the Stations screen you sent, and
labelled on the page as an example. Change them in the `f2.r1`–`f2.r4`
translation keys if you would rather show a different device or round numbers.

### The OG image

`assets/img/og-image.svg` is a 1200×630 placeholder for the link preview shown
when someone shares the site on Facebook or WhatsApp. Export a real one
containing: the CafeTrack wordmark, the headline *"আপনার গেমিং ক্যাফে চলছে
অনুমানে, নাকি হিসাবে?"*, and a cropped corner of the floor board. Save it as
`og-image.jpg` under 200KB and update the three `og:image` / `twitter:image`
tags in the `<head>`.

Also update each screenshot's **alt text** if you replace a capture — the alt
strings are the `shots.alt1`–`shots.alt7` keys in the translations object, and
they describe what the screen shows for anyone who cannot see the image. They
double as the caption under the full-size view.

---

## 2b. The explainer video

**Live**, using `https://youtu.be/pz9iCrws_FY` (`youtubeId: "pz9iCrws_FY"`).
The section sits between the problem cards and the features.

Video 1 has been removed at your request. The section handles that on its own:
with a single video the picker strip hides itself and you just get the player.
Add a second entry to `CONFIG.video.items` and the picker comes back, as a
keyboard-operable tab strip (arrow keys, Home, End).

**It is a facade player.** Nothing from YouTube's *player* loads until someone
clicks — the embed is built on click and uses `youtube-nocookie.com`, so no one
is tracked by YouTube before they have chosen to watch, and the 3G budget holds.

### Optional bits

- **Running time.** Left empty, no length is shown. From the frame you sent me
  YouTube reports the video as **9:09** — set `length: "৯ মিনিট"` if you want
  that under the play button. Your call: it is honest either way, and a nine
  minute runtime may or may not help the click.
- **Label.** With one video the picker is hidden, so `label` only becomes the
  embedded player's title. It matters again if you add a second video.
- **Poster.** `CONFIG.video.poster` is `"auto"`, which uses YouTube's own
  thumbnail — no work for you, but one image request to `i.ytimg.com` when the
  section scrolls into view. Point it at your own file to keep the page fully
  first-party. If a thumbnail cannot be fetched it falls back
  `maxresdefault → hqdefault → assets/img/video-poster.svg`, a plain
  CafeTrack-branded frame with no developer text on it, since a visitor can end
  up seeing it.

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

## 4. How people get in touch

There is no contact form. Every route on the page is a direct one: the
WhatsApp buttons (hero, final CTA and the floating button), and the phone line
in the final CTA that dials `CONFIG.whatsapp` through a `tel:` link. Both are
driven by `CONFIG.whatsapp` / `CONFIG.phoneDisplay`, so setting the number once
updates all of them.

A cafe owner reaching you on WhatsApp starts a conversation you can answer the
same minute, which is the point — nothing to check an inbox for.

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

### The palette

The site uses the dark crimson scheme you asked for, matching the reference you
sent: a near-black warm background (`#0a0508`) with a red bloom behind the hero,
crimson (`#e8192f`) as the single primary accent, and a magenta→violet ramp
(`#ff2d55 → #d21ea8 → #8b2ae0`) used sparingly — only on the pricing rows, the
video panel border, and the underline under the red word in a heading. Emerald
stays for money and "no monthly fee", amber only for warnings. Every colour is a
CSS variable at the top of the `<style>` block, so the whole scheme is about ten
lines to change.

**WhatsApp green is deliberately left alone.** The buttons that open WhatsApp
keep WhatsApp's own green rather than going crimson, because that green is what
makes a cafe owner recognise "this messages me on WhatsApp" without reading. If
you would rather they matched the palette, change `--wa` in the tokens — but I
would keep it.

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

Per your closing caution, there is **no live demo login** anywhere on the site —
the explainer video (§2b) is what shows the product without a meeting. Keep it
that way: do not put shared credentials on a public page.
