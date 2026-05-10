# Our Family Recipe Book

A warm, vintage-feeling digital recipe book that lives in a single HTML file. Built for kitchens, not data centers — open it from your desktop, your phone, or pin it to a tablet on the counter. Every feature works without an internet connection once it's loaded.

It's designed to grow with a family: multiple people share the device, each with their own personal space; recipes carry stories, photos, and audio memories; and the whole thing prints beautifully when you want a paper keepsake.

---

## Quick start

1. Open `index.html` in a modern browser (Chrome, Edge, Safari, Firefox).
2. The first time, you'll see a "Who's cooking today?" screen. Create your profile (name + optional avatar).
3. Three sample family recipes appear so the book isn't empty. Edit, delete, or add your own.
4. Everything saves automatically to your browser's local storage.

That's it. There's no install, no account, no backend.

---

## What's inside

### The book itself
- **Grid view** of recipe cards with photos, tags, prep time, and times-cooked count.
- **Book view** — a two-page spread with a center spine, a red ribbon bookmark, and a 3D page-flip animation between recipes.
- **Search** across titles, ingredients, instructions, tags, and notes.
- **Filter pills** for categories, favorites, tags, and contributors.
- **Sort** by recently added, alphabetical, most cooked, or fastest prep.
- **Four themes**: Parchment (default), Midnight Library, Summer Kitchen, Cozy Autumn.

### Recipe details
- Title, category, prep time, servings, "from the kitchen of," tags, and notes.
- **Multiple photos** per recipe (gallery), with the first as the cover.
- **Per-step instructions**, with an optional photo on each step.
- **Audio memory** — record up to 30 seconds (a story, a tip).
- **Wine / drink pairing** field.
- **Occasion** field ("Mom's birthday — May 14," "Thanksgiving").
- **Custom fields** — add your own labels and values.
- **Servings scaler** — +/− buttons rescale ingredient amounts and reformat fractions (½, ¼, etc.).
- **Unit toggle** — switch between imperial and metric on the fly (cups↔ml, oz↔g, °F↔°C).
- **Substitutions** — small built-in database surfaces a "↔ sub" button next to common ingredients (buttermilk, eggs, butter, etc.).
- **Comments & memories** — leave timestamped notes on a recipe.
- **Edit history** — last 10 versions kept, with a "↺ Restore" button.
- **Related recipes** — finds similar dishes by ingredient overlap.

### Cook mode
A focused, dark, full-screen view for actually cooking:
- Big step-by-step instructions, one at a time.
- Ingredient checklist on the side that **highlights ingredients used in the current step**.
- Step photos shown full-size.
- **Multiple named timers** running side by side, with a chime when each finishes.
- **Voice control** — toggle on, then say "next," "previous," "exit," or "start a 10 minute timer."
- **Wake Lock** keeps the screen on so you don't have to wipe your hands.
- **Confetti** when you finish.

### Planning & discovery
- **🎲 Surprise me** — picks a random recipe.
- **🤔 What can I make?** — type ingredients on hand; ranks recipes by match count.
- **📅 Meal Planner** — Mon–Sun × Breakfast/Lunch/Dinner grid. Click any slot to assign a recipe. Generate a shopping list for the week or print the plan.
- **🛒 Shopping List** — pick recipes and merge their ingredients into one printable, copyable list.
- **📔 Cooking Journal** — month-by-month calendar showing days you cooked. Recent entries listed below.
- **📊 Statistics** — total recipes, total cooks, favorites, breakdowns by category and contributor.
- **On this day** banner surfaces what you cooked on this date in past years.

### Family
- **👥 Contributors** — auto-generated from each recipe's "from" field. Click any contributor to see their photo, bio, and all their recipes.
- **Multiple local profiles** — each person on the device gets their own personal space (recipes, journal, contributors, comments, settings, versions).

### Sharing & importing
- **📥 Import recipe…** — four ways to bring a recipe in:
  - **🔗 URL** — paste a link to a recipe site; parses the JSON-LD schema most cooking sites embed.
  - **📋 Paste HTML** — fallback when the site blocks cross-origin fetching.
  - **📷 Photo (OCR)** — photograph a handwritten card or cookbook page; client-side OCR via Tesseract.js (loaded on demand).
  - **✨ Messy notes** — paste freeform text and a heuristic parser splits it into ingredients and steps.
- **↗ Share** per recipe — generates a URL with the recipe encoded, plus a QR code. Opening the URL on another device prompts to add it.
- **☑️ Bulk select** — multi-select recipes for batch favorite, tag, export, or delete.
- **💾 Backup / Restore** — download or restore the whole book as a JSON file.
- **☁️ Sync to file** — point the book at a JSON file in any cloud-synced folder (Dropbox, Drive, iCloud) using the File System Access API. Subsequent saves auto-write there. *Chrome/Edge only.*

### Print
- **🖨 Print card** — single recipe formatted as a clean two-column index card.
- **📖 Print whole book** — cover page + table of contents + one recipe per page. Use your browser's "Save as PDF" for a digital archive.
- **Print weekly plan** — full meal-plan grid with recipe titles.
- **Print shopping list** — checkbox list grouped by recipe.

### Polish
- ⌨️ **Keyboard shortcuts** for everything important (press `?` to see them).
- **Undo delete** — toast with an "Undo" button for ~5 seconds.
- **Custom book cover, title, and tagline** in Settings.
- **Custom categories** beyond the built-in dropdown.
- **PWA support** — `manifest.json` and `sw.js` make the book installable as an app and **work offline** once visited (when served via http; not as `file://`).

---

## File structure

