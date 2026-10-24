# Interlecto

Static, bilingual (ES/EN) hub page for Interlecto, linking to its projects'
own sites.

## Deployment

Served by a Cloudflare **Worker** (not Pages — there is no Pages project
for this), named `ilecto-site`, bound to the `ilecto.app` custom domain. The
page itself is static; the worker (`src/worker.js`) only exists to redirect
any non-canonical host (e.g. a `*.workers.dev` preview URL) to
`https://ilecto.app/`.

```bash
npx wrangler deploy
```

This mirrors the setup used for `novenario.ilecto.app`
(`site/` in [Novenario](https://github.com/chlewey/Novenario)): one small
worker per `*.ilecto.app` site, each owning only its own custom-domain
route. `ilecto.app`'s route used to be claimed by `novenario-site` as a
stopgap (it 301-redirected to `novenario.ilecto.app` before this page
existed) — that route was moved to `ilecto-site` once this page shipped.

## Projects linked from this page

| Project    | Domain                        | Repository                                             |
| ---------- | ------------------------------ | ------------------------------------------------------- |
| Novenario  | `https://novenario.ilecto.app/` | [Novenario](https://github.com/chlewey/Novenario) (Android app) |
| ProHo      | `https://proho.ilecto.app/`     | [proho](https://github.com/Interlecto/proho)             |

This page evolves independently of those projects: entries are added or
removed here as projects are launched or retired, without touching their
own repositories. Each project is responsible for keeping its own
`*.ilecto.app` subdomain correct and for linking back to `ilecto.app`.

## Development

Branches follow Interlecto's usual workflow: work happens on
`develop/<topic>` branches cut from `master`, which merge straight back into
`master`.
