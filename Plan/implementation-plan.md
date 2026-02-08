# Portfolio Codebase Refactoring Plan

## Context

The portfolio is a static HTML/CSS bento-style site (`index.html` 622 lines, `styles.css` 907 lines). A standardized CSS class system was documented in CLAUDE.md but never fully applied — the codebase still has ~30 context-specific card classes (many empty), duplicate grid definitions for every section, backgrounds hardcoded via nth-child selectors and named card rules instead of `.bg-*` utilities, and scattered media queries. The `assets/` folder (83 files, ~150MB) is entirely legacy Framer export content with zero references. This refactoring aligns the code to the documented utility class system, removes dead code, and makes the site scalable for adding new sections/cards by composing reusable classes.

**Files to modify:** `styles.css`, `index.html`
**Files to delete:** `assets/` folder (83 files), `portfolio-resources/profile.txt`
**No third-party libraries.** Pure HTML + CSS only.

---

## Phase 1: Delete Dead CSS (Zero Visual Risk)

Remove code that has absolutely no effect.

### 1a. Remove 14 empty rule blocks from `styles.css`

| Rule | Line |
|------|------|
| `.api-playground-card {}` | L249 |
| `.compliance-card {}` | L255 |
| `#lazy-load {}` | L285 |
| `#crash-course {}` | L287 |
| `#thank-tank {}` | L289 |
| `.visuals-grid .visual-card:nth-child(5) {}` | L320 |
| `.guitar-card {}` | L351 |
| `.japanese-card {}` | L353 |
| `.solo-card {}` | L355 |
| `.paint-card {}` | L359 |
| `.award-card {}` | L377 |
| `.trophy-card {}` | L399 |
| `.speaking-card {}` | L401 |
| `.mentoring-card {}` | L403 |

### 1b. Remove 3 unused rule blocks with declarations

- `.award-content` (L379-381) — never referenced in HTML
- `.award-badge` (L383-391) — never referenced in HTML
- `.award-text` (L393-397) — never referenced in HTML

### 1c. Remove dead CSS variable

- `--gradient-blue: rgba(96, 165, 250, 0.4);` (L19) — defined but never used

### 1d. Remove naming-only classes from `index.html`

Classes with no CSS rules (empty or nonexistent) — remove from HTML `class` attributes:

| Class to Remove | Section | Note |
|----------------|---------|------|
| `.jupiter-card` | Worked At | No CSS |
| `.zeta-card` | Worked At | No CSS |
| `.headout-card` | Worked At | No CSS |
| `.guitar-card` | Life Learner | Empty CSS deleted in 1a |
| `.japanese-card` | Life Learner | Empty CSS deleted in 1a |
| `.solo-card` | Life Learner | Empty CSS deleted in 1a |
| `.paint-card` | Life Learner | Empty CSS deleted in 1a |
| `.award-card` | Proud Of | Empty CSS deleted in 1a |
| `.trophy-card` | Proud Of | Empty CSS deleted in 1a |
| `.speaking-card` | Proud Of | Empty CSS deleted in 1a |
| `.mentoring-card` | Proud Of | Empty CSS deleted in 1a |

**Keep `.drive-card`** — it has a real CSS rule (`.drive-card .card-image { object-position: center top; }` at L365-367).

> **Checkpoint:** Browser verify at all 3 breakpoints (>1100px, 600-1100px, <600px). Should be pixel-identical.

---

## Phase 2: Add Background & Text Utility Classes (Additive, Zero Visual Risk)

Add the `.bg-*` and `.text-white` utility classes to `styles.css`. Nothing references them yet, so no visual change.

### 2a. Add after the `.grid-cols-2` block (after L230)

```css
/* Background Utilities - Solids */
.bg-dark { background: #1a1a1a; }
.bg-gray { background: #F5F5F5; }
.bg-amber { background: #FEF3C7; }
.bg-green { background: #34D399; }

/* Background Utilities - Gradients */
.bg-gradient-indigo { background: linear-gradient(135deg, #4F46E5, #7C3AED); }
.bg-gradient-green { background: linear-gradient(135deg, #10B981, #059669); }
.bg-gradient-amber { background: linear-gradient(135deg, #FCD34D, #F59E0B); }
.bg-gradient-red { background: linear-gradient(135deg, #EF4444, #DC2626); }
.bg-gradient-blue { background: linear-gradient(135deg, #3B82F6, #2563EB); }
.bg-gradient-violet { background: linear-gradient(135deg, #6366F1, #8B5CF6); }

/* Background Utilities - Branded */
.bg-gradient-instagram { background: linear-gradient(135deg, #F58529, #DD2A7B, #8134AF); }
.bg-twitter { background: #000000; }
.bg-linkedin { background: #0077B5; }

/* Text Utilities */
.text-white { color: white; }
.text-white .social-icon,
.text-white .social-handle { color: white; }
```

