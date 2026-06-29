# TODO — Content & Assets Checklist

Items marked **[ASSET]** require a file to be placed in `public/`.
Items marked **[CODE]** require editing a value inside a component file.
Items marked **[DRIVE]** are handled via Google Drive, no code change needed.

---

## Participants — `src/components/Participants.astro`

Edit the `bride` and `participants` objects at the top of the file.

- [ ] **[ASSET]** Add bride's photo → e.g. `public/photos/panna-mloda.jpg`
  - Then set `bride.img` to `'/photos/panna-mloda.jpg'`
  - Portrait orientation works best (the frame has a 4:5 aspect ratio)

- [ ] **[CODE]** Set bride's display name if you want one shown under the frame
  - Currently the label is always "PANNA MŁODA" — no name is displayed
  - Add a `displayName` field to `bride` and render it if desired

- [ ] **[ASSET]** Add photos for all 5 participants → e.g. `public/photos/uczestniczka-1.jpg` … `uczestniczka-5.jpg`
  - Then update each `img` field in the `participants` array

- [ ] **[CODE]** Replace placeholder names (`'Uczestniczka 1'` … `'Uczestniczka 5'`) with real first names in the `participants` array

---

## Location — `src/components/Location.astro`

Edit the `photos` array at the top of the file.

- [ ] **[ASSET]** Add 4 photos of Split → e.g. `public/photos/split-1.jpg` … `split-4.jpg`
  - Landscape orientation (the grid uses a 3:2 aspect ratio)
  - Then update the `src` fields in the `photos` array

---

## Flights — `src/components/Flights.astro`

Edit the `flights` object at the top of the file.

- [ ] **[CODE]** Fill in outbound flight date — replace `'TBD – Wrzesień 2026'` with the real date, e.g. `'18 Września 2026'`
- [ ] **[CODE]** Fill in outbound departure time — replace `'--:--'` with real time, e.g. `'07:45'`
- [ ] **[CODE]** Fill in outbound arrival time — replace `'--:--'` with real time, e.g. `'09:30'`
- [ ] **[CODE]** Fill in return flight date — replace `'TBD – Wrzesień 2026'` with the real date, e.g. `'21 Września 2026'`
- [ ] **[CODE]** Fill in return departure time — replace `'--:--'`
- [ ] **[CODE]** Fill in return arrival time — replace `'--:--'`

---

## Plan — `src/components/Plan.astro`

Edit the `days` array at the top of the file. Each day has `img` (full-size, shown in modal), `thumb` (shown on card), and `description`.

- [ ] **[ASSET]** Add a photo representing Friday (Piątek) → e.g. `public/photos/plan-piatek.jpg`
  - Update `img` and `thumb` for the `piatek` entry
  - Card thumbnail: ~400×300. Modal image: ~800×600. Landscape works best.

- [ ] **[ASSET]** Add a photo representing Saturday (Sobota) → `public/photos/plan-sobota.jpg`

- [ ] **[ASSET]** Add a photo representing Sunday (Niedziela) → `public/photos/plan-niedziela.jpg`

- [ ] **[ASSET]** Add a photo representing Monday (Poniedziałek) → `public/photos/plan-poniedzialek.jpg`

- [ ] **[CODE]** Review and update the day descriptions — current text is placeholder copy written during development. Rewrite to reflect the actual plan for each day.

---

## Gallery — `src/components/Gallery.astro`

No code changes needed. This section is driven entirely by the Google Drive folder.

- [ ] **[DRIVE]** Confirm the Drive folder is shared as "Anyone with the link → Viewer"
- [ ] **[DRIVE]** Share the folder link with all participants so they can add photos during the trip
- [ ] **[DRIVE]** Photos added to the folder appear on the site automatically on next page load — no redeploy needed

---

## General / Meta

- [ ] **[CODE]** Update the page `<title>` in `src/layouts/Layout.astro` if you want something more specific than `"Wieczór Panieński 2026"`
- [ ] **[CODE]** Add a `<meta name="description">` tag in `Layout.astro` (good practice before going live)
- [ ] **[CODE]** Add a favicon — place a `favicon.ico` or `favicon.png` in `public/` and add `<link rel="icon" href="/favicon.ico" />` to `Layout.astro`

---

## Before going live (hosting)

- [ ] Restrict the Google API key to your production domain in Google Cloud Console (APIs & Services → Credentials → Edit key → Website restrictions)
- [ ] Run `npm run build` and check for any build errors
- [ ] Confirm all placeholder images are replaced before sharing the URL

---

## Summary — asset files to place in `public/`

| File | Used in | Notes |
|---|---|---|
| `photos/panna-mloda.jpg` | Participants | Portrait, min 320×400 |
| `photos/uczestniczka-1.jpg` … `uczestniczka-5.jpg` | Participants | Portrait, min 260×320 |
| `photos/split-1.jpg` … `split-4.jpg` | Location | Landscape, min 600×400 |
| `photos/plan-piatek.jpg` | Plan modal | Landscape, min 800×600 |
| `photos/plan-sobota.jpg` | Plan modal | Landscape, min 800×600 |
| `photos/plan-niedziela.jpg` | Plan modal | Landscape, min 800×600 |
| `photos/plan-poniedzialek.jpg` | Plan modal | Landscape, min 800×600 |
| `favicon.ico` | Layout | Optional but recommended |
