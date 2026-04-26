# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Personal site for benrobison.com built on the **Scriptor** Jekyll theme (Jekyll ~> 4.0). Static site generated to `_site/`, deployable via Stackbit/Netlify (`stackbit.yaml` defines the headless-CMS schema).

## Commands

```bash
bundle install                 # install gem dependencies
bundle exec jekyll serve       # local dev server with live reload (http://localhost:4000)
bundle exec jekyll build       # build static site to _site/
```

Stackbit's production build command is `bundle install && jekyll build` (publishes `_site/`).

There is no test suite, linter, or JS build step — assets in `assets/css`, `assets/js`, `assets/fonts` are checked in directly. SCSS in `_sass/` is compiled by Jekyll (configured in `_config.yml` under `sass:` — output style is `compressed`).

## Architecture

Standard Jekyll layout — most structure is discoverable, but a few conventions matter:

- **Site config lives in two places.** `_config.yml` holds Jekyll settings (title, url, navigation, pagination, plugins). Author identity and social links are split into `_data/author.json` and `_data/social.json` — these are referenced by templates and exposed as editable models in `stackbit.yaml`.
- **Stackbit is the source of truth for content modeling.** `stackbit.yaml` declares the CMS schema for `config`, `author`, `social`, `post`, `page`, `tags`, and `notfound`. When adding new front-matter fields to posts/pages or new config options, update `stackbit.yaml` so the headless CMS sees them.
- **Layouts cascade:** `_layouts/default.html` is the shell; `page.html`, `post.html`, and `tags.html` extend it. Posts live in `_posts/` and use the `YYYY-MM-DD-title.md` naming convention. Permalink pattern is `/:title` (set in `_config.yml`).
- **Pagination** uses `jekyll-paginate` (5 posts per page, path `/page:num/`) — `index.html` is the paginated home.
- **Includes** in `_includes/` are small partials (header, footer, social, disqus, google_analytics, image helpers). `disqus` and `google_analytics` are gated on the corresponding `_config.yml` keys being non-empty.

## Adding content

- **New post:** create `_posts/YYYY-MM-DD-slug.md` with front matter `layout: post`, `title`, `date`, optional `description`, `feature_image`, `tags` (see existing posts in `_posts/` for the template).
- **New top-level page:** create `whatever.md` at the repo root with `layout: page` front matter, then add a `navigation` entry in `_config.yml` if it should appear in the header.
