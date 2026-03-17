# ps-template

A Claude Code skill that generates a stunning personal website from your resume PDF — deployed to GitHub Pages in minutes.

## Quick Start

```bash
git clone https://github.com/walras-group/ps-template
cd ps-template
```

1. Drop your resume PDF into the repo root
2. Enable GitHub Pages: **Settings → Pages → Source: GitHub Actions**
3. Open Claude Code and run:
   ```
   /resume-website
   ```
4. Follow the prompts — Claude reads your PDF, asks design preferences, and generates `index.html`
5. Push to deploy:
   ```bash
   git add index.html
   git commit -m "Add personal site"
   git push
   ```

Your site goes live at `https://<your-username>.github.io/<repo-name>/`

## How it works

- Claude reads your resume PDF and extracts all content
- Asks a few design questions (vibe, colors, animations)
- Generates a polished, production-grade `index.html` — pure HTML/CSS/JS, no build tools
- GitHub Actions auto-deploys on every push to `main`

## Stack

- Pure static HTML + CSS + JS
- Google Fonts via CDN
- GitHub Actions for CI/CD
- GitHub Pages for hosting
