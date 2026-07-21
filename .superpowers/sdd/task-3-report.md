# Task 3 Report: Pill Tabs + Card Polish

## Status
DONE

## Commits Made
- `b26034f` — refactor: pill tabs, card polish, stat card promotion, accent token

## Summary of Changes

### Step 1: Pill Tab Nav
- Replaced `.tab-nav` underline-style with pill container: padding 10px 16px, gap 6px, border-bottom reduced from 2px to 1px
- Replaced `.tab-btn` underline indicator with filled pill: border-radius 8px, uppercase text, 0.06em letter-spacing, removed border-bottom/margin-bottom trick
- `.tab-btn:hover` now uses `var(--bg-subtle)` instead of rgba overlay
- `.tab-btn.active` now uses `background: var(--accent)` with white text instead of `#2b6cb0` underline

### Step 2: Tab Panel Padding
- `.tab-panel` padding increased from 32px to 36px

### Step 3: Border Width 1.5px → 1px
Updated all 1.5px borders across the style block and one inline style:
- `.diagram-toggle` border and `.dtoggle-btn:first-child` border-right
- `.diagram-wrapper`, `.info-panel`, `.threat-filter-section`
- `.tf-btn`, `.lh-chip`, `.tb-legend-item`
- `.threat-sel-btn`, `.step-desc`, `.step-detail`, `.legend-note`
- `.mit-tb-banner` border and `.mit-tb-banner-clear` border
- `.filter-panel`, `.filter-chip`, `.table-wrap`, `.tb-info-box`, `hr.divider`
- Inline `<div>` in arch tab (border:1.5px solid var(--border))
- Intentionally left: dynamically generated `border:1.5px solid ${tc}` in JS mitigation row — uses a dynamic color variable, not `var(--border)`, and is outside scope

### Step 4: Card Border Radius 10px → 12px
Updated for: `.diagram-wrapper`, `.info-panel`, `.threat-filter-section`, `.filter-panel`, `.attack-overview`, `.stat-chip`
Not changed: `.diagram-toggle` (not in the target list)

### Step 5: Stat Cards Promoted
- `.stats-row` gap 12px → 16px, margin-bottom 20px → 24px
- `.stat-chip` padding 12px 18px → 20px 24px; added flex:1, min-width:120px
- `.stat-chip .sc-num` font-size 1.5rem → 2rem, color `#2b6cb0` → `var(--accent)`, added line-height:1
- `.stat-chip .sc-label` font-size 0.72rem → 0.78rem, color `var(--text-faint)` → `var(--text-muted)`, margin-top 2px → 6px, added font-weight:500

### Step 6: `var(--accent)` Substitutions
Replaced hard-coded hex colors in CSS rules with `var(--accent)`:
- `.dtoggle-btn.active` background
- `.step-item.active .step-circle` background + border-color
- `.step-item:hover .step-circle` border-color (was `#3182ce`)
- `.attack-nav-btn:hover:not([disabled])` background
- `.mit-expand-icon.open` color
- `.data-row:hover .mit-expand-icon` color
- `.tb-legend-item.active` border-color (was `#3182ce`)

Remaining `#2b6cb0`/`#3182ce` occurrences are intentionally left:
- CSS custom property fallbacks: `var(--ts-stroke,#2b6cb0)`, `var(--fc-stroke,#2b6cb0)` etc — these are fallbacks, not standalone overrides
- TRUST_BOUNDARIES data array — read-only per brief
- SVG/diagram rendering code (mkMarker, arrow colors, component strokes) — functional SVG code

## Concerns / Observations
- The dynamic JS-generated mitigation table rows (`border:1.5px solid ${tc}`) were left with 1.5px since the color is dynamic (TRUST_BOUNDARIES stroke), not `var(--border)` or `var(--border-mid)` as specified in the brief.
- The inline-style `border:1.5px solid var(--border)` at line ~409 (inside the arch tab) was also updated to 1px for consistency, even though the brief only mentioned the `<style>` block — this is a minor extra improvement with no downside.
- No functional changes: filtering, dark/light toggle, SVG diagrams, and tab switching all remain unchanged.
