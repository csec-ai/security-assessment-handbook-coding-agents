# Task 1 Brief: CSS Tokens + Fonts

## Context
You are implementing Task 1 of a theme redesign for a self-contained single-file SPA at `index.html` (1,656 lines). The file has all CSS in a `<style>` block, HTML, and JS in one file. No build step. This task is purely CSS token replacement + adding a Google Fonts link tag — no HTML structure changes, no JS changes.

## Global Constraints
- Single file: all changes to `index.html` only
- No external JS dependencies
- All data arrays are read-only — do not modify
- Existing SVG diagrams and interactive filtering must remain functional
- Dark/light theme toggle must remain functional
- Remove "INTERNAL USE" badge — NOT in this task (Task 2 handles it)
- Font loading: single `<link>` tag, Google Fonts only

## What to do

### Step 1: Add Google Fonts link tag
Insert immediately after line 5 (`<meta name="viewport" content="width=device-width, initial-scale=1.0" />`):

```html
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;600;700&display=swap" rel="stylesheet">
```

### Step 2: Replace `:root` token block
Replace the entire `:root { ... }` block (around lines 11–31) with:

```css
    :root {
      --bg-page:       #f8f9fa;
      --bg-panel:      #ffffff;
      --bg-subtle:     #f1f3f5;
      --bg-input:      #ffffff;
      --border:        #e5e7eb;
      --border-mid:    #d1d5db;
      --text-primary:  #0d0f12;
      --text-secondary:#374151;
      --text-muted:    #6b7280;
      --text-faint:    #9ca3af;
      --shadow-sm:     0 1px 3px rgba(0,0,0,0.06), 0 2px 6px rgba(0,0,0,0.04);
      --shadow-md:     0 1px 3px rgba(0,0,0,0.06), 0 4px 16px rgba(0,0,0,0.04);
      --thead-bg:      #1f2937;
      --thead-color:   #ffffff;
      --row-hover:     #f0f7ff;
      --row-active:    #eff6ff;
      --step-desc-bg:  #fafafa;
      --step-active-bg:#eff6ff;
      --step-active-bd:#bfdbfe;
      --accent:        #0f4c81;
      --accent-bg:     #eff6ff;
    }
```

### Step 3: Replace `html.dark` token block
Replace the entire `html.dark { ... }` block (around lines 32–52) with:

```css
    html.dark {
      --bg-page:       #0d0f12;
      --bg-panel:      #1a1d23;
      --bg-subtle:     #22272e;
      --bg-input:      #1a1d23;
      --border:        #2d3748;
      --border-mid:    #3d4a5c;
      --text-primary:  #f1f5f9;
      --text-secondary:#cbd5e0;
      --text-muted:    #94a3b8;
      --text-faint:    #64748b;
      --shadow-sm:     0 1px 3px rgba(0,0,0,0.4);
      --shadow-md:     0 1px 3px rgba(0,0,0,0.4), 0 4px 16px rgba(0,0,0,0.3);
      --thead-bg:      #0d1117;
      --thead-color:   #f1f5f9;
      --row-hover:     #1e3a5f;
      --row-active:    #1e3a5f;
      --step-desc-bg:  #1a1d23;
      --step-active-bg:#1e3a5f;
      --step-active-bd:#3b82f6;
      --accent:        #3b82f6;
      --accent-bg:     #1e3a5f;
    }
```

### Step 4: Update body font-family
Find the line inside `body { ... }` that sets font-family:
```css
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
```
Replace with:
```css
      font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
```

### Step 5: Update all monospace font references
In the `<style>` block, find every occurrence of:
```css
'SF Mono','Fira Code',monospace
```
Replace each with:
```css
'JetBrains Mono', 'SF Mono', 'Fira Code', monospace
```
(Affects `.tf-btn .tf-id`, `.threat-sel-btn .ts-id`, `.filter-chip .fc-id`, `.mitre-tactic` rules)

### Step 6: Commit
```bash
git add index.html
git commit -m "refactor: replace CSS tokens with neutral premium palette, add Inter + JetBrains Mono fonts"
```

## Report
Write your full report to: `.superpowers/sdd/task-1-report.md`

Report must include:
- Status: DONE | DONE_WITH_CONCERNS | NEEDS_CONTEXT | BLOCKED
- Commits made (short hash)
- Summary of what was changed
- Any concerns or observations
