# Interlecto

Static, bilingual (ES/EN) hub page for Interlecto, linking to its projects'
own sites. No build step: deploy `index.html` and `styles.css` as-is.

Deployment target: **https://ilecto.app/** (Cloudflare Pages), with this
repository as the project root.

## Projects linked from this page

| Project    | Domain                        | Repository                                             |
| ---------- | ------------------------------ | ------------------------------------------------------- |
| Novenario  | `https://novenario.ilecto.app/` | [Novenario](https://github.com/Interlecto/novenario) (Android app; this subdomain not deployed yet) |
| ProHo      | `https://proho.ilecto.app/`     | [proho](https://github.com/Interlecto/proho)             |

This page evolves independently of those projects: entries are added or
removed here as projects are launched or retired, without touching their
own repositories. Each project is responsible for keeping its own
`*.ilecto.app` subdomain correct and for linking back to `ilecto.app`.

## Development

Branches follow Interlecto's usual workflow: work happens on
`develop/<topic>` branches cut from `master`, which merge straight back into
`master`.
