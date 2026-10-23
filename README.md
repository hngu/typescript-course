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
