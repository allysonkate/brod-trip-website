# The Bro'd Trip

A static rebuild of brod-trip.com, a travel blog documenting a year of van life across all 50 states.

**Live site:** [https://allysonkate.com/brod-trip-website/index.html](https://allysonkate.com/brod-trip-website/index.html)

## Background

The original WordPress site went offline with no export and no backup. The only surviving copy was a partial crawl in the Wayback Machine. This repo is a from-scratch rebuild as a static site: 248 pages (244 blog posts plus the index, sponsors, in-the-news, and favorite-states pages), pulled from whatever the Wayback Machine had archived.

About 60% of the original images were recoverable. Posts missing a photo carry a placeholder noting the original filename and any caption that survived in the archive, rather than a broken image or a silent gap. Archived comments, where they existed, were pulled in along with the post content.

## Stack

Plain HTML, CSS, and JS. No build step, no framework, no dependencies. Hosted on GitHub Pages. Images are base64-encoded and embedded directly in each post's HTML rather than referenced as separate files, apart from a shared `images/` folder used for site-wide assets (logo, author photo, sponsor logos).

## Structure

Each blog post is its own flat HTML file at the repo root, named after its original WordPress slug (`big-sur-central-california.html`, `vlog-22-she-said-yes.html`, etc.). `index.html` lists and filters all posts. A handful of non-post pages (`sponsors.html`, `in-the-news.html`, `favorite-states.html`) live alongside them. `CLAUDE.md` documents the exact HTML template and naming conventions every post follows, for anyone (or any tool) picking the project back up.

## Notes

This is a personal archive project, not a template meant for reuse. Content and photos belong to the original blog.
