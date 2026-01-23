# GenAI Tool Selection Guide

A practical guide for Mornington Peninsula Shire staff to select the right AI tool for their tasks.

## Features

- **Tool Selection Wizard**: Answer a few questions to get a tool recommendation
- **Prompt Builder**: Step-by-step CO-STAR framework prompt builder
- **Tool Profiles**: Detailed information on ChatGPT, Claude, Perplexity, Copilot Chat, and Gemini
- **Quick Reference**: Task-based tool recommendations
- **Comparison Table**: Side-by-side feature comparison
- **AI Glossary**: Plain-language explanations of AI terminology

## Deployment

This is a standalone HTML file that can be deployed to any static hosting service.

### Cloudflare Pages

1. Push this repo to GitHub
2. Log into Cloudflare Dashboard → Pages
3. Create a new project → Connect to Git
4. Select this repository
5. Build settings:
   - **Framework preset**: None
   - **Build command**: (leave empty)
   - **Build output directory**: `/` or `.`
6. Deploy

The site will be available at `your-project.pages.dev`

### Alternative: Direct Upload

You can also drag and drop the `index.html` file directly in Cloudflare Pages dashboard under "Upload assets".

## Local Development

Simply open `index.html` in a browser. The file loads React from CDN and uses Babel for JSX transformation.

For a better dev experience with hot reload:
```bash
npx serve .
```

## Tech Stack

- React 18 (loaded from CDN)
- Babel Standalone (for JSX transformation)
- No build step required

## Note

This is a prototype/demo version. For production use, consider:
- Building with Vite or similar for better performance
- Removing Babel standalone (adds ~500KB)
- Adding proper error boundaries
- Implementing analytics

## Contact

Questions? Contact Justin Daly - Emerging Technologies Lead, Mornington Peninsula Shire
