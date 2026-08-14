# Sithumi & Iminda — Wedding Invitation

A single-page digital wedding invitation. Static HTML/CSS/JS — no build step, no server, no dependencies to install.

## View it locally

Just open `index.html` in a browser (double-click it, or drag it into a browser window).

## What's inside

```
.
├── index.html                  # the entire site (one page)
├── assets/
│   └── couple-illustration.webp
└── README.md
```

## Features

- Touch-to-unlock cover with an animated WebGL shader background
- Live countdown to the wedding date
- "Add to Calendar" (.ics download)
- "Get Directions" (opens the venue in Google Maps)
- RSVP form that sends the reply straight to WhatsApp, pre-filled
- Tap-to-call and copy-phone-number for direct contact
- Fully responsive, works on both mobile and desktop

## Before you share this with guests

- **Replace the couple illustration.** `assets/couple-illustration.webp` is a
  low-resolution *watermarked stock preview* (filename pattern `-260nw-...`
  is a Shutterstock preview naming convention) — it was a placeholder, not a
  licensed image. Swap it for your own photo/illustration, or a purchased
  license, before publishing.
- **Replace the venue photo.** The image in the "Venue" section is still the
  AI-generated placeholder from the original design export. Swap in a real
  photo of Aradhana Hotel, Hikkaduwa if you have one.
- Double-check the phone number, date, and address in `index.html` are
  correct — search for `076 256 7546`, `2026-11-09`, and `Aradhana Hotel`.

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
