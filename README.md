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
├── assets/
│   ├── hero-video.mp4             # the hero video
│   ├── venue.jpg                  # photo in the "Venue" section
│   ├── page-background.avif       # page backdrop photo
│   ├── page-background.jpg        # same image, fallback for browsers without AVIF
│   ├── couple-animation.lottie    # UNUSED, replaced by the hero video
│   ├── backdrop-animation.lottie  # UNUSED, replaced by the hero video
│   ├── animations.js              # UNUSED, inlined copies of the two above
│   └── couple-illustration.webp   # UNUSED, left over from the old static hero
└── README.md
```

### The hero video

`assets/hero-video.mp4` is 1280x720, ten seconds, 2.5 MB. It replaced a pair of
dotLottie animations, so the Lottie player and its CDN script are gone.

It carries `muted` and `playsinline` as well as `autoplay`. All three are
required: phones refuse to autoplay a video that is not muted, and iOS will
otherwise take it fullscreen instead of playing it inline. The file has an audio
track, which is silenced by `muted`.

The frame spans the full page width. The negative margins on its wrapper cancel
the page gutters exactly, so it reaches the screen edges without going past them.

The frame is deliberately 3:2, taller than the video's own 16:9. `object-fit:
cover` then scales the footage to fill that height, which enlarges the couple and
crops only the left and right edges, about 8% from each side and nothing from top
or bottom. `object-position` stays at the default centre, so the couple stays
centred. Change the `aspect-ratio` on `.hero-video-frame` to adjust how much is
cropped: closer to 16/9 crops less, closer to 1/1 crops more.

The edges are feathered so the video dissolves into the page rather than sitting
in a hard rectangle. That is done with two separate masks, one fading left and
right on the frame and one fading top and bottom on the video. Nested masks
multiply, so this softens all four sides using only single-gradient masks.
Doing it in a single declaration would need `mask-composite`, whose browser
support is far patchier. The vertical percentages are larger than the horizontal
ones because the frame is much wider than it is tall; that keeps the feather
roughly even in actual pixels.

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

Its height is pinned with `100lvh`, not stretched between `top` and `bottom`.
On a phone the address bar collapses as you scroll, which changes the viewport
height; a layer that tracks it gets re-scaled by `background-size: cover`, and
that reads as the background drifting while you scroll. `100lvh` ignores the
address bar. `initBackdropLock()` does the same job in pixels for browsers
without that unit, and holds the height when the bar or the keyboard changes the
viewport, re-measuring only on a real rotation.

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
