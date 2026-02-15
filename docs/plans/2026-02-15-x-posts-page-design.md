# X Posts Page — Design Document

## Goal
Create a content hub page that displays curated X (Twitter) posts alongside the existing blog section, aggregating all content in one site.

## Approach
**Separate `tweets.html` page** — a standalone HTML file following the same design system as `index.html`. Posts are manually curated (no API, no embeds).

## Page Structure

### File: `tweets.html`
- Same `<head>` as `index.html`: Tailwind CDN, Inter font, Lucide Icons, dark mode support
- Glassmorphism nav bar (duplicated from index, with "Tweets" link active)
- Dark/light mode toggle with localStorage persistence
- Same footer as `index.html`
- No Cmd+K command palette

### Nav Update in `index.html`
- Add "Tweets" link in desktop nav and mobile menu, pointing to `tweets.html`

## Page Header
- Title: "Latest from **X**" (gradient text on "X")
- Subtitle: "Thoughts on AI, cloud, and tech"
- "Follow @rahulx" button linking to X profile

## Tweet Card Design
Each card is a glassmorphism card (`card-glow` style) containing:
- Profile image (small, rounded)
- Name "Rahul Vats" + handle "@rahulx"
- Post date (e.g., "Feb 10, 2026")
- Tweet text (hashtags highlighted in violet)
- "View on X" link to the actual tweet
- No engagement stats (likes/retweets/replies)

## Layout
- Responsive grid: 1 column (mobile) / 2 columns (tablet) / 3 columns (desktop)
- Matches the existing blog cards grid pattern

## Styling
- Same color system: violet-500 / blue-500 accents, gradient text
- Dark bg: #0a0a0f / Light bg: slate-50
- Glassmorphism cards with hover glow effect
- Reveal animations via Intersection Observer

## Initial Content
- 3-5 placeholder tweet cards for manual replacement with real content

## What Changes in Existing Files
- `index.html`: Add "Tweets" nav link (desktop + mobile)
