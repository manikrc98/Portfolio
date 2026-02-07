# Portfolio Project Guidelines

## Project Overview
- Static HTML/CSS bento-style portfolio for Manik Chugh (Visual Designer)
- Single-page site: `index.html` + `styles.css`
- Assets in `portfolio-resources/images/` and `portfolio-resources/videos/`
- Legacy assets in `assets/` (hash-prefixed files from Framer export, not referenced in HTML)
- Font: Inter (400, 500, 600, 700) via Google Fonts
- No build tools, frameworks, or package managers — pure client-side static site
- Run locally: `python3 -m http.server 8000`

## GitHub
- Repo: https://github.com/manikrc98/Portfolio
- Branch: `main`
- GitHub CLI: `/tmp/gh_extract/gh_2.65.0_macOS_arm64/bin/gh` (authenticated as `manikrc98`)
- Initial commit `1cb30c8`: baseline before CSS class refactoring

## Reference: bento.me/mux
The design is based on https://bento.me/mux. Key visual patterns from the reference:

### Layout
- Two-column: sticky left sidebar (profile, skills, contact) + scrollable right content
- Content uses bento-style CSS Grid with 4-column, 3-column, and 2-column layouts
- Cards use grid spans: 1x1 (default), 2x1 (wide), 1x2 (tall), 2x2 (large)
- Row height controlled by `--card-size: 175px` via `grid-auto-rows`

