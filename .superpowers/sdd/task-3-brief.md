# Task 3 Brief: Pill Tabs + Card Polish

## Context
You are implementing Task 3 of a theme redesign for `index.html`. Tasks 1 and 2 are complete:
- Task 1: CSS tokens replaced (new `--accent` and `--accent-bg` tokens exist), Inter + JetBrains Mono fonts added
- Task 2: Near-black header gradient, INTERNAL USE badge removed, h1 enlarged

This task converts the tab underline indicator to filled pill tabs, increases card spacing/radius, lightens borders from 1.5px to 1px, promotes stat chips to stat cards, and replaces hard-coded `#2b6cb0` accent references with `var(--accent)`.

## Global Constraints
- Single file: all changes to `index.html` only
- No external JS dependencies
- All data arrays are read-only — do not modify
- Existing SVG diagrams and interactive filtering must remain functional
- Dark/light theme toggle must remain functional

## What to do

### Step 1: Replace `.tab-nav` and `.tab-btn` CSS rules

Find the existing block:
```css
    .tab-nav {
      display: flex; flex-wrap: wrap; background: var(--bg-panel); border-radius: 12px 12px 0 0;
      box-shadow: var(--shadow-sm); border-bottom: 2px solid var(--border);
    }
    .tab-btn {
      flex: 1; padding: 16px 28px; border: none; background: transparent; cursor: pointer;
      font-size: 0.9rem; font-weight: 500; color: var(--text-muted); text-align: center;
      border-bottom: 3px solid transparent; margin-bottom: -2px; transition: color 0.18s, background 0.18s, border-color 0.18s; white-space: nowrap;
    }
    .tab-btn:hover { color: var(--text-primary); background: rgba(0,0,0,0.04); }
    .tab-btn.active { color: #2b6cb0; border-bottom-color: #2b6cb0; background: var(--bg-panel); font-weight: 700; }
```
Replace with:
```css
    .tab-nav {
      display: flex; flex-wrap: wrap; background: var(--bg-panel); border-radius: 12px 12px 0 0;
      box-shadow: var(--shadow-sm); border-bottom: 1px solid var(--border);
      padding: 10px 16px; gap: 6px;
    }
    .tab-btn {
      flex: 1; padding: 10px 24px; border: none; background: transparent; cursor: pointer;
      font-size: 0.875rem; font-weight: 500; color: var(--text-muted); text-align: center;
      border-radius: 8px; text-transform: uppercase; letter-spacing: 0.06em;
      transition: color 0.18s, background 0.18s; white-space: nowrap;
    }
    .tab-btn:hover { color: var(--text-primary); background: var(--bg-subtle); }
    .tab-btn.active { color: #ffffff; background: var(--accent); font-weight: 600; }
```

### Step 2: Update `.tab-panel` padding
Find:
```css
    .tab-panel { display: none; background: var(--bg-panel); border-radius: 0 0 12px 12px; box-shadow: var(--shadow-md); padding: 32px; }
```
Replace with:
```css
    .tab-panel { display: none; background: var(--bg-panel); border-radius: 0 0 12px 12px; box-shadow: var(--shadow-md); padding: 36px; }
```

### Step 3: Replace all `border: 1.5px solid var(--border)` with `border: 1px solid var(--border)`
Do a find-and-replace across the entire `<style>` block:
- Replace: `border: 1.5px solid var(--border)` → `border: 1px solid var(--border)`
- Replace: `border: 1.5px solid var(--border-mid)` → `border: 1px solid var(--border-mid)`

Also update these specific border declarations that use 1.5px:
- `.diagram-toggle { ... border: 1.5px solid #e2e8f0; ... }` → `border: 1px solid #e2e8f0;`
- `.dtoggle-btn:first-child { border-right: 1.5px solid #e2e8f0; }` → `border-right: 1px solid #e2e8f0;`

### Step 4: Replace `border-radius: 10px` with `border-radius: 12px` on cards
Find and replace `border-radius: 10px` with `border-radius: 12px` for these rules (do NOT change border-radius on small elements like chips/badges):
- `.diagram-wrapper`
- `.info-panel`
- `.filter-panel`
- `.threat-filter-section`
- `.stat-chip`
- `.attack-overview`

### Step 5: Replace `.stats-row` and `.stat-chip` rules
Find:
```css
    .stats-row { display: flex; gap: 12px; flex-wrap: wrap; margin-bottom: 20px; }
    .stat-chip { background: var(--bg-panel); border: 1.5px solid var(--border); border-radius: 10px; padding: 12px 18px; }
    .stat-chip .sc-num { font-size: 1.5rem; font-weight: 700; color: #2b6cb0; }
    .stat-chip .sc-label { font-size: 0.72rem; color: var(--text-faint); margin-top: 2px; }
```
Replace with:
```css
    .stats-row { display: flex; gap: 16px; flex-wrap: wrap; margin-bottom: 24px; }
    .stat-chip { background: var(--bg-panel); border: 1px solid var(--border); border-radius: 12px; padding: 20px 24px; flex: 1; min-width: 120px; }
    .stat-chip .sc-num { font-size: 2rem; font-weight: 700; color: var(--accent); line-height: 1; }
    .stat-chip .sc-label { font-size: 0.78rem; color: var(--text-muted); margin-top: 6px; font-weight: 500; }
```

### Step 6: Replace hard-coded `#2b6cb0` accent references with `var(--accent)`
Find and replace these specific occurrences in the `<style>` block:
- `.dtoggle-btn.active { background: #2b6cb0; color: white; }` → `.dtoggle-btn.active { background: var(--accent); color: white; }`
- `.step-item.active .step-circle { background: #2b6cb0; color: white; border-color: #2b6cb0; }` → `.step-item.active .step-circle { background: var(--accent); color: white; border-color: var(--accent); }`
- `.attack-nav-btn:hover:not([disabled]) { background: #2b6cb0; }` → `.attack-nav-btn:hover:not([disabled]) { background: var(--accent); }`
- `.mit-expand-icon.open { transform: rotate(180deg); color: #2b6cb0; }` → `.mit-expand-icon.open { transform: rotate(180deg); color: var(--accent); }`
- `.data-row:hover .mit-expand-icon { color: #2b6cb0; }` → `.data-row:hover .mit-expand-icon { color: var(--accent); }`

Also update `.step-item:hover .step-circle { border-color: #3182ce; }` → `border-color: var(--accent);`
And `.tb-legend-item.active { background: var(--row-active); border-color: #3182ce; }` → `border-color: var(--accent);`

### Step 7: Commit
```bash
git add index.html
git commit -m "refactor: pill tabs, card polish, stat card promotion, accent token"
```

## Report
Write your full report to: `.superpowers/sdd/task-3-report.md`

Report must include:
- Status: DONE | DONE_WITH_CONCERNS | NEEDS_CONTEXT | BLOCKED
- Commits made (short hash)
- Summary of what was changed
- Any concerns or observations
