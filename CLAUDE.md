# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Flow Web** is a static website generator using Pug templates and Tailwind CSS. The project builds a collection of marketing pages for Flow, an AI-powered web application builder platform.

**Key Technologies:**
- **Pug**: Template language for generating HTML
- **Tailwind CSS 3**: Utility-first CSS framework for styling
- **Alpine.js**: Lightweight JavaScript framework for interactivity
- **BrowserSync**: Development server with live reload
- **npm-run-all**: Task orchestration

## Build & Development Commands

### Common Tasks

```bash
# Development: Build + watch + live server
npm run watch

# Production build
npm run build

# Clean output directory
npm run clean

# Build only
npm run build

# Watch specific parts (useful during development):
npm run watch-templates    # Watch Pug files
npm run watch-css          # Watch CSS changes
npm run browsersync        # Start dev server with live reload
```

### What Each Build Step Does

1. **clean**: Removes all files from `./public/`
2. **copy-assets**: Copies static files from `src/assets/` to `public/`
3. **templates**: Compiles Pug files from `src/pug/` to HTML in `public/`
4. **css**: Runs Tailwind CSS compiler (builds and minifies)

The `build` script runs: `clean → copy-assets → templates → css` in sequence.

## Project Structure

```
src/
├── pug/                    # Pug template files (.pug)
│   ├── index.pug          # Homepage
│   ├── about.pug          # About page
│   ├── pricing.pug        # Pricing page
│   ├── contact.pug        # Contact page
│   ├── blog.pug           # Blog page
│   ├── login.pug          # Login page
│   ├── register.pug       # Register page
│   └── jd_applied_research_engineer.pug  # Job posting
├── tailwind/
│   ├── tailwind.css       # Tailwind input CSS
│   └── tailwind.config.js # Tailwind configuration & theme
└── assets/                # Static files (images, favicons, etc.)

public/                    # Generated output (git-ignored)
├── index.html
├── css/
│   └── tailwind/
│       ├── tailwind.css
│       └── tailwind.min.css
└── [other compiled pages]
```

## Code Architecture

### Pug Templates

All page templates are in `src/pug/` and compile to individual HTML files in `public/`. Each `.pug` file creates a corresponding `.html` file.

**Key patterns in Pug files:**
- Use `x-data` and `x-on:click` for Alpine.js interactivity
- Mobile nav toggle uses `mobileNavOpen` state
- External links to branding assets stored in image paths
- Template uses semantic HTML5 elements
- Classes are Tailwind utilities applied inline

**Common structure:**
```pug
html(lang="en")
  head
    title [Page Title]
    meta(charset="utf-8")
    meta(name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no")
    link(rel="stylesheet" href="css/tailwind/tailwind.min.css")
    script(src="https://cdn.jsdelivr.net/npm/alpinejs@3.13.3/dist/cdn.min.js" defer)
  body(class="antialiased bg-body text-body font-body")
    // Page content here
```

### Tailwind CSS Configuration

The Tailwind config (`src/tailwind/tailwind.config.js`) uses Tailwind v3 with extensive customizations:

**Custom Colors:**
- Base colors: pink, gray, lime, teal, red, orange, green, blue
- All colors have 50-950 shade variants for flexibility
- Custom text color: `text-body: '#1D1F1E'` (dark gray)

**Custom Spacing:**
- Extends default spacing with values up to `470` (117.5rem)
- Commonly used in padding, margin, width/height utilities

**Custom Animations:**
- `spin`, `spinSlow`, `spinStar`, `ping`, `pulse`, `bounce`
- Blur effects with multiple sizes (sm, md, lg, xl, 2xl, 3xl, 4xl)

**Font Families:**
- `font-body`, `font-heading`: Figtree (primary)
- `font-sans`: Figtree, DM Sans
- `font-serif`, `font-mono`: Standard fallbacks

**Theme Generation:**
- Content paths: Watches `.pug`, `.html`, `.js` files in src
- All utilities are scanned automatically; unused CSS is pruned in production

### Static Assets

Place images, favicons, and other static files in `src/assets/`. These are copied verbatim to `public/` during the build.

The templates reference assets like:
```pug
img(src="images/logo-white2.svg" alt="")
img(src="fauna-assets/headers/bg-waves.png" alt="")
```

## Development Workflow

1. **Make changes** to `.pug` files in `src/pug/` or CSS in `src/tailwind/`
2. **Run `npm run watch`** to start live development server
3. **BrowserSync** automatically reloads pages when files change
4. **Check output** in `./public/` (don't edit directly—it's generated)
5. **Build for production** with `npm run build` (minifies CSS)

## Common Modifications

### Adding a New Page

1. Create `src/pug/newpage.pug` with the standard HTML structure
2. Run `npm run templates` or wait for watch mode to compile
3. Output appears as `public/newpage.html`

### Updating Tailwind Theme

Edit `src/tailwind/tailwind.config.js`. Changes to colors, spacing, or animations are picked up automatically during build.

### Adding Static Assets

Place files in `src/assets/`, then reference them in templates (e.g., `img(src="images/myfile.png")`). Run `npm run copy-assets` to update `public/`.

### Modifying Styles

Tailwind utilities are applied inline in Pug class attributes. Since the config scans `.pug` files, any new utility combination will automatically be included in the generated CSS during build.

## Notes

- **npm overwrite warning**: The README notes that npm commands overwrite `./public/`. Always review generated files and commit template changes, not build outputs.
- **Image placeholders**: Current assets use images from Unsplash. Replace with actual images as needed.
- **Alpine.js**: Provides lightweight interactivity (mobile menu toggling, state management) without a full framework.
- **Git**: `.gitignore` excludes `public/` directory; only commit source files in `src/`.
