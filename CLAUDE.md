# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Galactic Passwords - A single-page application for generating strong, memorable passwords using the pw.bkend.app API. Built with SvelteKit 2 and Svelte 5, styled with Tailwind CSS v4 using the Rosé Pine Moon color palette.

## Commands

All commands use Bun as the package manager:

```bash
bun run dev          # Start development server (http://localhost:5173)
bun run build        # Production build (outputs to /build)
bun run preview      # Preview production build locally
bun run check        # Run svelte-check for type checking
bun run check:watch  # Watch mode for type checking
bun run deploy       # Deploy to Netlify (pw.neontowel.dev)
```

## Architecture

### Technology Stack
- **SvelteKit ^2.22.0** with **Svelte ^5.0.0** (uses Svelte 5 runes and `{@render}` syntax)
- **Tailwind CSS ^4.0.0** with `@theme` directive for CSS custom properties
- **Vite ^7.0.4** as build tool
- **TypeScript ^5.0.0**
- **Netlify adapter** for deployment

### Project Structure
```
src/
├── app.css              # Global styles, Tailwind imports, custom animations
├── app.html             # Root HTML template
├── lib/index.ts         # Shared library exports (placeholder)
├── routes/
│   ├── +layout.svelte   # Root layout with theme background
│   └── +page.svelte     # Main application (all logic here)
└── styles/
    └── rose-pine-moon.css  # Theme CSS variables
```

### Key Implementation Details

**API Integration**: External API at `https://pw.bkend.app/v2/generate/{count}` returns `{ results: [...] }` with password objects containing `sentence`, `source`, and `passwords` (with `noSpaces` and `hyphenated` formats).

**State Management**: Local component state only (no stores). Key state variables: `count`, `passwords`, `loading`, `error`, `countdown`.

**Cooldown Mechanism**: 10-second cooldown enforced after each generation to prevent rate-limiting.

**Styling**: Rose Pine Moon theme colors defined as CSS custom properties in `app.css` using Tailwind's `@theme` block. Custom `.neon-gradient-text` class for animated gradient effects.

### Svelte 5 Patterns
- Uses `$props()` runes for reactive props
- Uses `{@render children()}` snippet syntax in layouts
- Reactive variables declared with `let`