### 2b. Fix `.card-large` responsive bug

`.card-large` uses `!important` (L269-270) but isn't reset in the mobile media query (L825-829). Add it:

```css
/* In @media (max-width: 600px) — replace L825-829 */
.card-tall,
.card-wide,
.card-large {
    grid-row: span 1 !important;
    grid-column: span 1 !important;
}
```

### 2c. Add `.grid-cols-4` and `.grid-cols-2` to responsive rules

Prep for Phase 4 — add utility class selectors alongside existing section-specific ones:

**1100px query:** Add `.grid-cols-4` to the 2-column collapse rule alongside `.grid-cols-3`
**600px query:** Add `.grid-cols-2`, `.grid-cols-4` to the 1fr collapse selector

> **Checkpoint:** Should be identical at desktop/tablet. The `.card-large` fix may improve mobile layout for large cards.

---

## Phase 3: Migrate Backgrounds to Utility Classes (HTML + CSS)

For each card: add `.bg-*` class in HTML, remove `background:` from card-specific CSS rule, delete the empty rule if nothing remains.

### Migration Table

| Card | Current CSS Rule | Add to HTML | CSS Action |
|------|-----------------|-------------|------------|
| CC Rewards | `.cc-rewards-card` L245-247 | `bg-gradient-indigo` | Delete background declaration. **Keep class name** in HTML — referenced in 1100px media query L762 |
| Personal Finance | `.personal-finance-card` L251-253 | `bg-dark` | Delete background declaration. **Keep class name** in HTML — referenced in 1100px media query L761 |
| Visuals card 1 (Money Wrapped) | `.visuals-grid :nth-child(1)` L304-306 | `bg-gradient-green` | Delete nth-child rule |
| Visuals card 2 (Gratitude Initiative) | `.visuals-grid :nth-child(2)` L308-310 | `bg-gradient-amber` | Delete nth-child rule |
| Visuals card 3 (CC Pitch) | `.visuals-grid :nth-child(3)` L312-314 | `bg-gradient-red` | Delete nth-child rule |
| Visuals card 4 (Sodexo Spain) | `.visuals-grid :nth-child(4)` L316-318 | `bg-gradient-blue` | Delete nth-child rule |
| Downloads | `.downloads-card` L405-407 | `bg-gradient-violet` | Delete rule. Remove class from HTML |
| Map | `.map-card` L508-512 | `bg-green text-white` | Remove only `background:` line. Keep `aspect-ratio`, `min-height` |
| Instagram | `.instagram-card` L533-540 | `bg-gradient-instagram text-white` | Delete background + color override rules. Remove class from HTML |
| Twitter | `.twitter-card` L542-549 | `bg-twitter text-white` | Delete background + color override rules. Remove class from HTML |
| LinkedIn | `.linkedin-card` L551-558 | `bg-linkedin text-white` | Delete background + color override rules. Remove class from HTML |
| Map (colors) | `.map-card .social-icon/handle` L560-563 | (already `text-white` above) | Delete color override rule |
| Resume | `.resume-card, .framer-card` L611-614 | `bg-gray` | Delete rule. Remove `.resume-card` from HTML |
| Framer | (same rule) | `bg-gray` | Remove `.framer-card` from HTML |
| Email | `.email-card` L616-620 | `bg-dark` | Remove only `background:`. Keep `color: white; padding: 0` |
| Thank-you | `.thank-you-card` L629 | `bg-amber` | Remove only `background:`. Keep `padding`, `display`, `align-items` |

Also remove `.visual-card` and `.sodexo-card` from HTML — no remaining CSS rules after nth-child deletion.

> **Checkpoint:** Every card must have the exact same background color/gradient as before.

---

## Phase 4: Migrate Section Grids to Utility Classes (HTML + CSS)

Replace section-specific grid class names with `.grid-cols-N` in HTML, then delete orphaned CSS.

### 4a. Trivial replacements (CSS definitions identical to utility classes)

