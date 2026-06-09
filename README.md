# top25-site

Static hosting for the [top25](https://github.com/nylith-code/top25) browsing
site, served via GitHub Pages at **https://top25.nylith.com**.

## How it works

The site is built in the `top25` repo (`bun run build:site`, which writes to
`top25/dist/`). The built output is copied into **`docs/`** here and committed.
GitHub Pages is configured to **deploy from a branch**: `main` / `docs`.

Everything outside `docs/` (this README, `LICENSE`, …) is a real repo file and
is never served — only the contents of `docs/` are published.

## Deploying a new build

```sh
# in the top25 repo
bun run build:site

# then mirror dist/ into docs/ here (clean copy so stale hashed bundles drop out)
rsync -a --delete --exclude=CNAME --exclude=.nojekyll \
  ../top25/dist/ docs/

git add -A && git commit -m "Deploy site" && git push
```

`docs/` must always contain `CNAME` (the custom domain) and `.nojekyll` (serve
files as-is, no Jekyll). These are committed here, not produced by the build —
so the mirror excludes them, and `--delete` leaves them untouched while dropping
stale hashed bundles from previous builds.
