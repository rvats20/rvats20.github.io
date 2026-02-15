# Portfolio — rahulvts

## Tech Stack
- **Styling**: Tailwind CSS CDN (no build step)
- **Icons**: Lucide Icons CDN (replaces FontAwesome)
- **Font**: Inter (Google Fonts)
- **JS**: Vanilla JavaScript, inline in index.html
- **No dependencies**: No jQuery, no Typed.js, no ScrollReveal, no VanillaTilt, no particle.js

## Architecture
Single-file architecture — everything lives in `index.html`:
- Tailwind config inline via `<script>` tag
- Custom CSS in `<style>` block (animations, glassmorphism, gradient text)
- All JavaScript in `<script>` block at bottom of body

## Key Features
- Dark/Light mode toggle (localStorage persisted, `darkMode: 'class'`)
- Cmd+K command palette (keyboard navigable)
- Scroll progress bar (gradient, fixed top)
- Intersection Observer for reveal animations (replaces ScrollReveal.js)
- CSS typing animation (replaces Typed.js)
- "Available for work" pulsing badge
- Bento grid About section
- Alternating timeline for Experience

## File Structure
```
index.html          — The entire site
Assets/img/         — Images (perfil.png, work1-6.jpg, about.jpg, etc.)
README.md           — Repo readme
CLAUDE.md           — This file
```

## Color System
- Accent: violet-500 (#8b5cf6) / blue-500 (#3b82f6)
- Dark bg: #0a0a0f
- Light bg: slate-50
- Gradient text: violet → blue (135deg)

## Conventions
- All images use `loading="lazy"`
- No dev tools blocking
- No tab-switch title change gimmick
- No Tawk.to chat widget
- External links: LinkedIn, GitHub, X, Hashnode blogs, Google Drive resume
