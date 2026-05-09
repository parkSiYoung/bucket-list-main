# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the App

No build step required. Open `index.html` directly in a browser, or serve it locally:

```bash
python -m http.server 8000
# then visit http://localhost:8000
```

There are no npm packages, no build tools, no test framework, and no linter configured.

## Architecture

Vanilla JavaScript single-page app with two modules loaded via `<script>` tags in `index.html`:

- **`js/storage.js`** — Data layer. Exports `BucketStorage`, a plain object with methods for LocalStorage CRUD: `load`, `save`, `addItem`, `updateItem`, `deleteItem`, `toggleComplete`, `getStats`, `getFilteredList`. All data lives under the `bucketList` key as a JSON array.
- **`js/app.js`** — UI layer. `BucketListApp` class handles DOM caching, event binding, rendering, and modal control. On every state change, `render()` replaces the full list HTML using `createBucketItemHTML()`.
- **`css/styles.css`** — Custom animations and overrides layered on top of Tailwind CSS (loaded from CDN).

**Data model** stored in LocalStorage:
```js
{ id: "timestamp", title: "string", completed: boolean, createdAt: "ISO", completedAt: "ISO|null" }
```

**Security**: `escapeHtml()` in `app.js` encodes all user input before injecting into the DOM — preserve this pattern when adding any new HTML generation.

## Key Conventions

- UI text and code comments are in Korean.
- Items are stored newest-first (`unshift` in `addItem`).
- No async operations; all storage calls are synchronous.
- Tailwind utility classes handle layout/spacing; `styles.css` handles animations, filter button active states, and responsive overrides.
