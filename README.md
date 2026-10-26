# felipeg17.github.io

Personal site of Felipe Gonzalez: https://felipeg17.github.io

## Stack

- Jekyll (GitHub Pages build), kramdown + rouge
- Plugins: jekyll-feed, jekyll-seo-tag, jekyll-sitemap, jekyll-paginate
- Plain CSS in `assets/css/style.css`, no JS framework
- Project lists live in `_data/projects.yml`; blog posts in `_posts/`

## Run locally

```bash
bundle install
bundle exec jekyll serve
```

Site is served at http://localhost:4000.

## Deploy

Push to `master`. GitHub Pages builds and publishes automatically.
