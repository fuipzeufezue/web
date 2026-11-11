# OSTHER

**Osmen's Library** — a literary reading room for ten short stories.

OSTHER is a React + Vite + TypeScript site that presents sixteen of Osmen's short
stories as a curated collection. Each story has its own cover, a short
introduction, and a dedicated reading page containing the full Arabic text
extracted from the original PDFs.

---

## Quick start

```bash
npm install
npm run dev
```

Then open the URL printed by Vite (default: <http://localhost:5173>).

> On Windows PowerShell, if `npm` is blocked by the execution policy, use
> `npm.cmd` instead (for example `npm.cmd run dev`).

### Production build

```bash
npm run build
npm run preview
```

The static build is written to `dist/`.

---

## What is included

- **Home page** with a literary hero, animated book illustration, and a grid of
  story cards (cover, title, description, reading time, and a **READ** action).
- **Reading pages** at `/story/:slug`, each rendering the complete story with
  preserved paragraph breaks, chapter/scene headings, and separators.
- **Reading controls**: font-size adjustment, light/dark reading mode,
  a live reading-progress indicator, and a back-to-top button.
- **Responsive layout** for desktop, laptop, tablet, and mobile.
- **Motion**: book-opening loader, route transitions, scroll reveals, floating
  decorations, card and cover hover effects, and staggered text entrances.
- **Original PDF access**: each reading page links to a copy of the source PDF.
- **Accessibility**: skip link, semantic landmarks, visible focus states,
  reduced-motion support, and Arabic RTL typography.

No animation library is used — motion is CSS plus `IntersectionObserver`.

---

## Story selection

The source folder contains **16 PDFs**, and the library currently features
**all sixteen**:

| # | File | Story | Slug |
|---|------|-------|------|
| 1 | `الرحلة الأخيرة.pdf` | الرحلة الأخيرة | `the-last-journey` |
| 2 | `المشروع اكس.pdf` | المشروع اكس | `project-x` |
| 3 | `تبرير كاذب.pdf` | تبرير كاذب | `false-alibi` |
| 4 | `تجربة سايلنت ايريا.pdf` | تجربة سايلنت ايريا | `silent-area` |
| 5 | `تجربة سباركل.pdf` | تجربة سباركل | `spark-experiment` |
| 6 | `جيفري ساغنامولي.pdf` | جيفري ساغنامولي | `jeffrey-sagnamoli` |
| 7 | `عودة البارون.pdf` | عودة البارون | `barons-return` |
| 8 | `قصة ارى الماضي بوضوح.pdf` | أرى الماضي بوضوح | `see-the-past` |
| 9 | `قصة الخامس والعشرون كاملة.pdf` | الخامس والعشرون | `twenty-fifth` |
| 10 | `قصة حررنا.pdf` | حررنا | `hurrna` |
| 11 | `قصة دائما خطوتين للأمام الجزء الأول.pdf` | دائما خطوتين لألمام | `always-two-steps-ahead` |
| 12 | `قصة دائما خطوتين للأمام الجزء الثاني.pdf` | دائما خطوتين لألمام | `always-two-steps-ahead-part-two` |
| 13 | `قضية لونيز.pdf` | قضية لونيز | `the-lunez-case` |
| 14 | `قطار شينفولد.pdf` | قطار شينفولد | `the-shinerfeld-express` |
| 15 | `مرض كاذب.pdf` | مرض كاذب | `the-false-disease` |
| 16 | `ملل قاتل.pdf` | ملل قاتل | `the-bored-killer` |


### How to change the selection

`scripts/stories.config.mjs` is the single source of truth for the library.

1. Open `scripts/stories.config.mjs`.
2. Edit the `stories` array — remove entries, reorder them, or add a new entry
   using the same shape (`slug`, `order`, `file`, `title`, `titleLines`,
   `englishTitle`, `description`, `tagline`, `genre`, `cover`, `palette`,
   `motif`).
3. Point `PDF_DIR` at the folder containing the PDFs if it changes.
4. Re-import the text and metadata:

   ```bash
   npm run import
   ```

   This reads the PDFs, extracts and lightly cleans the Arabic text, writes
   `src/data/content/*.txt` and `src/data/stories.json`, and copies the PDFs
   into `public/stories/` for the download links.

5. If you added or changed a story's palette or motif, regenerate the covers:

   ```bash
   npm run covers
   ```

   Or run both steps together:

   ```bash
   npm run assets
   ```

The grid, navigation, next-story links, and statistics are all derived from
`src/data/stories.json`, so no page code needs to change when the selection
changes.

---

## Where things live

```text
OSTHER/
├─ public/
│  ├─ covers/            generated SVG covers (one per story)
│  ├─ stories/           copies of the selected PDFs (originals untouched)
│  ├─ favicon.svg
│  └─ _redirects         SPA fallback for static hosts
├─ scripts/
│  ├─ stories.config.mjs selection + metadata (edit this)
│  ├─ import-stories.mjs PDF text extraction and metadata generation
│  └─ generate-covers.mjs literary SVG cover generator
├─ src/
│  ├─ components/        header, footer, story card, loader, reveal, icons, art
│  ├─ data/
│  │  ├─ library.ts      types, lookups, lazy content loading, parsing
│  │  ├─ stories.json    generated metadata
│  │  └─ content/        extracted Arabic text (one .txt per story)
│  ├─ pages/             HomePage, ReaderPage, NotFoundPage
│  ├─ styles/            global.css, home.css, reader.css
│  ├─ App.tsx            routes and scroll management
│  └─ main.tsx           entry point, fonts, styles
├─ index.html
├─ vite.config.ts
└─ package.json
```

---

## Source PDFs

The original PDFs in `C:\Users\HI\Desktop\Osmen\Projects\Short Stories` are
**never modified, moved, or renamed**. The importer only reads them and copies
the selected files into `public/stories/`.

Text is extracted with `pdfjs-dist`. A small, conservative replacement pass
fixes common Arabic PDF-font quirks (for example `اىل` → `إلى` and
`عىل` → `على`). No story text is invented or rewritten; the reading pages show
the extracted source text.

If a PDF changes, run `npm run import` again to refresh the text and metadata.

---

## Routing and deployment

Routes use `BrowserRouter`:

- `/` — library home
- `/story/:slug` — reading page
- any other path — the styled 404 page

`public/_redirects` contains the Netlify-style fallback:

```text
/*    /index.html   200
```

Use an equivalent SPA rewrite on other static hosts so refreshing a story URL
does not return a 404.

---

## Scripts

| Command | Purpose |
|---------|---------|
| `npm run dev` | Start the Vite dev server |
| `npm run build` | Type-check and build for production |
| `npm run preview` | Preview the production build |
| `npm run import` | Extract text/metadata and copy PDFs |
| `npm run covers` | Regenerate all SVG covers |
| `npm run assets` | Run `import` then `covers` |

---

## Notes

- Fonts are bundled locally with `@fontsource`: Cormorant Garamond (display),
  Amiri (Arabic reading), and Manrope (interface). No external font requests.
- Covers are self-contained SVGs with an embedded Amiri face, so they render
  consistently without network access.
- Reading preferences (font size and theme) are stored in `localStorage`.
