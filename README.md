# typescript-course

[![exercises](https://github.com/hngu/typescript-course/actions/workflows/exercises.yml/badge.svg?branch=main)](https://github.com/hngu/typescript-course/actions/workflows/exercises.yml)

An Astro TypeScript learning site ([`src/elearning`](src/elearning/README.md)) and the graded,
runnable solutions to every coding exercise in it ([`src/exercises`](src/exercises/README.md)),
deployed to Cloudflare.

**Live:** <https://typescript-course.zerozero-dev7.workers.dev>

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

Deployed to <https://typescript-course.zerozero-dev7.workers.dev> as a Cloudflare Worker with static
assets (Workers Builds, connected to this repo; every push to `main` deploys). There
is no Worker script: [`wrangler.jsonc`](wrangler.jsonc) points Wrangler at the built site.

| Setting | Value |
| --- | --- |
| Build command | `pnpm --filter elearning build` |
| Deploy command | `npx wrangler deploy` |
| Root directory | blank — the build reads `src/exercises` |
| `PNPM_VERSION` build variable | `11.20.0` |

Wrangler doesn't need to be a dependency: `npx` fetches it on the build machine. Keep
`wrangler.jsonc` committed — without it Wrangler runs its application detection, which fails at the
root of a pnpm workspace.

**Cloudflare does not read `packageManager`.** The pnpm version comes only from the `PNPM_VERSION`
build variable in the dashboard (Settings → Build → Variables and secrets); without it the build
falls back to the image's default pnpm, which may not match the lockfile. When you bump pnpm, change
it in both places: `package.json#packageManager` (CI, Corepack) and `PNPM_VERSION`, then redeploy.
Node needs nothing — the build image reads `.nvmrc`.
