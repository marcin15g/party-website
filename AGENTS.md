# Party Website — Agent Context

Single-page scrollable bachelorette party website for a trip to Split, Croatia (18–21 September 2026). Built with Astro, plain CSS custom properties, and vanilla JS. Polish language throughout.

---

## Running the project

Node is installed via NVM at `C:\Users\marci\AppData\Local\nvm\v24.18.0\` — it is NOT on the system PATH by default. Always invoke node/npm with the full path or prefix commands with:

```powershell
$env:PATH = "C:\Users\marci\AppData\Local\nvm\v24.18.0;" + $env:PATH
```

```bash
npm run dev      # dev server at http://localhost:4321
npm run build    # production build → dist/
npm run preview  # preview production build
```

The preview server launch config lives in `.claude/launch.json` and uses the full npm.cmd path.

---

## Tech stack

| Layer | Technology |
|---|---|
| Framework | Astro 4.x (static, no SSR) |
| Styling | Plain CSS with custom properties — no Tailwind |
| Scroll animations | AOS (Animate On Scroll) — installed via npm, imported as ES module |
| Photo lightbox | GLightbox — installed via npm |
| Maps | Google Maps embed iframe (no API key needed) |
| Photo gallery | Google Drive API v3 (client-side, API key only — no OAuth) |
| Fonts | Google Fonts: Playfair Display (serif/headings) + Lato (sans/body) |

---

## Colour palette

Sampled from a floral oil painting used as the countdown background (`public/bg.png`).

```css
--color-darkest: #1A1412;   /* deep canvas shadow — used as dark section bg */
--color-dark:    #7D3030;   /* wine rose — primary accent */
--color-mid:     #C4887A;   /* dusty rose — secondary accent, borders */
--color-light:   #E8CCBC;   /* pale petal — frames, light backgrounds */
--color-cream:   #F4EAE0;   /* warm off-white — main body background */
```

Defined in `src/layouts/Layout.astro` under `<style is:global>`.

---

## Project structure

```
src/
  layouts/
    Layout.astro          ← HTML shell, Google Fonts, AOS init
  components/
    Countdown.astro       ← Section 1
    Participants.astro    ← Section 2
    Location.astro        ← Section 3
    Flights.astro         ← Section 4
    Plan.astro            ← Section 5
    Essentials.astro      ← Section 6
    Gallery.astro         ← Section 7
  pages/
    index.astro           ← Imports and mounts all components in order
public/
  bg.png                  ← Floral oil painting (countdown background)
