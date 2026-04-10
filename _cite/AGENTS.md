# _cite — Python Citation Pipeline

Pre-build step that generates `_data/citations.yaml` from `_data/sources.yaml` (and other data files). Run **before** `jekyll build`.

## PIPELINE

```
_data/sources.yaml  ──┐
_data/orcid.yaml    ──┤  cite.py  ──→  _data/citations.yaml
_data/google-scholar*.yaml ──┘         (DO NOT EDIT BY HAND)
```

1. `cite.py` loads plugins in order: `google-scholar` → `pubmed` → `orcid` → `sources`
2. Each plugin reads matching `_data/{plugin-name}*.yaml` files
3. Plugin `main(entry)` expands each entry into one or more sources
4. Sources with matching IDs are merged (later entries override)
5. Each source is cited via **Manubot** (subprocess call) to fetch title, authors, publisher, date, link
6. Results saved to `_data/citations.yaml` with `# DO NOT EDIT` header

## FILES

| File | Purpose |
|------|---------|
| `cite.py` | Main pipeline orchestrator |
| `util.py` | Helpers: logging, YAML I/O, date formatting, Manubot wrapper with disk cache |
| `requirements.txt` | Python deps: manubot, PyYAML, diskcache, rich, python-dotenv, google-search-results |
| `plugins/sources.py` | Pass-through (returns entry as-is) |
| `plugins/google-scholar.py` | Expands Google Scholar entries; uses `GOOGLE_SCHOLAR_API_KEY` env var |
| `plugins/orcid.py` | Fetches works from ORCID API |
| `plugins/pubmed.py` | Fetches works from PubMed API |
| `.cache/` | diskcache DB for Manubot results (90-day TTL) |

## COMMANDS

```bash
pip install -r _cite/requirements.txt
python _cite/cite.py
```

## CONVENTIONS

- Add publications to `_data/sources.yaml` with a DOI id (e.g., `id: doi:10.1234/...`). The pipeline auto-fetches metadata.
- Override auto-fetched fields by adding them directly in the source entry (e.g., custom `image`, `description`, `buttons`, `tags`).
- `GOOGLE_SCHOLAR_API_KEY` must be set as env var or in `.env` for Google Scholar plugin.
- Cache lives in `_cite/.cache/` — delete to force re-fetch. Cache expires after 90 days.

## ANTI-PATTERNS

- **DO NOT edit `_data/citations.yaml`** — it's overwritten on every run.
- **Run from repo root** — `cite.py` uses `Path.cwd()` to resolve `_data/` paths. Always invoke as `python _cite/cite.py` from repo root.
- **DO NOT add `_cite/.cache/` to `.gitignore`** — CI commits cache on failure to preserve fetched data.
