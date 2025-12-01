# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

항상 한국어로 답변하세요.

## Project Overview

This is a personal blog (yongwookha.github.io) built with Jekyll using the Beautiful Jekyll theme. The blog features content in Korean covering Machine Learning, Study notes, Book reviews, and Life/Travel.

## Development Commands

```bash
# Install dependencies
bundle install

# Run local development server
bundle exec jekyll serve

# Build the site
bundle exec jekyll build
```

## Architecture

### Collections Structure

Blog posts are organized into collections (not the standard `_posts` folder):

- `collections/_MachineLearning/` - ML/AI technical posts
- `collections/_Study/` - Study notes and tutorials
- `collections/_Book/` - Book reviews
- `collections/_LifeAndTravel/` - Personal/travel posts
- `collections/Draft/` - Unpublished drafts

### Post Format

Posts use the naming convention: `YYYY-MM-DD-title.md`

Required YAML front matter:
```yaml
---
layout: post
title: Post Title
subtitle: Short description
tags: [TAG1, TAG2]
cover-img: /assets/img/image.jpg  # optional
comments: true
---
```

### Key Configuration

- `_config.yml` - Site settings, collection definitions, navigation, analytics (gtag), Disqus comments
- `_layouts/` - Page templates (post.html, page.html, home.html, minimal.html)
- `_includes/` - Reusable components (nav, footer, comments, analytics)
- `assets/` - Static files (images, CSS, JS)

### Comments and Analytics

- Disqus enabled with shortname "yongwooks-page"
- Google Analytics via gtag (G-NWH2HVPB7P)
- Staticman configured for static comments

## CI/CD

GitHub Actions workflow (`.github/workflows/ci.yml`) builds the site using Jekyll 3.8 in a Docker container on push/PR.
