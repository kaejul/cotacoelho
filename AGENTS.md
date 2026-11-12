# Cota Coelho — menu site

Static website for Cota Coelho, a Portuguese petiscos restaurant in Almada. All user-facing text is in Portuguese (pt-PT).
Hosted as a static site (GitHub Pages, see `CNAME`). There is no build step and no dependencies.

## Files

- `index.html` / `menu.html`: the public menu page. **They are identical copies; keep them in sync.** The page fetches `menu.json` and renders it into `#menu-grid`.
- `menu.json`: the single source of truth for the menu (categories, items, prices).
- `edit.html`: menu editor (not linked from the site; excluded from indexing via `robots.txt` and `<meta name="robots" content="noindex">`). It loads `menu.json`, shows every field as an input (add/remove/reorder items and categories), and downloads a new `menu.json`. It cannot change the live site; the downloaded file has to replace `menu.json` in the repo and be committed.
- `server.js`: local dev server only (`node server.js` → http://localhost:5000, tries the next port if busy).
- `logo1.svg`, `logo2.svg`, `favicon.*`: assets used by the pages.
- `robots.txt`: blocks `edit.html` from crawlers.

Untracked files in the working tree (`*.orig`, `*_old.svg`, QR codes, PDFs/PNGs) are local scratch/print material, not part of the site.

## menu.json format

```json
{
  "categories": [
    {
      "id": "couvert",
      "title": "Couvert na Telha",
      "type": "couvert",
      "couvertItems": [{ "name": "Manteiga de Alho", "note": "caseira" }],
      "perPersonPrice": "5,5€",
      "extra": { "name": "...", "price": "...", "tags": [] }
    },
    {
      "id": "aguas",
      "title": "Águas",
      "items": [{ "name": "Água Luso", "note": "50cl", "sub": "...", "price": "2€", "tags": ["v"] }]
    }
  ]
}
```

- Category: `id` (unique slug), `title`, optional `type: "couvert"`. Couvert categories use `couvertItems` + `perPersonPrice` (+ optional `extra`); all others use `items`.
- Item: `name` (required), optional `note` (short inline, e.g. size), optional `sub` (second line, e.g. wine region · grapes), `price`, optional `tags`.
- `price` is a display string such as `"3,5€"` (comma decimals are the convention), or `null` to show `????€`.
- `tags` accepts `v` (vegetariano), `vg` (vegano), `gf` (sem glúten). The legend is currently hidden.
- Omit optional keys instead of using empty strings. Key order: `id, title, type, couvertItems|items, perPersonPrice, extra` and `name, note, sub, price, tags`. 2-space indentation, no trailing newline. `edit.html` writes exactly this format; keep it in sync if the format changes.
- Array order is display order.

## Conventions

- Commits are short Portuguese descriptions of the menu change (e.g. "migas com entrecosto").
- Keep pages self-contained: inline CSS/JS, Google Fonts only (Playfair Display, Cormorant Garamond, DM Mono), colors from the `:root` variables.
