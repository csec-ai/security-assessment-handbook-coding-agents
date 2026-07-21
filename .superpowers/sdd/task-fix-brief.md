# Fix Brief: Final Review Findings

## Context
You are fixing 6 issues found in a final code review of `index.html` (a self-contained single-file SPA). All changes are to `index.html` only.

## Global Constraints
- Single file: all changes to `index.html` only
- No external JS dependencies
- All data arrays are read-only — do not modify
- Existing interactivity must remain functional

## Fixes to apply

### Fix 1: Stat chip trust-boundary fallback (line ~1559 in JS)
In the `renderMit()` function, find the `tops.map(...)` line that generates per-trust-boundary stat chips. It currently has:
```javascript
tops.map(([tb,n]) => `<div class="stat-chip"><div class="sc-num" style="font-size:1.1rem;color:${TRUST_BOUNDARIES[tb]?.stroke||'#2b6cb0'}">${n}</div><div class="sc-label">${tb}</div></div>`).join('');
```
Replace the fallback `'#2b6cb0'` with `'var(--accent)'`:
```javascript
tops.map(([tb,n]) => `<div class="stat-chip"><div class="sc-num" style="font-size:1.1rem;color:${TRUST_BOUNDARIES[tb]?.stroke||'var(--accent)'}">${n}</div><div class="sc-label">${tb}</div></div>`).join('');
```

### Fix 2: Remove orphaned `.header-badge` CSS rule
Find and delete the entire `.header-badge` CSS rule. It looks like:
```css
    .header-badge {
      background: rgba(255,255,255,0.12); border: 1px solid rgba(255,255,255,0.2);
      padding: 6px 16px; border-radius: 20px; font-size: 0.78rem; font-weight: 600; letter-spacing: 0.04em;
    }
```
Delete the whole rule — the HTML element that used it was already removed.

### Fix 3: Fix `1.5px` border in JS template (line ~1580)
In `renderMit()`, find the JS template string that builds the trust-boundary badge `<td>`. It currently has:
```javascript
`<span style="display:inline-block;background:${tf};color:${tc};border:1.5px solid ${tc};border-radius:5px;padding:3px 9px;font-size:0.8rem;font-weight:700;">${m.tb}</span>`
```
Replace `border:1.5px solid ${tc}` with `border:1px solid ${tc}`:
```javascript
`<span style="display:inline-block;background:${tf};color:${tc};border:1px solid ${tc};border-radius:5px;padding:3px 9px;font-size:0.8rem;font-weight:700;">${m.tb}</span>`
```

### Fix 4: Tokenise `.dtoggle-btn` colors
Find the `.dtoggle-btn` CSS rules. They currently look like:
```css
    .dtoggle-btn { padding: 10px 24px; border: none; cursor: pointer; font-size: 0.875rem; font-weight: 600; color: #718096; background: #f7fafc; transition: all 0.15s; }
    .dtoggle-btn:first-child { border-right: 1px solid #e2e8f0; }
    .dtoggle-btn:hover:not(.active) { background: #edf2f7; }
```
Replace with:
```css
    .dtoggle-btn { padding: 10px 24px; border: none; cursor: pointer; font-size: 0.875rem; font-weight: 600; color: var(--text-muted); background: var(--bg-subtle); transition: all 0.15s; }
    .dtoggle-btn:first-child { border-right: 1px solid var(--border); }
    .dtoggle-btn:hover:not(.active) { background: var(--bg-subtle); filter: brightness(0.97); }
```
Also find `.diagram-toggle { ... border: 1px solid #e2e8f0; ... }` and replace `#e2e8f0` with `var(--border)`.

### Fix 5: Fix `.attack-overview` hard-coded border colors
Find the `.attack-overview` CSS rule. It currently has:
```css
    .attack-overview { background: var(--row-active); border: 1px solid #bee3f8; border-radius: 12px; padding: 18px 20px; margin-bottom: 20px; border-left: 4px solid #3182ce; }
```
Replace with:
```css
    .attack-overview { background: var(--accent-bg); border: 1px solid var(--border-mid); border-radius: 12px; padding: 18px 20px; margin-bottom: 20px; border-left: 4px solid var(--accent); }
```

### Fix 6: Add `aria-label` to drawer close button
Find the drawer close button HTML:
```html
    <button class="drawer-close" onclick="closeDrawer()" title="Close">×</button>
```
Replace with:
```html
    <button class="drawer-close" onclick="closeDrawer()" title="Close" aria-label="Close">×</button>
```

### Commit
```bash
git add index.html
git commit -m "fix: final review — tokenise dtoggle colors, attack-overview border, fix accent fallback, remove dead CSS, 1px border consistency, aria-label on drawer close"
```

## Report
Write your full report to: `.superpowers/sdd/task-fix-report.md`

Report must include:
- Status: DONE | DONE_WITH_CONCERNS | NEEDS_CONTEXT | BLOCKED
- Commits made (short hash)
- Summary of each fix applied
- Any concerns
