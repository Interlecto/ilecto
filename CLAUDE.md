# CLAUDE.md

Interlecto hub page: a static, bilingual (ES/EN) page at `https://ilecto.app/` that links to each Interlecto project's own site. See `README.md` for deployment notes and the project table.

## Layout

- `public/` — the static site: `index.html`, `styles.css`, `_headers`, icons, and one image per linked project in `public/projects/`.
- `src/worker.js` — a small Cloudflare Worker that only redirects non-canonical hosts to `https://ilecto.app/`; the page itself is static.
- `wrangler.jsonc` — Worker config: name `ilecto-site`, assets from `./public`, custom domain `ilecto.app`, `workers_dev` and preview URLs off.

## Commands

No build step and no dependencies to install. Deploy with:

```
npx wrangler deploy
```

Deploying publishes to production, so only do it when asked.

## Conventions

- Plain HTML/CSS, system fonts only; the brand colour `#3399cc` is `--accent` in `styles.css`.
- Keep both languages in step when editing page text.
- When adding a project, add its image to `public/projects/` and its row to the table in `README.md`.
- This is a Worker, not Cloudflare Pages: do not add Pages config.
