# Task 4 Brief: Side Drawer

## Context
You are implementing Task 4 of a theme redesign for `index.html`. Tasks 1–3 are complete. This task adds a right-side detail drawer that slides in when:
- An ⓘ icon is clicked on a threat selector button (Tab 2: Attack Path)
- The mitigation text cell is clicked in the mitigations table (Tab 3: Mitigations)

## Global Constraints
- Single file: all changes to `index.html` only
- No external JS dependencies
- All data arrays (`THREATS_DATA`, `ATTACK_CHAINS`, `MITIGATIONS_DATA`) are read-only — do not modify
- Existing SVG diagrams and interactive filtering must remain functional
- Dark/light theme toggle must remain functional

## Key facts about the current JS structure

### Threat selector buttons (Tab 2, built in `initAttack()` around line 1338):
The current button HTML is set at line 1349:
```javascript
btn.innerHTML = `<span class="ts-id">${id}</span><span class="ts-name">${td ? td.name : chain.name.replace(id+': ','')}</span>`;
```
The click handler (line 1350) calls `renderAttack()` to load the attack chain — keep this working.

### Mitigation table rows (Tab 3, built in `renderMit()` around line 1487):
The main row HTML is at line 1514. The first `<td>` renders the mitigation text:
```javascript
<td style="font-weight:600;color:var(--text-primary);">${m.m}</td>
```
The existing row click handler (line 1535) toggles the expand/collapse row — keep this working.

### `MITIGATIONS_DATA` structure:
Each entry has: `m` (text), `ts` (array of threat IDs or `['All']`), `ex` (example), `tb` (trust boundary ID).
Access by index: `MITIGATIONS_DATA[i]`.

### `ATTACK_CHAINS` structure:
Keys are threat IDs (e.g. `'T-001'`). Each has: `name`, `desc`, `steps[]`. Each step has: `step`, `desc`, `tactics[]`, `techniques[]`, `mits[]`.
`techniques` is an array of strings like `'AML.T0079 — Stage Capabilities'`.

## What to do

### Step 1: Add drawer CSS
Add the following CSS block at the END of the `<style>` block (immediately before `</style>`):

```css
    /* ── Detail Drawer ── */
    .drawer-backdrop {
      display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.3); z-index: 200;
    }
    .drawer-backdrop.open { display: block; }
    #detail-drawer {
      position: fixed; top: 0; right: 0; height: 100vh; width: 380px; max-width: 100vw;
      background: var(--bg-panel); border-left: 1px solid var(--border);
      box-shadow: -4px 0 24px rgba(0,0,0,0.12);
      transform: translateX(100%); transition: transform 0.3s ease;
      z-index: 201; display: flex; flex-direction: column; overflow: hidden;
    }
    #detail-drawer.open { transform: translateX(0); }
    .drawer-header {
      display: flex; align-items: flex-start; justify-content: space-between;
      padding: 24px 24px 16px; border-bottom: 1px solid var(--border); flex-shrink: 0;
    }
    .drawer-header-left { flex: 1; min-width: 0; }
    .drawer-type-badge {
      display: inline-block; padding: 3px 10px; border-radius: 12px;
      font-size: 0.72rem; font-weight: 700; text-transform: uppercase;
      letter-spacing: 0.06em; margin-bottom: 10px;
      background: var(--accent-bg); color: var(--accent);
    }
    .drawer-title { font-size: 1.1rem; font-weight: 700; color: var(--text-primary); line-height: 1.35; }
    .drawer-close {
      flex-shrink: 0; margin-left: 12px; margin-top: -4px;
      background: none; border: 1px solid var(--border); border-radius: 8px;
      width: 32px; height: 32px; cursor: pointer; font-size: 1.1rem;
      color: var(--text-muted); display: flex; align-items: center; justify-content: center;
      transition: background 0.15s, color 0.15s;
    }
    .drawer-close:hover { background: var(--bg-subtle); color: var(--text-primary); }
    .drawer-body { flex: 1; overflow-y: auto; padding: 20px 24px 32px; }
    .drawer-section { margin-bottom: 20px; }
    .drawer-section-label {
      font-size: 0.72rem; font-weight: 700; text-transform: uppercase;
      letter-spacing: 0.08em; color: var(--text-faint); margin-bottom: 8px;
    }
    .drawer-section p { font-size: 0.875rem; color: var(--text-secondary); line-height: 1.65; }
    .drawer-divider { border: none; border-top: 1px solid var(--border); margin: 16px 0; }
    .drawer-mitre-row { display: flex; flex-wrap: wrap; gap: 6px; margin-bottom: 8px; }
    .drawer-mit-item {
      padding: 8px 12px; border-radius: 8px; border: 1px solid var(--border);
      background: var(--bg-subtle); font-size: 0.875rem; color: var(--text-primary);
      line-height: 1.5; margin-bottom: 6px;
    }
    .drawer-mit-item::before { content: '→ '; color: #16a34a; font-weight: 700; }
    .drawer-steps-list { list-style: none; padding: 0; }
    .drawer-steps-list li {
      display: flex; gap: 10px; padding: 8px 0;
      border-bottom: 1px solid var(--border); font-size: 0.875rem; color: var(--text-secondary); line-height: 1.55;
    }
    .drawer-steps-list li:last-child { border-bottom: none; }
    .drawer-step-num {
      flex-shrink: 0; width: 22px; height: 22px; border-radius: 50%;
      background: var(--accent); color: white;
      font-size: 0.72rem; font-weight: 700; display: flex; align-items: center; justify-content: center;
    }
    @media (max-width: 640px) {
      #detail-drawer { width: 100vw; }
    }
```

