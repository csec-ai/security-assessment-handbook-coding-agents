# Task 5 Brief: Inline Accordion Polish

## Context
You are implementing Task 5 of a theme redesign for `index.html`. Tasks 1–4 are complete. This task polishes the existing expand/collapse rows in the Mitigations table — updating CSS to use the `--accent` token, softening the bottom border color, and improving the chevron animation.

## Global Constraints
- Single file: all changes to `index.html` only
- No external JS dependencies
- All data arrays are read-only — do not modify
- Existing expand/collapse behaviour must remain functional

## What to do

### Step 1: Replace `.mit-expand-row` and related CSS rules

Find this exact block in the `<style>` section:
```css
    .mit-expand-row { background: var(--bg-subtle); }
    .mit-expand-row td { padding: 0 !important; background: var(--bg-subtle) !important; border-bottom: 2px solid #bee3f8 !important; }
    .mit-expand-content { padding: 14px 18px 16px; }
    .mit-expand-label { font-size: 0.72rem; font-weight: 700; text-transform: uppercase;
      letter-spacing: 0.08em; color: var(--text-faint); margin-bottom: 6px; }
    .mit-expand-content p { font-size: 0.875rem; color: var(--text-secondary); line-height: 1.65; }
    .mit-expand-icon { display: inline-block; transition: transform 0.2s; color: var(--text-faint); font-size: 0.75rem; }
    .mit-expand-icon.open { transform: rotate(180deg); color: #2b6cb0; }
    .data-row:hover .mit-expand-icon { color: #2b6cb0; }
```

Replace with:
```css
    .mit-expand-row { background: var(--bg-subtle); }
    .mit-expand-row td { padding: 0 !important; background: var(--bg-subtle) !important; border-bottom: 2px solid var(--border-mid) !important; }
    .mit-expand-content { padding: 16px 20px 18px; }
    .mit-expand-label { font-size: 0.72rem; font-weight: 700; text-transform: uppercase;
      letter-spacing: 0.08em; color: var(--text-faint); margin-bottom: 8px; }
    .mit-expand-content p { font-size: 0.875rem; color: var(--text-secondary); line-height: 1.65; }
    .mit-expand-icon { display: inline-block; transition: transform 0.2s ease; color: var(--text-faint); font-size: 0.75rem; margin-left: 6px; }
    .mit-expand-icon.open { transform: rotate(180deg); color: var(--accent); }
    .data-row:hover .mit-expand-icon { color: var(--accent); }
```

### Step 2: Commit
```bash
git add index.html
git commit -m "refactor: polish accordion expand row styles, use accent token"
```

## Report
Write your full report to: `.superpowers/sdd/task-5-report.md`

Report must include:
- Status: DONE | DONE_WITH_CONCERNS | NEEDS_CONTEXT | BLOCKED
- Commits made (short hash)
- Summary of what was changed
- Any concerns or observations