.env                      ← Google API key + Drive folder ID (never committed)
.env.example              ← Template showing required var names
.gitignore                ← Excludes .env, dist/, node_modules/, .astro/
```

---

## Sections

### 1 · Countdown — `Countdown.astro`
Full-viewport hero. Background: `public/bg.png` (oil painting) with a dark gradient overlay. Live JS countdown timer targeting **11 September 2026 at 19:20 Warsaw time** (`2026-09-11T19:20:00+02:00`). Text: "ZACZYNAMY ZA:" above four units (dni / godzin / minut / sekund). Timer runs with `setInterval` every second. Target date is in `define:vars` as `TARGET_ISO` constant at the top of the frontmatter.

### 2 · Participants — `Participants.astro`
Cream background. Top row: bride in a single antique-style CSS frame (double-border via `::before`/`::after` pseudo-elements) with "PANNA MŁODA" label above. Below a ruled divider reading "POZOSTAŁE UCZESTNICZKI", five smaller participant frames in a flex-wrap row. All images are currently placeholders from `placehold.co` — replace the `bride` and `participants` arrays in the frontmatter with real image paths under `public/`.

### 3 · Location — `Location.astro`
Darkest background (`--color-darkest`). Two-column grid: left is a Google Maps `<iframe>` embed (Split, Croatia, `z=13`, no API key required) with a slight sepia filter; right is a 2×2 photo grid of Split images (currently placeholders). Replace the `photos` array in the frontmatter with real image paths. Map has `loading="lazy"` and the embed URL uses `maps.google.com/maps?q=Split,+Croatia&z=13&output=embed`.

### 4 · Flights — `Flights.astro`
Cream background. Two-column layout: left has two flight cards (outbound WAW→SPU and return SPU→WAW) with IATA codes, city names, departure/arrival times, and a date pill. All times/dates are currently `"--:--"` and `"TBD – Wrzesień 2026"` — fill in the `flights` object in the frontmatter when details are known. Right column is a hand-built September 2026 calendar grid (CSS grid, 7 columns Mon–Sun). September 1 2026 is a Tuesday (startOffset = 1). Days **18, 19, 20, 21** are highlighted (dark background). Controlled by the `highlightedDays` Set in the frontmatter.

### 5 · Plan — `Plan.astro`
Light background (`--color-light`). Four cards in a horizontal grid (2×2 on mobile), one per day:
- PIĄTEK — 18 Września
- SOBOTA — 19 Września
- NIEDZIELA — 20 Września
- PONIEDZIAŁEK — 21 Września

Each card shows a thumbnail image and day name. Clicking opens a **modal overlay** (fixed position, dark backdrop, fade-in transition). The modal shows the full-size image on the left and the day description on the right (2-column grid, stacks on mobile). Closes via ✕ button, clicking outside, or Escape key. Day data (image URLs, descriptions) live in the `days` array in the frontmatter — replace placeholders with real images.

**Key JS pattern:** data is passed from Astro frontmatter to client JS via `define:vars={{ days }}` (safe here because it's a plain array of strings, no ES module imports needed in this script).

### 6 · Essentials — `Essentials.astro`
Darkest background. Two-column layout: left is a categorised packing list (Dokumenty, Bagaż, Plaża & Słońce, Finanse & Elektronika, Na wszelki wypadek) with diamond bullet points; right is a hand-drawn SVG suitcase illustration (`viewBox="0 0 320 380"`) drawn using the site colour palette — wine-red body, dusty-rose accents, cream label and sticker. The SVG has a "Split 2026" circular sticker and "bon voyage" italic text. Edit the `items` array in the frontmatter to change the list.

### 7 · Gallery — `Gallery.astro`
Cream background. Fetches photos from a **public Google Drive folder** using the Drive API v3 at runtime in the browser (client-side JS, no backend). Displays them in a CSS `columns` masonry grid. Clicking a photo opens a **GLightbox** lightbox.

**Important Astro pattern used here:** `define:vars` cannot be combined with ES module `import` statements (breaks with "Cannot use import statement outside a module"). The workaround: API key and folder ID are rendered into `data-api-key` / `data-folder-id` attributes on a hidden `<div id="gallery-config">`, then read from the DOM inside a regular `<script>` (which Astro bundles as a proper ES module). Each lightbox anchor must have `data-type="image"` — without it, GLightbox treats the Google Drive thumbnail URL as a webpage (no file extension to infer from).

**Environment variables** (in `.env`, never committed):
```
PUBLIC_GOOGLE_API_KEY=AIza...
PUBLIC_DRIVE_FOLDER_ID=1BxiMV...
```
Astro exposes `PUBLIC_*` vars to client-side scripts automatically. The folder must be shared as "Anyone with the link can view" in Google Drive for the API key (no OAuth) to work.

The section also renders an "Otwórz folder Google Drive" button linking directly to the folder, so participants can add photos from their phones.

---

## AOS (scroll animations)

AOS is imported as an npm package in `Layout.astro`:
```js
import AOS from 'aos';
import 'aos/dist/aos.css';
AOS.init({ duration: 800, easing: 'ease-in-out', once: true, offset: 80 });
```
**Do not use the CDN version** — it was tried first and caused all `data-aos` elements to stay invisible because the CSS loaded but the JS failed to initialise before first render. The npm package guarantees both load together.

Components use `data-aos="fade-up"` / `data-aos="fade-right"` / `data-aos="fade-left"` with optional `data-aos-delay` (in ms) for staggered entrance.

---

## What still needs real content

- `Participants.astro` — real photos for bride and 5 participants
- `Location.astro` — 4 real Split photos
- `Plan.astro` — real day photos and descriptions for each of the 4 days
- `Flights.astro` — actual flight times and dates once booked
- `Gallery.astro` — Drive folder is live; photos appear automatically as they're added
