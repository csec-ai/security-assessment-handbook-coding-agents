# Task 1 Report: CSS Tokens + Fonts

## Status
DONE

## Commits Made
- `c5c6cbb` — refactor: replace CSS tokens with neutral premium palette, add Inter + JetBrains Mono fonts

## Summary of Changes

### Step 1: Google Fonts link tag
Added 3 `<link>` tags immediately after the viewport meta tag (before `<title>`):
- `preconnect` to fonts.googleapis.com
- `preconnect` to fonts.gstatic.com (crossorigin)
- Google Fonts stylesheet loading Inter (400/500/600/700) and JetBrains Mono (400/600/700)

### Step 2: `:root` token block replaced
Replaced old light-mode palette with the new neutral premium palette:
- Page background: `#eef2f7` → `#f8f9fa`
- Panel background: `white` → `#ffffff`
- Text primary: `#1a202c` → `#0d0f12`
- Shadows updated to more subtle double-layer values
- Added two new tokens: `--accent: #0f4c81` and `--accent-bg: #eff6ff`
- Minor adjustments to `--row-active`, `--step-active-bg`, `--step-active-bd`

### Step 3: `html.dark` token block replaced
Replaced old dark-mode palette with the new darker, higher-contrast palette:
- Page background: `#0f172a` → `#0d0f12`
- Panel background: `#1e293b` → `#1a1d23`
- Text secondary: `#e2e8f0` → `#cbd5e0` (reduced brightness)
- Text muted/faint: adjusted for improved hierarchy
- Shadows updated to double-layer values
- `--step-active-bd`: `#60a5fa` → `#3b82f6`
- Added two new tokens: `--accent: #3b82f6` and `--accent-bg: #1e3a5f`

### Step 4: Body font-family updated
`-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif`
→ `'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`

### Step 5: Monospace font updated (4 occurrences)
All 4 occurrences of `'SF Mono','Fira Code',monospace` replaced with `'JetBrains Mono', 'SF Mono', 'Fira Code', monospace`:
- `.tf-btn .tf-id`
- `.threat-sel-btn .ts-id`
- `.filter-chip .fc-id`
- `.mitre-tactic`

## Concerns / Observations
- None. All changes are purely CSS token replacements and a font link tag addition. No HTML structure or JS was touched. The dark/light theme toggle, SVG diagrams, and interactive filtering remain unaffected.
- The LF→CRLF git warning is cosmetic (Windows line ending normalization) and does not affect functionality.
