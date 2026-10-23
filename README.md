# typescript-course

[![exercises](https://github.com/hngu/typescript-course/actions/workflows/exercises.yml/badge.svg?branch=main)](https://github.com/hngu/typescript-course/actions/workflows/exercises.yml)

An Astro TypeScript learning site ([`src/elearning`](src/elearning/README.md)) and the graded,
runnable solutions to every coding exercise in it ([`src/exercises`](src/exercises/README.md)),
deployed to Cloudflare Pages.

## Setup

Node is pinned in `.nvmrc` and pnpm in `package.json#packageManager`. Do not add a `.tool-versions`
file — Cloudflare Pages treats it as an asdf manifest and the deploy fails. With asdf, add
`legacy_version_file = yes` to `~/.asdfrc` so asdf-nodejs reads `.nvmrc`.

```sh
pnpm install
pnpm --filter elearning dev
pnpm --filter exercises verify
pnpm --filter elearning verify
```

## Deploying

Cloudflare Pages builds `main` (and previews PRs) through the Pages Git integration:

| Setting | Value |
| --- | --- |
| Build command | `pnpm --filter elearning build` |
| Build output directory | `src/elearning/dist` |
| Root directory | blank — the build reads `src/exercises` |
| `PNPM_VERSION` env var | `11.20.0`, on **both** Production and Preview |

**Pages does not read `packageManager`.** The pnpm version comes only from the `PNPM_VERSION`
variable in the dashboard (Settings → Variables and Secrets); without it Pages falls back to its
default pnpm, which may not match the lockfile. When you bump pnpm, change it in both places:
`package.json#packageManager` (CI, Corepack) and `PNPM_VERSION` on Pages, then redeploy. Node needs
nothing — Pages reads `.nvmrc`.
