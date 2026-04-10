# _includes — Liquid Partials

Parameterized Liquid components used by layouts and content pages. Most accept `include.*` params.

## KEY COMPONENTS

| Include | Params | Purpose |
|---------|--------|---------|
| `section.html` | `dark`, `background`, `size` | Section break — inserts HTML comment parsed by `content.html` regex. Splits page into styled `<section>` blocks |
| `content.html` | `content` | Wraps page content; splits on `<!-- section break -->`, extracts `<dark>`, `<background>`, `<size>` via `regex_scan` |
| `citation.html` | `lookup`, `style` | Renders citation from `_data/citations.yaml`. `style="rich"` shows image, description, buttons, tags |
| `list.html` | `data`, `component`, `filter`, `style` | Iterates `site.data[data]` or `site[data]`, groups by year, renders each item via `component` include |
| `card.html` | `title`, `subtitle`, `description`, `image`, `link`, `tags`, `repo`, `style`, `tooltip` | Generic card with image, text, tags |
| `feature.html` | `title`, `text`, `image`, `link`, `flip` | Side-by-side image+text block. `flip` reverses order |
| `figure.html` | `image`, `caption`, `link`, `width`, `height` | `<figure>` with optional caption and sizing |
| `portrait.html` | `lookup` (slug) or inline member fields | Team member portrait; resolves from `site.members` by slug |
| `button.html` | `type`, `text`, `link`, `icon`, `tooltip`, `style`, `flip` | Button; merges defaults from `_data/types.yaml[type]`. `style="bare"` for inline links |
| `icon.html` | `icon` | Font Awesome class → `<i>`, or `.svg` filename → inline SVG |
| `float.html` | `content`, `flip`, `clear` | Floating sidebar block (used in member layout) |
| `cols.html` | `col1`, `col2`, ... | Equal-width columns from arbitrary named params |
| `grid.html` | `content`, `style` | Grid wrapper |
| `alert.html` | `type`, `content` | Styled alert box; color/icon from `_data/types.yaml` (tip, info, warning, error, etc.) |
| `tags.html` | `tags`, `repo`, `link` | Tag pills with search links; auto-fetches GitHub repo topics if `repo` set |
| `search-box.html` | *(none)* | Search input; triggers `onSearchInput()` / `onSearchClear()` from `_scripts/search.js` |

## HOW SECTIONS WORK

1. Content page includes `{% include section.html dark=true %}` (or `background`, `size`)
2. `section.html` emits `<!-- section break -->` + hidden `<dark>`, `<background>`, `<size>` tags
3. `content.html` (called by `default.html` layout) splits rendered content on that comment
4. Each chunk becomes a `<section>` with `data-dark`, `data-size`, `style="--image:"` attributes

## HOW LIST + COMPONENT WORKS

`list.html` is a generic data iterator. Pass `data` (key into `site.data` or `site` collections) and `component` (include name without `.html`). Each item's fields are spread as include params.

Example: `{% include list.html data="citations" component="citation" style="rich" %}`

## CONVENTIONS

- All includes use `include.*` for parameters — never global variables.
- Images go through `relative_url | uri_escape` and `fallback.html` for broken-image handling.
- Text fields that may contain markdown use `| markdownify | remove: "<p>" | remove: "</p>"` for inline rendering.
- Buttons merge defaults from `_data/types.yaml` via `hash_default` custom filter, so `type="email"` auto-provides icon, tooltip, and link template.