```
Cooking_book/
├── index.html        ← The whole app (single-file webpage, ~5800 lines)
├── manifest.json     ← PWA manifest (app name, icons, colors)
├── sw.js             ← Service worker (caches the app shell for offline)
└── README.md         ← This file
```

That's the whole project. No build step, no dependencies to install.

---

## Profiles & data

Every profile gets its own isolated namespace inside the browser's localStorage. The keys look like:

```
family_recipe_book_v4__p_<profileId>
family_recipe_book_journal__p_<profileId>
family_recipe_book_contributors__p_<profileId>
family_recipe_book_plan__p_<profileId>
family_recipe_book_settings__p_<profileId>
family_recipe_book_versions__p_<profileId>
```

The profiles index itself lives at `family_recipe_book_profiles`. Existing books from before profiles existed are auto-migrated to a default "Me" profile on first load.

**To switch profiles:** click the avatar chip in the top-right and choose "Switch profile" or "Sign out."

**To remove a profile:** on the login screen, hover its tile and click the × that appears (with confirmation).

### Storage limits

The browser caps localStorage at roughly **5 MB per origin**. That's plenty for hundreds of recipes with notes, but photos and audio add up. If you hit the cap you'll see a toast saying "Storage is full." Mitigations: remove some photos, drop audio memos, or use **☁️ Sync to file** to offload backups to a real file.

---

## Keyboard shortcuts

| Key | Action |
|-----|--------|
| `/` | Focus search |
| `N` | New recipe |
| `G` | Grid view |
| `B` | Book view |
| `←` / `→` | Flip pages (book view) |
| `R` | Surprise me |
| `T` | Cycle theme |
| `?` | Show shortcuts |
| `Esc` | Close modal |

---

## Browser compatibility

| Feature | Works in |
|---------|----------|
| Core book (everything default) | Chrome, Edge, Safari, Firefox |
| Audio recording | Chrome, Edge, Safari, Firefox (with mic permission) |
| Voice control in cook mode | Chrome, Edge (uses `webkitSpeechRecognition`) |
| Cloud sync to file | Chrome, Edge (uses File System Access API) |
| Wake Lock (screen-on in cook mode) | Chrome, Edge, Safari (recent versions) |
| PWA install / offline | Any modern browser, **when served via http(s)** |
| OCR (photo → text) | Chrome, Edge, Safari, Firefox (loads Tesseract.js on demand) |

Opening the file directly via `file://` works for everything except PWA install and service worker caching. To get those, serve the folder over a tiny local web server, e.g.:

```bash
# Python 3
python3 -m http.server 8000

# Node
npx serve .
```

Then open `http://localhost:8000` and "Add to Home Screen" / "Install" from your browser's menu.

---

## Backup recommendations

The recipe book lives in your browser's localStorage. That's reliable in normal use, but it can be cleared by browser cleanup, "Clear Site Data," or a fresh OS install. Pick one of these:

- **Manual:** menu → 💾 Download backup. Save the JSON somewhere safe occasionally.
- **Automatic:** menu → ☁️ Sync to file → pick a file inside Dropbox/Drive/iCloud Drive. Every save writes there automatically (Chrome/Edge only).
- **Bulk export:** ☑️ Select multiple → ↓ Export to share a subset.

---

## Customization

Everything visible in the app is in `index.html`. The CSS section near the top defines four themes as `data-theme` attribute selectors:

```css
[data-theme="parchment"] { --paper: #f5ecd7; --accent: #8a3324; ... }
[data-theme="midnight"] { ... }
[data-theme="summer"] { ... }
[data-theme="autumn"] { ... }
```

To add a fifth theme, copy one of those blocks, change the variables, and add a new `<span class="theme-swatch" data-theme="yourname">` inside `#themeSwatches`.

The header title, tagline, and cover image are all editable through the UI — menu → ⚙️ Book settings.

---

## Honest limitations

- **No real cloud accounts.** Profiles are local to one device. Opening the file on a different computer starts fresh. Use Backup or Cloud Sync if you want to move a book between machines.
- **No multi-user collaboration.** Comments and edits are single-user inside each profile. There's no live sync between people.
- **localStorage cap.** Roughly 5 MB. Photos and audio chew through that fastest.
- **OCR isn't magic.** Tesseract works well on clean printed text, less well on cursive or poor lighting.
- **Voice control needs Chrome/Edge.** No fallback in Safari/Firefox for the Web Speech API.
- **URL recipe import fails on many sites** due to CORS. The "Paste HTML" tab is the reliable fallback.

If you want a hosted version with real accounts, cross-device sync, family sharing, and a server-side backup, that's a different architecture (a small backend with Supabase/Firebase or your own server). The current app is intentionally a single file you can email to your mother.

---

## Tech stack

- Plain **HTML / CSS / JavaScript**, no framework, no bundler.
- **Google Fonts** (Cormorant Garamond, Playfair Display, Caveat) loaded via `<link>`.
- **qrcode.js** loaded from cdnjs for share QR codes.
- **Tesseract.js** loaded on demand from cdnjs only when you open the OCR tab.
- **Web Audio API** for timer chimes.
- **MediaRecorder API** for audio memos.
- **Web Speech API** (`webkitSpeechRecognition`) for voice control.
- **Wake Lock API** to keep the screen on in cook mode.
- **File System Access API** for cloud sync.
- **localStorage** for persistence (profiles, recipes, journal, etc.).
- **Service worker + manifest** for PWA install + offline.

---

## Credits

Created by **[Arcadi Llanza](https://alc1218.github.io/PersonalWebpage/)**, with care, to feel like a recipe book your grandmother might have kept.

---

*Made with love in our kitchen. ✦*