| HTML Change | CSS Lines to Delete |
|-------------|---------------------|
| `bento-grid worked-grid` → `bento-grid grid-cols-4` | L325-328 |
| `bento-grid life-grid` → `bento-grid grid-cols-4` | L346-348 |
| `bento-grid graphic-grid` → `bento-grid grid-cols-4` | L441-444 |
| `bento-grid reco-grid` → `bento-grid grid-cols-4` | L453-456 |
| `bento-grid social-grid` → `bento-grid grid-cols-4` | L493-496 |
| `bento-grid spotify-grid` → `bento-grid grid-cols-4` | L568-571 |
| `bento-grid links-grid` → `bento-grid grid-cols-2` | L589-591 |
| `bento-grid visuals-grid` → `bento-grid grid-cols-4` | L299-302 |

### 4b. Already-orphaned grid CSS to delete

- `.proud-grid` (L372-375) — HTML already uses `grid-cols-4`
- `.photos-grid` (L416-419) — HTML already uses `grid-cols-4`

### 4c. Keep as-is (unique responsive behavior)

- **`.live-work-grid`** — custom `grid-template-rows: 200px 200px` at 1100px
- **`.fun-grid`** — custom `repeat(3, 1fr)` and `grid-template-rows: 200px` at 1100px

### 4d. Consolidate media queries

**1100px query** — replace scattered section selectors with:
```css
.grid-cols-3,
.grid-cols-4 {
    grid-template-columns: repeat(2, 1fr);
}
```
Delete: `.visuals-grid, .life-grid, .social-grid` (L775-779), duplicate `.social-grid` (L781-783), `.spotify-grid` (L785-787), and remove `.worked-grid, .proud-grid, .graphic-grid, .reco-grid, .photos-grid` from L789-796.

**600px query** — consolidate to:
```css
.live-work-grid,
.fun-grid,
.grid-cols-2,
.grid-cols-3,
.grid-cols-4 {
    grid-template-columns: 1fr;
}
```

### 4e. Fix `.fun-card-tall` duplication

- Replace `.fun-card-tall` with `.card-tall` in HTML
- Delete `.fun-card-tall { grid-row: span 2; }` from CSS
- In 1100px query, change `.fun-card-tall { grid-row: span 1; }` to `.fun-grid .card-tall { grid-row: span 1; }`

> **Checkpoint:** Test all 13 sections at desktop, tablet, mobile. Pay special attention to grid layouts.

---

## Phase 5: Minor Cleanup & Consistency

### 5a. Move inline styles to CSS

- Remove `style="border-radius:16px"` from 3 Spotify iframes in HTML
- Add CSS: `.spotify-card iframe { border-radius: var(--card-radius-sm); }`

### 5b. Add CSS variables for repeated hardcoded colors

Add to `:root`:
```css
--color-dark: #1a1a1a;
--color-amber-text: #92400E;
```
Replace hardcoded usages in `.card-label`, `.link-icon svg`, `.thank-you-text`.

### 5c. Accessibility improvements

- Add `aria-label` to all `<a>` cards that contain only media (~18 links)
- Add fallback text inside `<video>` elements: `Your browser does not support the video tag.`

> **Checkpoint:** No visual changes. Verify with browser accessibility inspector.

---

## Phase 6: Asset Cleanup

### 6a. Delete entire `assets/` folder

83 files, ~150MB, zero references in HTML/CSS. All legacy Framer export.

### 6b. Delete `portfolio-resources/profile.txt`

Unused metadata file.

### 6c. Future considerations (not in scope)

- Large files: `Virtual_Cards.mov` (15M), `HandClicked_Image_2.JPG` (5.4M) — compression candidates
- `.mov` files only play in Safari — consider `.mp4` conversion
- File naming inconsistency: `Livework_Image3.png` vs `LiveWork_Image2.png`

> **Checkpoint:** Verify no broken images/videos.

---

## Expected Outcome

| Metric | Before | After |
|--------|--------|-------|
| CSS lines | ~907 | ~750 (~17% reduction) |
| Empty CSS rules | 14 | 0 |
| Section-specific grid classes | 10 | 2 (live-work, fun) |
| Card-specific background rules | 14 | 0 (all via `.bg-*`) |
| Background utility classes | 0 | 16 |
| Inline styles | 3 | 0 |
| Dead CSS variables | 1 | 0 |
| Media query selectors (1100px) | 15+ | 5 |
| `assets/` folder | 83 files / ~150MB | Deleted |
| Accessibility: aria-labels | 0 | ~18 |

**To add a new card after refactoring:** Compose from existing classes — `card` + `card-tall`/`card-wide` + `bg-gradient-*` + `text-white` — no new CSS needed.

---

## Verification Protocol

After each phase:
1. Run `python3 -m http.server 8000`
2. Compare at 3 breakpoints: >1100px, 600-1100px, <600px
3. Check every section for visual regression
4. After all phases: full accessibility audit in browser devtools
