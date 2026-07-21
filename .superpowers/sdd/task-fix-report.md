# Fix Report: Final Review Findings

## Status: DONE

## Commit
- `20cb8bb` — fix: final review — tokenise dtoggle colors, attack-overview border, fix accent fallback, remove dead CSS, 1px border consistency, aria-label on drawer close

## Fixes Applied

### Fix 1: Stat chip trust-boundary fallback (line 1559 in JS)
Replaced hard-coded `'#2b6cb0'` fallback color with `'var(--accent)'` in the `renderMit()` function's `tops.map(...)` template string. This ensures the stat chip number color respects the CSS token in both light and dark modes when a trust boundary stroke is not found.

### Fix 2: Remove orphaned `.header-badge` CSS rule
Deleted the entire `.header-badge` CSS block (4 lines, formerly lines 85–88). The HTML element that referenced this class had already been removed; the dead rule was causing unnecessary bloat.

### Fix 3: Fix `1.5px` border in JS template (line 1576)
Changed `border:1.5px solid ${tc}` to `border:1px solid ${tc}` in the trust-boundary badge `<span>` inside `renderMit()`. Sub-pixel border widths produce inconsistent rendering across browsers; 1px is the standard.

### Fix 4: Tokenise `.dtoggle-btn` colors
- `.dtoggle-btn`: replaced `color: #718096` with `color: var(--text-muted)` and `background: #f7fafc` with `background: var(--bg-subtle)`.
- `.dtoggle-btn:first-child`: replaced `border-right: 1px solid #e2e8f0` with `border-right: 1px solid var(--border)`.
- `.dtoggle-btn:hover:not(.active)`: replaced `background: #edf2f7` with `background: var(--bg-subtle); filter: brightness(0.97)`.
- `.diagram-toggle`: replaced `border: 1px solid #e2e8f0` with `border: 1px solid var(--border)`.

### Fix 5: Fix `.attack-overview` hard-coded border colors
- `border: 1px solid #bee3f8` replaced with `border: 1px solid var(--border-mid)`.
- `border-left: 4px solid #3182ce` replaced with `border-left: 4px solid var(--accent)`.
- `background: var(--row-active)` replaced with `background: var(--accent-bg)`.

### Fix 6: Add `aria-label` to drawer close button
Added `aria-label="Close"` to the `<button class="drawer-close" ...>` element at line 1820. This improves accessibility for screen readers, which otherwise only see the raw `×` character (a multiplication sign) without a meaningful label.

## Concerns
None. All fixes were straightforward, localized changes with no risk of regressions. The `--accent-bg` and `--border-mid` CSS variables are assumed to be defined in the existing token set (consistent with how other rules reference them in the file). No data arrays were modified.
