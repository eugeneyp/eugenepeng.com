# Eugene Peng's Personal Website - Astro Project Context

## Project Architecture
- **Framework:** Astro (Static Site Generator)
- **Theme:** Astro Pure (`cworld1/astro-theme-pure`)
- **Hosting:** GitHub Pages via GitHub Actions
- **Domain:** https://www.eugenepeng.com

## Coding Standards & Preferences
- **Languages:** Use TypeScript for configuration and logic. Use standard Markdown/MDX for content.
- **Styling:** The Astro Pure theme utilizes UnoCSS (Tailwind-compatible). Prefer modifying UnoCSS configurations or utility classes before writing custom CSS.
- **Paths:** Since the site is hosted on a custom root domain, do **not** configure base paths in `astro.config.ts` or internal routing.

## Important Commands
- Run local development server: `npm run dev`
- Build for production: `npm run build`
- Theme package check: `npm run pure check`

## Agent Workflow Instructions
- When adding new blog posts or projects, place them in the appropriate content collections (`src/content/`).
- Before modifying core layout files, verify if the change can be achieved by updating the configuration objects in `src/site.config.ts`.
- When diagnosing deployment issues, review `.github/workflows/deploy.yml` and ensure the `actions/deploy-pages` output aligns with GitHub Pages requirements.