### Cards
- White background (#FFFFFF), 24px border-radius, subtle shadow `0 1px 3px rgba(0,0,0,0.04)`
- Hover: lift up 4px + scale 1.02 + stronger shadow `0 8px 24px rgba(0,0,0,0.12)`
- Media (images/videos) are `object-fit: cover` and zoom 1.05x on hover

### Caption Pills
- Positioned at card bottom inside a `.card-overlay` (gradient from black 0.7 to transparent)
- `.card-label`: white rounded pill — `rgba(255,255,255,0.95)` bg, dark text, 12px/500, 20px radius
- `.badge`: semi-transparent tag above label — `rgba(255,255,255,0.2)` + backdrop-blur, white text, 10px uppercase

### Link Arrow Icon
- Top-right circle (28px), white bg, contains SVG arrow (↗)
- Hidden by default, slides in on card hover (opacity 0→1, translate offset→0)

### Colors Used
- Page background: #F3F4F6
- Card backgrounds vary per card (gradients and solids — see class system below)
- Profile ring: yellow #FACC15
- Social cards: branded colors (Instagram gradient, Twitter black, LinkedIn blue)
- Thank-you card: amber #FEF3C7 with #92400E text

### Typography
- Font: Inter, weights 400 (body), 500 (labels/links), 600 (section titles/badges), 700 (name)
- Name: 28px/700, Section titles: 16px/600, Card labels: 12px/500, Badges: 10px/600 uppercase

### Content Sections (13 total)
1. **Live Work** — 4-col grid, video+image project cards with links
2. **Fun Projects** — 4-col grid, Figma plugin, YouTube, initiative
3. **Dope Visuals** — 4-col grid, video cards with gradient backgrounds
4. **Worked At** — 4-col grid, company logo cards (Jupiter, Zeta, Headout)
5. **In 2023, I Learnt to** — 4-col grid, video+image life cards
6. **Proud of** — 3-col grid, awards, speaking, downloads
7. **Hand Clicked** — 3-col grid, photography (3:4 aspect ratio)
8. **Graphic** — 4-col grid, design work (1:1 aspect ratio)
9. **Words Matter** — 4-col grid, recommendation images
10. **Enjoyed Reading** — 3-col grid, book covers (3:4)
11. **Find Me At** — 4-col grid, social + map cards
12. **Listening To** — 4-col grid, Spotify iframe embeds
13. **Important Links** — 2-col grid, resume, portfolio, email, thank-you

---

## Standardized CSS Class System

### Grid Layout (utility — replace section-specific grids)
```css
.grid-cols-4 { grid-template-columns: repeat(4, 1fr); grid-auto-rows: var(--card-size); }
.grid-cols-3 { grid-template-columns: repeat(3, 1fr); grid-auto-rows: var(--card-size); }
.grid-cols-2 { grid-template-columns: repeat(2, 1fr); }
```
Usage: `<div class="bento-grid grid-cols-4">` — replaces `.live-work-grid`, `.fun-grid`, `.visuals-grid`, etc.

### Card Sizing
```css
.card       /* Base: white bg, 24px radius, shadow, hover lift+scale */
.card-tall  /* grid-row: span 2 */
.card-wide  /* grid-column: span 2 */
.card-large /* grid-column: span 2; grid-row: span 2 */
```

### Background Utilities (replace nth-child selectors and scattered bg rules)
```css
/* Solids */
.bg-dark    { background: #1a1a1a; }
.bg-gray    { background: #F5F5F5; }
.bg-amber   { background: #FEF3C7; }
.bg-green   { background: #34D399; }

/* Gradients */
.bg-gradient-indigo    { background: linear-gradient(135deg, #4F46E5, #7C3AED); }
.bg-gradient-green     { background: linear-gradient(135deg, #10B981, #059669); }
.bg-gradient-amber     { background: linear-gradient(135deg, #FCD34D, #F59E0B); }
.bg-gradient-red       { background: linear-gradient(135deg, #EF4444, #DC2626); }
.bg-gradient-blue      { background: linear-gradient(135deg, #3B82F6, #2563EB); }
.bg-gradient-violet    { background: linear-gradient(135deg, #6366F1, #8B5CF6); }

/* Branded */
.bg-gradient-instagram { background: linear-gradient(135deg, #F58529, #DD2A7B, #8134AF); }
.bg-twitter            { background: #000000; }
.bg-linkedin           { background: #0077B5; }
```

Card-to-background mapping:
| Card | Background Class |
|------|-----------------|
| CC Rewards | `.bg-gradient-indigo` |
| Personal Finance | `.bg-dark` |
| Money Wrapped | `.bg-gradient-green` |
| Gratitude Initiative | `.bg-gradient-amber` |
| CC Pitch | `.bg-gradient-red` |
| Sodexo Spain | `.bg-gradient-blue` |
| Plugin Downloads | `.bg-gradient-violet` |
| Instagram | `.bg-gradient-instagram` |
| Twitter/X | `.bg-twitter` |
| LinkedIn | `.bg-linkedin` |
| Map/Location | `.bg-green` |
| Resume, Framer | `.bg-gray` |
| Email | `.bg-dark` |
| Thank-you | `.bg-amber` |

### Text Utilities
```css
.text-white { color: white; }
.text-white .social-icon, .text-white .social-handle { color: white; }
```
Apply to cards with dark/gradient backgrounds that contain social icons.

### Caption System
```css
.card-overlay  /* position:absolute bottom, gradient bg, flex column */
.card-label    /* white pill: rgba(255,255,255,0.95), 12px/500, 20px radius */
.badge         /* transparent tag: rgba(255,255,255,0.2), backdrop-blur, 10px/600 uppercase */
```

### Media
```css
.card-image  /* width/height 100%, object-fit:cover, scale 1.05x on hover */
.card-video  /* same as card-image */
```

### Interactive
```css
.link-icon  /* absolute top-right, 28px white circle, arrow SVG, hidden → slides in on hover */
```

### Card Type Classes (unique layout patterns — keep these)
| Class | Layout |
|-------|--------|
| `.company-card` | Centered flex for company logos, `object-fit: contain` |
| `.social-card` | Centered column flex for icon + handle text |
| `.map-card` | `min-height: 120px`, auto aspect ratio |
| `.photo-card` | `aspect-ratio: 3/4` |
| `.graphic-card` | `aspect-ratio: 1` (square) |
| `.book-card` | `aspect-ratio: 3/4`, no padding |
| `.reco-card` | No padding, auto-height image |
| `.spotify-card` | Transparent bg, no shadow, no hover effect |
| `.link-card` | Horizontal flex, `min-height: 70px`, padding 20px 24px |
| `.email-card` | `color: white; padding: 0` |
| `.thank-you-card` | `padding: 24px; display: flex; align-items: center` |
| `.life-card` | `.card-image { object-position: center }` |
| `.drive-card` | `.card-image { object-position: center top }` |
| `.proud-card` | `.card-image { object-fit: cover }` |

### Responsive Breakpoints
```
> 1100px:  Classes as defined (4-col, 3-col, 2-col)
600-1100px: .grid-cols-4 → 2 cols, .grid-cols-3 → 2 cols, tall/large lose row span
< 600px:   All grids → 1 col, all spans reset, card radius → 16px
```

---

## Key Rules
- No inline CSS — all styles via standard classes
- Background colors/gradients via `.bg-*` utilities, never nth-child or named card classes
- Section grids use `.bento-grid .grid-cols-N`, not section-specific class names
- Empty/dead CSS rules should be removed, not left as placeholders
- Card hover: translateY(-4px) scale(1.02) with stronger shadow
- Media hover: scale(1.05) zoom inside card

## CSS Custom Properties
```css
--bg-color: #F3F4F6;
--card-bg: #FFFFFF;
--text-primary: #000000;
--text-secondary: #666666;
--text-muted: #999999;
--card-radius: 24px;
--card-radius-sm: 16px;
--gap: 40px;
--card-size: 175px;
--sidebar-width: 300px;
```
