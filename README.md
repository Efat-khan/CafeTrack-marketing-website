# CafeTrack — marketing website

A single-page, bilingual (Bangla-first) marketing site whose only job is to get a
gaming-cafe owner to message you on WhatsApp and book a 10-minute demo.

It is one self-contained `index.html` plus an `assets/` folder. No build step, no
framework, no npm install. Open the file, or upload the folder anywhere.

```
index.html                    the whole site (markup + CSS + JS + translations)
assets/fonts.css              @font-face rules for the self-hosted fonts
assets/fonts/*.woff2          Noto Sans Bengali + Inter, Bangla/Latin subsets only
assets/img/shot-*.svg         labelled screenshot placeholders — replace these
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
| `video.items[].youtubeId` | **Already set** — both of your videos are in. Add, remove or reorder entries to change what appears | Done — see §2b |
| `video.items[].label` | The name each picker button shows | **Worth doing.** Empty falls back to "ভিডিও ১" / "Video 1" |
| `video.items[].length` | Running time under the play button, e.g. `প্রায় ২ মিনিট` | Optional — empty hides the line. Only fill it in once you have checked the real length |
| `pricing.oneTime` | The one-time charge, written how you want it read, e.g. `৳25,000` | Optional — empty reads "কথা বলে ঠিক করব" |
| `pricing.yearlyHosting` | The yearly hosting charge, e.g. `৳6,000` | Optional — empty reads "কথা বলে ঠিক করব" |
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

## 2. Screenshots you need to take

### Replacing them is drag-and-drop

Every image starts as a labelled placeholder SVG that states, on the image
itself, which screen to capture and at what width. **To use a real screenshot,
save it into `assets/img/` with the same base name and a `.png` extension.**
That is the whole procedure — no code to edit, nothing to rename:

```
assets/img/shot-1-floor-board.png     ← your Dashboard capture
assets/img/shot-3-station-rates.png   ← your Stations capture
```

The page checks for each real file as it scrolls into view and swaps it in once
it has loaded; the "replace this file" caption underneath disappears by itself.
Anything you have not replaced yet keeps showing its placeholder, so the site
is never broken half-way through. Prefer JPEGs? Set `CONFIG.shots.ext` to
`"jpg"`. Once every shot is done you can set `CONFIG.shots.enabled` to `false`
and point the `src` attributes straight at your files.

Keep the same shape — 1440×900 for the desktop shots, 390×844 for the phone one
— so nothing shifts on the page.

Screen names below match your product's own left-hand nav.

| # | File | Product screen | Capture at | Must show |
| --- | --- | --- | --- | --- |
| 1 | `shot-1-floor-board.svg` | **Dashboard** → THE FLOOR | 1440px | **3–4 sessions actually running** — player name, elapsed timer, cost so far, progress bar |
| 2 | `shot-2-end-session-bill.svg` | **Dashboard** → End session dialog | 1440px | The itemised bill: time, block, rate, rounding, discount, **To collect** |
| 3 | `shot-3-station-rates.svg` | **Stations** | 1440px | The BY CONTROLLERS column readable end to end (1× / 2× / 3× / 4×), the MAX column, the QR Preview link |
| 4 | `shot-4-daily-summary.svg` | **Summary** | 1440px | Takings by device type, expenses by category, drawer movements, payment split |
| 5 | `shot-5-heatmap.svg` | **Analytics** | 1440px | The 7 × 24 peak-hours heatmap plus device utilisation bars |
| 6 | `shot-6-shift-drawer.svg` | **Shifts** — drawer at close | 1440px | The expected-cash arithmetic line by line vs counted cash |
| 7 | `shot-7-qr-checkin.svg` | The public QR check-in page | **390px (phone)** | What a customer sees after scanning a device's QR |

**Shot 1 appears twice** (hero and feature 01), so it is the one worth getting
right first.

### Two things that will make or break these shots

**1. Start some sessions before you capture the floor.** The Dashboard shot you
sent me had *0 of 9 devices in play* — every tile read "Free" with a Start
session button, and the In-play table said "Nothing in play." But the sentence
printed next to that image on the site promises the tile shows *who is playing,
the elapsed timer, the running bill, and the progress against booked time*. An
empty floor shows none of those, so the picture would quietly contradict the
copy. Start three or four sessions with plausible names, let a couple of minutes
run so the timers and costs are non-zero, then capture.

**2. Capture in Dark mode.** Your product's default is light with a violet
accent; this site is dark. There is a **Dark mode** toggle at the bottom of your
sidebar — screenshots taken with it on will sit properly inside the dark browser
frames instead of glowing white against them. Do all seven the same way.

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

Also update each screenshot's **alt text** if your real screens differ — the alt
strings are the `shots.alt1`–`shots.alt7` keys in the translations object, and
they describe what the screen shows for anyone who cannot see the image.

---

## 2b. The explainer videos

**Both of your videos are live** in the section between the problem cards and
the features:

| # | Video | In `CONFIG.video.items` |
| --- | --- | --- |
| 1 | `https://youtu.be/0ouIKaPdgOk` | `youtubeId: "0ouIKaPdgOk"` |
| 2 | `https://youtu.be/pz9iCrws_FY` | `youtubeId: "pz9iCrws_FY"` |

One big player with a picker underneath. Clicking a picker button swaps the
poster; if a video is already playing, it switches straight to the new one
instead of dropping the visitor back to a still frame. The picker is a proper
tab strip — arrow keys, Home and End all work, and only the selected tab is in
the tab order.

**It is a facade player.** Nothing from YouTube's *player* loads until someone
clicks — the embed is built on click and uses `youtube-nocookie.com`, so no one
is tracked by YouTube before they have chosen to watch, and the 3G budget holds.

### Please name the two videos

The picker buttons currently read **"ভিডিও ১" / "ভিডিও ২"** (and "Video 1" /
"Video 2" in English), because I have no way to know what each video covers —
YouTube is blocked from the machine I build on, so I could not watch either one.

Give each a real title in `CONFIG.video.items[n].label` and the picker uses it
in both languages, e.g.:

```js
{ youtubeId: "0ouIKaPdgOk", label: "সিস্টেম পরিচিতি", length: "" },
{ youtubeId: "pz9iCrws_FY", label: "সেশন আর বিল",     length: "" }
```

A real title sells far better than "Video 1" — this is the one thing in the
section worth five minutes of your time.

`length` is optional and shown under the button; leave it empty unless you have
checked the real running time.

### Adding, removing or replacing a video

Edit the `items` array. One entry hides the picker automatically; an empty array
hides the whole section, so a visitor never meets a play button that does
nothing. Each entry takes **either** `youtubeId` **or** `mp4` (a self-hosted
file), so you can mix the two.

### Posters

`CONFIG.video.poster` is `"auto"`, which uses YouTube's own thumbnails — no work
for you, but it means an image request to `i.ytimg.com` when the section scrolls
into view. To keep the page entirely first-party, give an item its own `poster`
file. If a thumbnail cannot be fetched the code falls back
`maxresdefault → hqdefault → assets/img/video-poster.svg`, which is a plain
CafeTrack-branded frame — deliberately free of any developer text, because a
visitor can end up seeing it.

### What I removed

The section used to carry three chips reading "সেশন শুরু আর শেষ · বিল আর রসিদ ·
দিনশেষের হিসাব". Those described what I had *suggested* recording, not what your
videos actually show, so with real videos in place they were three claims I
could not stand behind. They are gone. The poster's alt text is now simply
"<label> — CafeTrack এর ভিডিও থাম্বনেইল", which is true whatever the video
contains.

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
