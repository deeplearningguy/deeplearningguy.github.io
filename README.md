# deeplearningguy.github.io

Personal blog, built with [Astro](https://astro.build) and deployed to GitHub Pages.

## Writing a post

Add a Markdown (or MDX) file to `src/content/blog/`. The filename becomes the URL slug.

```md
---
title: 'Post title'
description: 'One-line summary, used for SEO and RSS.'
pubDate: 'Oct 06 2026'
# updatedDate: 'Oct 10 2026'      (optional)
# heroImage: '../../assets/x.jpg' (optional)
---

Body goes here. Math: $inline$ and $$display$$. Code fences are syntax highlighted.
```

## Commands

| Command           | Action                                   |
| :---------------- | :--------------------------------------- |
| `npm run dev`     | Start the dev server at `localhost:4321` |
| `npm run build`   | Build the site to `./dist/`              |
| `npm run preview` | Preview the production build locally     |

## Deploying

Pushing to `main` runs `.github/workflows/deploy.yml`, which builds and publishes the site. In the
repo settings, Pages > Source must be set to "GitHub Actions".

Site title, description, and GitHub link live in `src/consts.ts`.
