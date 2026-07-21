# Task 2 Brief: Header Redesign + Remove Badge

## Context
You are implementing Task 2 of a theme redesign for a self-contained single-file SPA at `index.html`. Task 1 already added Inter/JetBrains Mono fonts and replaced CSS tokens (including new `--accent` and `--accent-bg` tokens). This task updates the header visual and removes the "INTERNAL USE" badge.

## Global Constraints
- Single file: all changes to `index.html` only
- No external JS dependencies
- All data arrays are read-only — do not modify
- Existing SVG diagrams and interactive filtering must remain functional
- Dark/light theme toggle must remain functional

## What to do

### Step 1: Replace `.header` CSS rule
Find the existing `.header` rule which currently looks like:
```css
    .header {
      background: linear-gradient(135deg, #0f2744 0%, #1a3f6f 60%, #1e4d8c 100%);
      color: white; padding: 22px 40px;
      display: flex; align-items: center; justify-content: space-between;
      box-shadow: 0 2px 12px rgba(0,0,0,0.25);
    }
```
Replace it with:
```css
    .header {
      background: linear-gradient(135deg, #0d0f12 0%, #1a1d23 100%);
      color: white; padding: 22px 40px;
      display: flex; align-items: center; justify-content: space-between;
      box-shadow: 0 2px 12px rgba(0,0,0,0.35);
    }
```

### Step 2: Update h1 font size
Find the line:
```css
    .header-left h1 { font-size: 1.5rem; font-weight: 700; letter-spacing: -0.01em; }
```
Replace with:
```css
    .header-left h1 { font-size: 1.75rem; font-weight: 700; letter-spacing: -0.02em; }
```

### Step 3: Remove "INTERNAL USE" badge from HTML
Find this line in the HTML (inside the `<header>` element):
```html
    <div class="header-badge">INTERNAL USE</div>
```
Delete that entire line.

### Step 4: Commit
```bash
git add index.html
git commit -m "refactor: near-black header gradient, remove INTERNAL USE badge, larger h1"
```

## Report
Write your full report to: `.superpowers/sdd/task-2-report.md`

Report must include:
- Status: DONE | DONE_WITH_CONCERNS | NEEDS_CONTEXT | BLOCKED
- Commits made (short hash)
- Summary of what was changed
- Any concerns or observations
