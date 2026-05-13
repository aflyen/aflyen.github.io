# Agent instructions for `aflyen.github.io`

## Build and preview commands

This repository is a Hugo site using the in-repo `aflyen` theme.

| Task | Command | Notes |
| --- | --- | --- |
| Start local preview server | `hugo server` | Default local workflow from `README.md`. |
| Build production output locally | `hugo --minify` | Generates the site into `public/`. |
| Match the GitHub Pages build more closely | `HUGO_ENVIRONMENT=production HUGO_ENV=production hugo --minify` | CI also injects `--baseURL` in `.github/workflows/hugo.yml`. |

The GitHub Pages workflow uses Hugo **0.123.7**. There is no repository-specific automated test suite or lint command in this repo today.

## High-level architecture

- `config.yaml` is the site entry point for base URL, theme selection, menu entries, site description, and palette.
- Content sources live under `content/`:
  - `content/posts/` contains dated blog posts.
  - `content/pages/` contains standalone pages such as About.
- The custom theme lives in `themes/aflyen/`:
  - `layouts/_default/baseof.html` provides the shared shell.
  - `layouts/partials/{head,header,footer}.html` provide global head and navigation/footer markup.
  - `layouts/index.html` renders the homepage by featuring the newest post and listing the next 12 most recent posts.
  - `layouts/_default/{list,single,terms,taxonomy}.html` render section pages, individual articles, and taxonomy pages.
- Styling is split between `themes/aflyen/static/css/theme.css` and `static/css/styles.css`, both linked from `themes/aflyen/layouts/partials/head.html`.
- Static assets live under `static/`, especially `static/assets/`. Hugo builds generated output into `public/`; treat `public/` as build output rather than source.

## Key repository conventions

- New written content should be Markdown with YAML front matter. Prefer the project skill in `.github/skills/create-hugo-article/` when creating posts or pages.
- New filenames follow `YYYY-MM-DD-slug.md` in lowercase with hyphens. Older WordPress-imported posts still exist as `.html`; preserve that format unless intentionally migrating a post.
- New posts typically use front matter fields such as `title`, `description`, `date`, `draft: false`, optional `categories`, `tags`, `preview`, and sometimes `author.display_name`.
- Standalone pages typically use `title`, `description`, `url`, and optional `preview`.
- Use absolute asset paths like `/assets/2024/...` in content. Post images usually live in `static/assets/YYYY/MM/`; page-specific assets may live in `static/assets/pages/`.
- Homepage and list teasers come from `.Description` or `.Summary`, but only when the value is plain text. Descriptions that start with `/` or `http` are skipped as teaser text.
- The shared article meta partial hides dates for content in the `pages` section. If a page should behave like a dated article, it belongs in `content/posts/`.
- Main menu items and the active color palette are configured in `config.yaml`, not hardcoded in the theme templates.