### Step 2: Add drawer HTML
Add the following immediately before the closing `</body>` tag:

```html
<!-- Detail Drawer -->
<div class="drawer-backdrop" id="drawer-backdrop" onclick="closeDrawer()"></div>
<div id="detail-drawer" role="dialog" aria-modal="true" aria-label="Detail panel">
  <div class="drawer-header">
    <div class="drawer-header-left">
      <div class="drawer-type-badge" id="drawer-type-badge"></div>
      <div class="drawer-title" id="drawer-title"></div>
    </div>
    <button class="drawer-close" onclick="closeDrawer()" title="Close">×</button>
  </div>
  <div class="drawer-body" id="drawer-body"></div>
</div>
```

### Step 3: Add drawer JavaScript functions
Add the following functions at the END of the `<script>` block (immediately before `</script>`):

```javascript
function openDrawer(type, id) {
  const drawer   = document.getElementById('detail-drawer');
  const backdrop = document.getElementById('drawer-backdrop');
  const badge    = document.getElementById('drawer-type-badge');
  const title    = document.getElementById('drawer-title');
  const body     = document.getElementById('drawer-body');
  let html = '';

  if (type === 'threat') {
    const t = THREATS_DATA.find(x => x.id === id);
    if (!t) return;
    const chain = ATTACK_CHAINS[id];
    badge.textContent = 'Attack Vector';
    title.textContent = t.name;

    html += `<div class="drawer-section"><div class="drawer-section-label">Description</div><p>${t.desc}</p></div>`;
    html += `<hr class="drawer-divider">`;

    if (chain) {
      html += `<div class="drawer-section"><div class="drawer-section-label">Attack Overview</div><p>${chain.desc}</p></div>`;
      html += `<div class="drawer-section"><div class="drawer-section-label">Attack Steps</div><ol class="drawer-steps-list">`;
      chain.steps.forEach(s => {
        html += `<li><span class="drawer-step-num">${s.step}</span><span>${s.desc}</span></li>`;
      });
      html += `</ol></div>`;
      html += `<hr class="drawer-divider">`;

      const allTactics = [...new Set(chain.steps.flatMap(s => s.tactics || []))];
      const allTechs   = [...new Set(chain.steps.flatMap(s => s.techniques || []))];
      if (allTactics.length) {
        html += `<div class="drawer-section"><div class="drawer-section-label">MITRE ATLAS Tactics</div><div class="drawer-mitre-row">`;
        allTactics.forEach(tac => { html += `<span class="mitre-tactic">${tac}</span>`; });
        html += `</div></div>`;
      }
      if (allTechs.length) {
        html += `<div class="drawer-section"><div class="drawer-section-label">MITRE ATLAS Techniques</div><div class="drawer-mitre-row">`;
        allTechs.forEach(tech => { html += `<span class="mitre-technique">${tech}</span>`; });
        html += `</div></div>`;
      }
    }

    const mits = MITIGATIONS_DATA.filter(m => m.ts.includes(id) || m.ts.includes('All'));
    if (mits.length) {
      html += `<hr class="drawer-divider">`;
      html += `<div class="drawer-section"><div class="drawer-section-label">Applicable Mitigations (${mits.length})</div>`;
      mits.forEach(m => { html += `<div class="drawer-mit-item">${m.m}</div>`; });
      html += `</div>`;
    }

  } else if (type === 'mitigation') {
    const m = MITIGATIONS_DATA[id];
    if (!m) return;
    badge.textContent = 'Mitigation';
    title.textContent = m.m;

    html += `<div class="drawer-section"><div class="drawer-section-label">Example Implementation</div><p>${m.ex}</p></div>`;
    html += `<hr class="drawer-divider">`;
    html += `<div class="drawer-section"><div class="drawer-section-label">Trust Boundary</div><p>${m.tb}</p></div>`;

    if (!m.ts.includes('All')) {
      html += `<hr class="drawer-divider">`;
      html += `<div class="drawer-section"><div class="drawer-section-label">Applies to Threats</div><div class="drawer-mitre-row">`;
      m.ts.forEach(tid => {
        const t2 = THREATS_DATA.find(x => x.id === tid);
        if (t2) html += `<span class="mitre-technique">${t2.id}: ${t2.name}</span>`;
      });
      html += `</div></div>`;
    }
  }

  body.innerHTML = html;
  drawer.classList.add('open');
  if (window.innerWidth <= 640) backdrop.classList.add('open');
  document.addEventListener('keydown', drawerEscHandler);
}

function closeDrawer() {
  document.getElementById('detail-drawer').classList.remove('open');
  document.getElementById('drawer-backdrop').classList.remove('open');
  document.removeEventListener('keydown', drawerEscHandler);
}

function drawerEscHandler(e) {
  if (e.key === 'Escape') closeDrawer();
}
```

