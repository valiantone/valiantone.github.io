# Frequently Bayesian

A personal blog featuring a variety of digital fundamentals — from career to code, data to design.

Built with [Zola](https://www.getzola.org/), a fast static site generator written in Rust.

**Live site:** [valiantone.github.io](https://valiantone.github.io)

## Features

- Light & dark mode with a custom "bifunctional" color theme (forest green + pink/magenta)
- Tag-based taxonomy
- Full-text search via Zola's built-in elasticlunr index
- RSS feed
- Sitemap generation
- Deployed to GitHub Pages via GitHub Actions

## Project Structure

```
/
├── config.toml            # Site configuration
├── content/
│   ├── _index.md          # Home page content
│   ├── about.md           # About page
│   ├── search.md          # Search page
│   └── blog/
│       ├── _index.md      # Blog section config
│       └── *.md           # Blog posts
├── sass/
│   └── style.scss         # All styles (design tokens, layout, components)
├── static/
│   ├── images/            # Post images
│   ├── assets/            # Logos & icons
│   ├── toggle-theme.js    # Theme toggle script
│   └── favicon.svg
└── templates/
    ├── base.html           # Base layout
    ├── index.html          # Home page
    ├── blog.html           # Post listing
    ├── blog-page.html      # Single post
    ├── about.html          # About page
    ├── search.html         # Search page
    ├── 404.html            # Not found page
    ├── macros.html         # Reusable Tera macros
    └── tags/
        ├── list.html       # All tags
        └── single.html     # Posts for a tag
```

## Local Development

Install Zola ([instructions](https://www.getzola.org/documentation/getting-started/installation/)), then:

```bash
# Start the dev server with live reload
zola serve

# Build the site (outputs to public/)
zola build
```

## Adding a New Post

Create a new Markdown file in `content/blog/` with TOML frontmatter:

```markdown
+++
title = "My New Post"
date = 2025-06-15
description = "A short summary."
[taxonomies]
tags = ["topic"]
[extra]
featured = false
+++

Post content goes here.
```

## Deployment

The site is automatically built and deployed to GitHub Pages on push to `main` via the workflow in `.github/workflows/deploy.yml` using [shalzz/zola-deploy-action](https://github.com/shalzz/zola-deploy-action).

## License

Licensed under the MIT License.
