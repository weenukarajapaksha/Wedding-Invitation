# Ahinsa & Iminda — Wedding Invitation

A single-page digital wedding invitation. Static HTML/CSS/JS — no build step, no
server, nothing to `npm install`. Tailwind, the web fonts and the dotLottie
player are pulled from CDNs at page load, so the page needs a network connection.

## The details

| | |
|---|---|
| Couple | Ahinsa (daughter of Mr. & Mrs. Vithanage) and Iminda (son of Mr. & Mrs. Liyanage) |
| Date | Monday, 16 November 2026 |
| Time | 8.30 am to 3.30 pm, Poruwa ceremony at 8.30 am |
| Venue | Hotel Grand Palace, Hikkaduwa |
| RSVP | Akash 077 322 9401, Vithanage 077 921 0709 |

## View it locally

Just open `index.html` in a browser (double-click it, or drag it into a browser window).

## What's inside

```
.
├── index.html                     # the entire site (one page)
├── inline-animations.js           # maintenance script, see below
├── assets/
│   ├── couple-animation.lottie    # bride and groom, source file
│   ├── backdrop-animation.lottie  # floral wreath behind them, source file
│   ├── animations.js              # both of the above, base-64 inlined
│   ├── venue.jpg                  # photo in the "Venue" section
│   ├── page-background.avif       # full-page backdrop photo
│   ├── page-background.jpg        # same image, fallback for browsers without AVIF
│   └── couple-illustration.webp   # UNUSED, left over from the old static hero
└── README.md
```

### Why the animations exist twice

The hero stacks two dotLottie animations: a floral wreath that draws itself on,
with the couple standing inside its opening. Browsers refuse to fetch a local
file from a page opened with `file://`, so plain `.lottie` references show
nothing when you double-click `index.html`. Both animations are therefore also
inlined as base-64 in `assets/animations.js`, which the page prefers. The
`.lottie` files are kept as the editable sources.

To swap in a different animation, replace the `.lottie` file and regenerate the
inlined copies from the project root:

```bash
node inline-animations.js
```

The two animations are sized relative to each other in CSS: `.couple-stage` is a
square, the wreath fills it, and `.couple-lottie` sits at 40% so the couple stays
clear of the wreath's inner edge. If you swap either file for art with different
proportions, that 40% is the number to adjust. The stage itself is deliberately
wider than the page gutters, via the negative margins on its wrapper, so the
wreath reads large; the wreath art has transparent padding built in, so it stays
on screen.

## Features

- Touch-to-unlock cover with an animated WebGL shader background
- Animated couple in the hero, framed by an animated floral wreath
- Full-page photographic backdrop behind everything
- Live countdown to the wedding date
- "Add to Calendar" (.ics download)
- "Get Directions" (opens the venue in Google Maps)
- RSVP form that sends the reply straight to WhatsApp, pre-filled, addressed to
  whichever of the two RSVP contacts the guest picks
- Tap-to-call and copy-phone-number for both contacts
- Fully responsive, works on both mobile and desktop

### The page backdrop

`#page-bg` in `index.html` is a fixed layer sitting behind the content and above
the ambient shader. It is scoped to the same centred 600px column as the rest of
the invitation, so on a wide screen it does not bleed into the page margins.
Its `opacity` (currently `0.2`) controls how strongly the photo reads.

Keep it low. The photo is a busy floral scene, and above roughly `0.4` it starts
to compete with the hero wreath and to wash out the small uppercase labels and
the form placeholders in the RSVP section. At `0.2` it reads as a soft tint and
everything stays legible without needing panels behind the text.

The image is supplied twice. AVIF is smaller, but iOS Safari before 16.4 cannot
display it, so a JPEG is declared first and the AVIF upgrade is applied only
where `image-set()` is understood. If you replace the backdrop, replace both
files, keeping the same two names.

### Motion

Everything is CSS transitions and animations; there is no animation library.

- **The hero entrance waits for the cover.** The names, lineage and ampersand
  rise in, and both hero animations restart from their first frame, only once
  the guest taps to unlock. They are held with `animation-play-state: paused`
  until `body` gains the `entered` class. Without this the entrance plays out of
  sight behind the overlay and is never seen.
- **Sections reveal on scroll.** Add the `reveal` class to any block and it
  fades and rises as it enters the viewport. `--reveal-delay` staggers siblings,
  as the RSVP heading, intro and form do. Reveals fire once, so scrolling back
  up never re-hides anything.
- **Reduced motion is respected.** With `prefers-reduced-motion: reduce`, the
  reveals show immediately with no transition, the hero entrance and shimmer are
  off, the card hover lift is disabled, and both hero animations hold on a
  single frame instead of looping.

Note that the reveal effect cannot be verified with `chrome --headless
--dump-dom`. That mode never paints, so IntersectionObserver callbacks and CSS
animations never run and everything looks stuck at opacity 0. Drive a real
browser instead.

## Before you share this with guests

- **Delete `assets/couple-illustration.webp`.** Nothing references it any more.
  It is a low-resolution *watermarked Shutterstock preview* that was never
  licensed, so it should not stay in a public repository.
- **Get a larger venue photo.** `assets/venue.jpg` is only 283x281 pixels, so
  the browser has to enlarge it to fill the frame and it looks soft, more so on
  a high-density phone screen. The frame height was set to suit these
  proportions. If you can get the original full-size photo, drop it in under the
  same name and raise the `h-[340px] md:h-[420px]` values on the venue block.
- Double-check the details in `index.html` are correct — search for
  `077 322 9401`, `077 921 0709`, `2026-11-16`, and `Hotel Grand Palace`.

## Deploying for free (GitHub Pages)

1. Push this folder to a GitHub repository (see below).
2. On GitHub: **Settings → Pages → Source** → select the `main` branch and
   `/ (root)` folder → **Save**.
3. GitHub will publish it at `https://<your-username>.github.io/<repo-name>/`
   within a minute or two.

## Pushing this folder to GitHub

This folder is already a git repository with an initial commit. To publish it:

```bash
# 1. Create a new, empty repository on https://github.com/new
#    (don't initialize it with a README/license — this folder already has one)

# 2. Point this local repo at it and push
git remote add origin https://github.com/<your-username>/<repo-name>.git
git branch -M main
git push -u origin main
```
