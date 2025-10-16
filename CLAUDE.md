# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

AstroLinkHub is a JSON-driven, customizable links page built with Astro 4. The entire site content is configured through a single `index.json` file at the root, making it easy to update without touching code.

## Development Commands

**Package Manager**: This project uses `pnpm` as specified in package.json

```bash
# Install dependencies
pnpm install

# Start dev server (http://localhost:4321)
pnpm dev

# Build for production (runs astro check first)
pnpm build

# Preview production build
pnpm preview
```

**Node Requirements**: Node.js v18.17.1, v20.3.0 or higher (v19 not supported)

## Architecture

### JSON-Driven Configuration

The entire site is configured via `index.json` at the project root:

- **html**: Meta tags, title, description, favicon, Open Graph settings
- **background**: Gradient colors (firstColor, secondColor) via CSS custom properties
- **header**: Profile image, name, description, text color
- **socials**: Small icon links displayed below header
- **featured**: Large promotional cards with background images or colors
- **buttons**: Standard link buttons with icons
- **footer**: Copyright text and developer URL

### Component Structure

**Path Aliases** (defined in tsconfig.json):
- `@json` → `index.json` (root configuration file)
- `@/*` → `src/*` (source files)

**Component Hierarchy**:
```
src/pages/index.astro (main page)
└── src/layouts/Layout.astro (HTML structure + global styles)
    └── Section components (Header, Socials, Featured, Buttons, Footer)
        └── src/components/Section.astro (wrapper with max-width styling)
```

**Section Components** (`src/components/sections/`):
Each section imports data directly from `index.json` and renders it:
- `Header.astro` - Profile image, name, description
- `Socials.astro` - Icon-only social links
- `Featured.astro` - Large promotional cards
- `Buttons.astro` - Standard buttons with icons
- `Footer.astro` - Copyright footer

**Pattern**: All section components import data from `index.json`, use the `Section` wrapper component, and include scoped styles.

### Icon System

Icons are stored as individual `.astro` components in `src/icons/`. The `src/utils/Icons.ts` utility exports a `getIcon(network: string)` function that maps network names to icon components.

**Available Icons**:
AstroLogo, Discord, Facebook, GitHub, Instagram, Link, LinkedIn, Mail, Map, Medium, SoundCloud, Spotify, TikTok, Twitch, WhatsApp, X, YouTube

**Adding New Icons**:
1. Add icon component to `src/icons/[Name].astro`
2. Import and add to `SOCIAL_ICONS` object in `src/utils/Icons.ts`
3. Use the icon in `index.json` by setting `"network": "[Name]"`

### Styling Approach

- **Global styles**: Defined in `src/layouts/Layout.astro` using `is:global`
- **Scoped styles**: Each component has its own `<style>` block
- **CSS Variables**: Background gradient colors are passed via `define:vars`
- **Font**: Google Fonts "Onest" loaded in Layout
- **Responsive**: Mobile-first with `@media` queries in Layout

## Making Content Changes

**All content changes should be made in `index.json`** - no code changes needed for typical updates.

To add/modify content:
1. Edit `index.json` sections
2. Use available icon names from the Icons system
3. Colors support hex, rgb, or CSS color names
4. Images should be placed in `/public` and referenced with `/filename`

## Type Safety

TypeScript is configured with Astro's strict config. Type definitions are auto-generated in `.astro/types.d.ts` (not committed).