### Step 4: Wire drawer to threat selector buttons
In `initAttack()` (around line 1338), find the line that sets `btn.innerHTML`:
```javascript
btn.innerHTML = `<span class="ts-id">${id}</span><span class="ts-name">${td ? td.name : chain.name.replace(id+': ','')}</span>`;
```
Replace it with:
```javascript
btn.innerHTML = `<span class="ts-id">${id}</span><span class="ts-name">${td ? td.name : chain.name.replace(id+': ','')}</span><span class="ts-info-icon" title="View full details" onclick="event.stopPropagation();openDrawer('threat','${id}')" style="margin-left:auto;font-size:0.75rem;color:var(--text-faint);cursor:pointer;padding:2px 4px;border-radius:4px;" onmouseenter="this.style.color='var(--accent)'" onmouseleave="this.style.color='var(--text-faint)'">ⓘ</span>`;
```

### Step 5: Wire drawer to mitigation text cells
In `renderMit()` (around line 1487), find the first `<td>` in the main row HTML (around line 1514):
```javascript
<td style="font-weight:600;color:var(--text-primary);">${m.m}</td>
```
Replace it with:
```javascript
<td style="font-weight:600;color:var(--text-primary);"><span class="drawer-trigger" style="cursor:pointer;" onclick="event.stopPropagation();openDrawer('mitigation',${MITIGATIONS_DATA.indexOf(m)})" title="View full details">${m.m}</span></td>
```

Note: `MITIGATIONS_DATA.indexOf(m)` works because `m` is a direct reference to the array element (not a copy). This gives the stable index into `MITIGATIONS_DATA` needed by `openDrawer`.

### Step 6: Commit
```bash
git add index.html
git commit -m "feat: add right-side detail drawer for threats and mitigations"
```

## Report
Write your full report to: `.superpowers/sdd/task-4-report.md`

Report must include:
- Status: DONE | DONE_WITH_CONCERNS | NEEDS_CONTEXT | BLOCKED
- Commits made (short hash)
- Summary of what was changed
- Any concerns or observations
