# website

Source for [unboundforce.dev](https://unboundforce.dev), the docs, hero
profiles, roadmap and blog of [Unbound Force](https://github.com/unbound-force).

Built with [Hugo](https://gohugo.io) and the
[Doks](https://getdoks.org) theme, and deployed to GitHub Pages on every
push to `main`.

## Run it locally

You need:

- Node.js 20.11 or later, as set by `engines` in [`package.json`](package.json).
- Go 1.23 or later, as set in [`go.mod`](go.mod). Hugo uses it to resolve
  its modules.
- The Hugo extended binary. npm doesn't install it, so install it yourself
  with `brew install hugo` or from
  [gohugo.io](https://gohugo.io/installation/). CI uses the version pinned
  in [`ci.yml`](.github/workflows/ci.yml).

```bash
npm install
npm run dev      # live-reloading server at http://localhost:1313/
```

To check a production build:

```bash
npm run build    # output goes to public/
npm run preview
```

## Where the site's source lives

| Path                  | What's there                              |
| --------------------- | ----------------------------------------- |
| `content/docs/`       | Documentation pages, by section           |
| `content/blog/`       | Blog posts                                |
| `config/_default/`    | Hugo settings, theme parameters and menus |
| `assets/scss/common/` | Theme colors, fonts and custom CSS        |
| `layouts/`            | Template overrides                        |
| `static/`             | Images and other files served as is       |

## Contributing

Open a pull request against `main`. CI builds the site on every pull
request, so a broken build shows up before review. Before you open one,
run `npm run build` and look over the pages you changed with
`npm run dev`, in both light and dark mode.

[`AGENTS.md`](AGENTS.md) has the full project conventions, and
[`CONTRIBUTING`](https://github.com/unbound-force/.github/blob/main/.github/CONTRIBUTING.md)
covers how Unbound Force works in general.

## License

[Apache 2.0](LICENSE)
