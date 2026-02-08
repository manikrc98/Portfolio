# Project State Tracker

## Current Phase
**Phase 6: Asset Cleanup**

## Progress

| Phase | Status | Notes |
|-------|--------|-------|
| Phase 1: Delete Dead CSS | ✅ Complete | Removed 49 lines of CSS, cleaned 11 HTML classes |
| Phase 2: Add Utility Classes | ✅ Complete | Added 16 utility classes, fixed `.card-large` bug, prepped responsive rules |
| Phase 3: Migrate Backgrounds | ✅ Complete | 14 cards migrated to `.bg-*` utilities |
| Phase 4: Migrate Section Grids | ✅ Complete | 8 section grids → `.grid-cols-N`, media queries consolidated |
| Phase 5: Minor Cleanup | ✅ Complete | Inline styles → CSS, CSS variables, 21 aria-labels, 8 video fallbacks |
| Phase 6: Asset Cleanup | Pending | Delete `assets/` folder (83 files), `profile.txt` |

## Completed

### Phase 1: Delete Dead CSS (2026-02-08)
- ✅ Removed 14 empty CSS rule blocks
- ✅ Removed 3 unused CSS rules (.award-content, .award-badge, .award-text)
- ✅ Removed --gradient-blue CSS variable
- ✅ Removed 11 naming-only classes from HTML
- **Commit:** 6502497 "Phase 1: Remove dead CSS and naming-only classes"
- **Impact:** CSS reduced from 907 to 858 lines (-49 lines)

### Phase 2: Add Utility Classes (2026-02-08)
- ✅ Added 16 background & text utility classes after line 230
  - 4 solid backgrounds: `.bg-dark`, `.bg-gray`, `.bg-amber`, `.bg-green`
  - 6 gradient backgrounds: `.bg-gradient-indigo/green/amber/red/blue/violet`
  - 3 branded backgrounds: `.bg-gradient-instagram`, `.bg-twitter`, `.bg-linkedin`
  - 3 text utilities: `.text-white` with child selectors
- ✅ Fixed `.card-large` responsive bug in 600px media query
- ✅ Added `.grid-cols-4` to 1100px media query (prep for Phase 4)
- ✅ Added `.grid-cols-2` and `.grid-cols-4` to 600px media query (prep for Phase 4)
- **Commit:** 0d8cc20 "Phase 2: Add utility classes and fix responsive bugs"
- **Impact:** CSS increased from 857 to 885 lines (+28 lines, additive only)
- **Visual changes:** Zero — utilities not yet applied in HTML

### Phase 3: Migrate Backgrounds (2026-02-08)
- ✅ Migrated 14 cards to use `.bg-*` utility classes in HTML
- ✅ Removed card-specific background CSS rules
- ✅ Deleted orphaned `.instagram-card`, `.twitter-card`, `.linkedin-card` classes
- ✅ Deleted orphaned `.resume-card`, `.framer-card`, `.downloads-card` classes
- ✅ Removed nth-child background selectors from `.visuals-grid`
- **Commit:** 7d8b274 "Phase 3 Completed"
- **Impact:** Backgrounds now managed via reusable utility classes
- **Visual changes:** Zero — all cards retain identical appearance

### Phase 4: Migrate Section Grids (2026-02-08)
- ✅ Replaced 8 section-specific grid classes with `.grid-cols-N` utilities
  - visuals-grid, worked-grid, life-grid, graphic-grid → grid-cols-4
  - reco-grid, social-grid, spotify-grid → grid-cols-4
  - links-grid → grid-cols-2
- ✅ Deleted 10 orphaned grid CSS definitions (85 lines)
- ✅ Replaced `.fun-card-tall` with `.card-tall` throughout
- ✅ Consolidated media query selectors at 1100px and 600px
- ✅ Kept `.live-work-grid` and `.fun-grid` (custom responsive behavior)
- **Commit:** d4bfa47 "Phase 4: Migrate section grids to utility classes"
- **Impact:** CSS reduced from 815 to 730 lines (-85 lines, 10.4% reduction)
- **Visual changes:** Zero — all grids maintain identical layout

### Bug Fix: Mobile Section Overlap (2026-02-08)
- ✅ Fixed section overlap on mobile (<600px breakpoint)
- ✅ Added `grid-auto-rows: auto` to mobile media query
- **Root cause:** Cards with aspect ratios (`.company-card`, `.graphic-card`, etc.) tried to maintain their proportions but were constrained to 175px grid rows, causing overflow
- **Commit:** 80c0183 "Fix mobile section overlap by resetting grid-auto-rows"
- **Impact:** Mobile layout now respects card aspect ratios and natural sizing
- **Visual changes:** Sections no longer overlap on mobile devices

## Blockers / Questions
(none yet)

## Key Decisions Made
- Keep `.live-work-grid` and `.fun-grid` as-is (unique responsive behavior)
- Keep `.cc-rewards-card` and `.personal-finance-card` class names (referenced in media queries)
- Keep `.drive-card` class (has real CSS rule for `object-position`)

### Phase 5: Minor Cleanup & Consistency (2026-02-08)
- ✅ Moved inline styles to CSS
  - Removed `style="border-radius:16px"` from 3 Spotify iframes
  - Added `.spotify-card iframe { border-radius: var(--card-radius-sm); }` CSS rule
- ✅ Added CSS variables for repeated colors
  - Added `--color-dark: #1a1a1a` to `:root`
  - Added `--color-amber-text: #92400E` to `:root`
  - Replaced hardcoded colors in `.bg-dark`, `.card-label`, `.link-icon svg`, `.thank-you-text`
- ✅ Accessibility improvements
  - Added `aria-label` attributes to 21 card links with descriptive text
  - Added fallback text "Your browser does not support the video tag." to 8 video elements
- **Commit:** 0b5db2d "Phase 5: Minor Cleanup & Consistency"
- **Impact:** Improved code maintainability, better accessibility for screen readers and non-video browsers
- **Visual changes:** Zero — all styling maintained via CSS variables

## Blockers / Questions
(none yet)

## Key Decisions Made
- Keep `.live-work-grid` and `.fun-grid` as-is (unique responsive behavior)
- Keep `.cc-rewards-card` and `.personal-finance-card` class names (referenced in media queries)
- Keep `.drive-card` class (has real CSS rule for `object-position`)

## Last Updated
2026-02-08 (Phase 5 complete)
