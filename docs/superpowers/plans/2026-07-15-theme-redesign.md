# Theme Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Elevate `index.html` to a professional, executive-grade design (neutral premium palette, Inter font, pill tabs, spacious cards) with a right-side drawer and inline accordions for progressive disclosure, while removing the "INTERNAL USE" badge.

**Architecture:** All changes are confined to a single self-contained file (`index.html`). CSS is in the `<style>` block (lines 7–354). HTML structure is lines 356–~650. JavaScript is in the `<script>` block (~line 650 onward). Data arrays (`THREATS_DATA`, `ATTACK_CHAINS`, `MITIGATIONS_DATA`, etc.) are untouched. The drawer is a new `<div id="detail-drawer">` injected into `<body>` before the closing `</body>` tag, with open/close logic added to the existing JS section.

**Tech Stack:** Vanilla HTML/CSS/JS, no build step. Two Google Fonts loaded via `<link>`: Inter and JetBrains Mono.

## Global Constraints

- Single file: all changes to `index.html` only
- No external JS dependencies
- All data arrays (`THREATS_DATA`, `ATTACK_CHAINS`, `MITIGATIONS_DATA`, `COMPONENTS`, `DIAG`, `TRUST_BOUNDARIES`) are read-only — do not modify
- Existing SVG diagrams and interactive filtering must remain functional
- Dark/light theme toggle must remain functional
- Mobile layout must work (drawer full-width on mobile, tabs horizontally scrollable)
- Remove "INTERNAL USE" badge entirely
- Font loading: single `<link>` tag, Google Fonts only

---

## Task 1: CSS Tokens + Fonts

**Files:**
- Modify: `index.html` — `:root` and `html.dark` blocks (lines 11–51), `<head>` (lines 3–6)

**What this does:** Replace all CSS custom property values with the new neutral premium palette and add the Google Fonts `<link>` tag. No visual structure changes yet — just the token layer and font loading.

- [ ] **Step 1: Add Google Fonts link tag**

Insert immediately after line 5 (`<meta name="viewport" ...>`):

```html
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;600;700&display=swap" rel="stylesheet">
```

- [ ] **Step 2: Replace `:root` token block**

Replace the entire `:root { ... }` block (lines 11–31) with:

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

- [ ] **Step 3: Replace `html.dark` token block**

Replace the entire `html.dark { ... }` block (lines 32–52) with:

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

- [ ] **Step 4: Update body font-family**

Replace line 55:
```css
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
```
with:
```css
      font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
```

- [ ] **Step 5: Update all monospace font references**

In the `<style>` block, replace all occurrences of `'SF Mono','Fira Code',monospace` with:
```css
'JetBrains Mono', 'SF Mono', 'Fira Code', monospace
```
(This affects `.tf-btn .tf-id`, `.threat-sel-btn .ts-id`, `.filter-chip .fc-id`, and `.mitre-tactic`.)

- [ ] **Step 6: Open in browser and verify**

Open `index.html` in a browser. Confirm:
- Page background is off-white (`#f8f9fa`) in light mode
- Body text uses Inter (check DevTools → Computed → font-family)
- Dark mode toggle still works and background shifts to near-black

- [ ] **Step 7: Commit**

```bash
git add index.html
git commit -m "refactor: replace CSS tokens with neutral premium palette, add Inter + JetBrains Mono fonts"
```

---

## Task 2: Header Redesign + Remove Badge

**Files:**
- Modify: `index.html` — `.header` CSS rule (~line 70), header HTML (~lines 358–369)

**What this does:** Replace the blue gradient header with a near-black gradient, remove the "INTERNAL USE" badge from HTML, and update the h1 size.

- [ ] **Step 1: Replace `.header` CSS rule**

Replace the `.header` rule (~line 70):
```css
    .header {
      background: linear-gradient(135deg, #0f2744 0%, #1a3f6f 60%, #1e4d8c 100%);
      color: white; padding: 22px 40px;
      display: flex; align-items: center; justify-content: space-between;
      box-shadow: 0 2px 12px rgba(0,0,0,0.25);
    }
```
with:
```css
    .header {
      background: linear-gradient(135deg, #0d0f12 0%, #1a1d23 100%);
      color: white; padding: 22px 40px;
      display: flex; align-items: center; justify-content: space-between;
      box-shadow: 0 2px 12px rgba(0,0,0,0.35);
    }
```

- [ ] **Step 2: Update h1 font size**

Replace line 76:
```css
    .header-left h1 { font-size: 1.5rem; font-weight: 700; letter-spacing: -0.01em; }
```
with:
```css
    .header-left h1 { font-size: 1.75rem; font-weight: 700; letter-spacing: -0.02em; }
```

- [ ] **Step 3: Remove "INTERNAL USE" badge from HTML**

Find (~line 367):
```html
    <div class="header-badge">INTERNAL USE</div>
```
Delete that line entirely (the `.header-badge` CSS rule can remain — it does no harm unused, but optionally remove it too from ~line 78).

- [ ] **Step 4: Verify in browser**

Open `index.html`. Confirm:
- Header is near-black (no blue gradient)
- "INTERNAL USE" badge is gone
- Title is slightly larger
- Dark mode toggle still visible and functional

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "refactor: near-black header gradient, remove INTERNAL USE badge, larger h1"
```

---

## Task 3: Pill Tabs + Card Polish

**Files:**
- Modify: `index.html` — `.tab-nav`, `.tab-btn`, `.tab-panel` CSS rules (~lines 84–97); card/panel CSS rules (~lines 113, 117, 141, 315, 334–335, 339)

**What this does:** Convert tab underline indicator to filled pill, increase card border-radius/padding/shadow, lighten borders from 1.5px to 1px.

- [ ] **Step 1: Replace `.tab-nav` and `.tab-btn` CSS**

Replace the `.tab-nav` and `.tab-btn` rules (~lines 84–97):
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
with:
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

- [ ] **Step 2: Update `.tab-panel` border-radius**

Replace the `.tab-panel` rule (~line 96):
```css
    .tab-panel { display: none; background: var(--bg-panel); border-radius: 0 0 12px 12px; box-shadow: var(--shadow-md); padding: 32px; }
```
with:
```css
    .tab-panel { display: none; background: var(--bg-panel); border-radius: 0 0 12px 12px; box-shadow: var(--shadow-md); padding: 36px; }
```

- [ ] **Step 3: Update card border widths and radius**

In the `<style>` block, do a find-and-replace:
- Replace all `border: 1.5px solid var(--border)` with `border: 1px solid var(--border)`
- Replace all `border: 1.5px solid var(--border-mid)` with `border: 1px solid var(--border-mid)`
- Replace `border-radius: 10px` with `border-radius: 12px` (for `.diagram-wrapper`, `.info-panel`, `.filter-panel`, `.threat-filter-section`, `.stat-chip`)

- [ ] **Step 4: Update stat chips to stat cards**

Replace the `.stats-row` and `.stat-chip` rules (~lines 334–337):
```css
    .stats-row { display: flex; gap: 12px; flex-wrap: wrap; margin-bottom: 20px; }
    .stat-chip { background: var(--bg-panel); border: 1.5px solid var(--border); border-radius: 10px; padding: 12px 18px; }
    .stat-chip .sc-num { font-size: 1.5rem; font-weight: 700; color: #2b6cb0; }
    .stat-chip .sc-label { font-size: 0.72rem; color: var(--text-faint); margin-top: 2px; }
```
with:
```css
    .stats-row { display: flex; gap: 16px; flex-wrap: wrap; margin-bottom: 24px; }
    .stat-chip { background: var(--bg-panel); border: 1px solid var(--border); border-radius: 12px; padding: 20px 24px; flex: 1; min-width: 120px; }
    .stat-chip .sc-num { font-size: 2rem; font-weight: 700; color: var(--accent); line-height: 1; }
    .stat-chip .sc-label { font-size: 0.78rem; color: var(--text-muted); margin-top: 6px; font-weight: 500; }
```

- [ ] **Step 5: Update active accent references**

In the `<style>` block, replace hard-coded `#2b6cb0` accent color references with `var(--accent)`:
- `.tab-btn.active` already done above
- `.dtoggle-btn.active { background: #2b6cb0; ... }` → `background: var(--accent);`
- `.step-item.active .step-circle { background: #2b6cb0; ... }` → `background: var(--accent);`
- `.attack-nav-btn:hover:not([disabled]) { background: #2b6cb0; }` → `background: var(--accent);`
- `.mit-expand-icon.open { ... color: #2b6cb0; }` → `color: var(--accent);`
- `.data-row:hover .mit-expand-icon { color: #2b6cb0; }` → `color: var(--accent);`
- `.stat-chip .sc-num { color: #2b6cb0; }` → already done in Step 4

- [ ] **Step 6: Verify in browser**

Open `index.html`. Confirm:
- Active tab has filled blue pill background (white text)
- Inactive tabs are transparent with uppercase label
- Stat cards on the Mitigations tab are larger with prominent numbers
- Cards have slightly more padding and softer borders

- [ ] **Step 7: Commit**

```bash
git add index.html
git commit -m "refactor: pill tabs, card polish, stat card promotion, accent token"
```

---

## Task 4: Side Drawer

**Files:**
- Modify: `index.html` — `<style>` block (add drawer CSS), `</body>` (add drawer HTML), `<script>` block (add drawer JS)

**What this does:** Add a right-side detail drawer that slides in when a threat/mitigation row title is clicked. The drawer shows the full technical profile. Close via ×, Escape, or backdrop click on mobile.

- [ ] **Step 1: Add drawer CSS**

Add the following CSS block at the end of the `<style>` block (before `</style>`):

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

- [ ] **Step 2: Add drawer HTML**

Add the following HTML immediately before the closing `</body>` tag:

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

- [ ] **Step 3: Add drawer JavaScript**

Add the following functions at the end of the `<script>` block (before the closing `</script>` tag):

```javascript
function openDrawer(type, id) {
  const drawer = document.getElementById('detail-drawer');
  const backdrop = document.getElementById('drawer-backdrop');
  const badge = document.getElementById('drawer-type-badge');
  const title = document.getElementById('drawer-title');
  const body = document.getElementById('drawer-body');

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

      // MITRE tactics/techniques from all steps
      const allTactics = [...new Set(chain.steps.flatMap(s => s.tactics || []))];
      const allTechs   = [...new Set(chain.steps.flatMap(s => (s.techniques || []).map(x => x.id + ' ' + x.name)))];
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

    // Mitigations for this threat
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
        const t = THREATS_DATA.find(x => x.id === tid);
        if (t) html += `<span class="mitre-technique">${t.id}: ${t.name}</span>`;
      });
      html += `</div></div>`;
    }
  }

  body.innerHTML = html;
  drawer.classList.add('open');
  // Only show backdrop on mobile
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

- [ ] **Step 4: Wire drawer to threat selector buttons (Attack Path tab)**

In the JS that builds the threat selector buttons (search for `threat-selector` or `ts-id` in the script block), find where `.threat-sel-btn` click handlers are set. Each button currently calls a function to load the attack chain. Change the button HTML generation to add a title-click vs chevron distinction.

Find the block that creates `.threat-sel-btn` elements and add a small info icon that triggers the drawer. The simplest approach: add an `onclick` to each `.ts-name` span to call `openDrawer('threat', t.id)`.

Locate the JS line that sets `innerHTML` for `.threat-sel-btn`. It will look something like:
```javascript
btn.innerHTML = `<span class="ts-id">${t.id}</span><span class="ts-name">${t.name}</span>`;
```
Change it to:
```javascript
btn.innerHTML = `<span class="ts-id">${t.id}</span><span class="ts-name">${t.name}</span><span class="ts-info-icon" title="View full details" onclick="event.stopPropagation();openDrawer('threat','${t.id}')" style="margin-left:auto;font-size:0.75rem;color:var(--text-faint);cursor:pointer;padding:2px 4px;border-radius:4px;" onmouseenter="this.style.color='var(--accent)'" onmouseleave="this.style.color='var(--text-faint)'">ⓘ</span>`;
```

- [ ] **Step 5: Wire drawer to mitigation table rows**

In the JS that builds the mitigation table rows, find where `.data-row` click handlers are set (search for `data-row` or `mit-expand` in the script). Each row currently toggles an expand row. Add a named cell (the mitigation text cell) that opens the drawer.

Find the `<td>` for the mitigation text and wrap its content in a clickable span:
```javascript
// Find the td that renders m.m (the mitigation text)
// Change it from: `<td>${m.m}</td>`
// To:
`<td><span class="drawer-trigger" style="cursor:pointer;color:var(--text-primary);" onclick="event.stopPropagation();openDrawer('mitigation',${i})" title="View full details">${m.m}</span></td>`
```
(Where `i` is the index of the mitigation in `MITIGATIONS_DATA`.)

- [ ] **Step 6: Verify drawer in browser**

Open `index.html`:
1. Go to Tab 2 (Attack Path) — click the ⓘ icon on any threat button → drawer slides in with threat details
2. Go to Tab 3 (Mitigations) — click any mitigation text → drawer opens with example + trust boundary
3. Press Escape → drawer closes
4. Resize to mobile width (< 640px) → backdrop overlay appears
5. Confirm dark mode: drawer uses `--bg-panel` and `--border` tokens correctly

- [ ] **Step 7: Commit**

```bash
git add index.html
git commit -m "feat: add right-side detail drawer for threats and mitigations"
```

---

## Task 5: Inline Accordion Polish

**Files:**
- Modify: `index.html` — `.mit-expand-row`, `.mit-expand-content` CSS (~lines 303–312); JS that toggles expand rows

**What this does:** The existing mitigation table already has expand rows. This task polishes the expand animation and adds a chevron indicator. Also adds a subtle accordion to attack chain step details (they already expand — just improve the visual treatment).

- [ ] **Step 1: Update expand row CSS**

Replace the `.mit-expand-row` and `.mit-expand-content` rules (~lines 303–312):
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
with:
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

- [ ] **Step 2: Verify accordion in browser**

Open Tab 3 (Mitigations), click a table row chevron:
- Expand row appears with a smooth visual
- Chevron rotates
- Color uses `--accent` not hard-coded blue

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "refactor: polish accordion expand row styles, use accent token"
```

---

## Task 6: Final Verification

- [ ] **Step 1: Full visual pass — light mode**

Open `index.html` in a browser:
1. Header: near-black gradient, no "INTERNAL USE" badge, Inter font, larger title
2. Tab bar: pill tabs, active tab filled with `--accent` color
3. Tab 1 (Architecture): diagram loads, filter buttons work, info panel shows on component click
4. Tab 2 (Attack Path): threat selector loads, clicking steps highlights diagram, ⓘ opens drawer
5. Tab 3 (Mitigations): stat cards are larger/more prominent, filter chips work, row chevrons expand, mitigation text opens drawer

- [ ] **Step 2: Full visual pass — dark mode**

Toggle dark mode. Confirm:
- Header stays near-black (visually similar in both modes)
- Backgrounds use the dark tokens (near-black panels)
- Drawer renders correctly in dark mode
- Accordion expand rows render correctly

- [ ] **Step 3: Mobile check**

Resize browser window to 375px width:
- Tabs scroll horizontally (no layout break)
- Drawer opens full-width with backdrop
- Stat cards wrap to 2-per-row or stack cleanly
- Diagrams are horizontally scrollable

- [ ] **Step 4: Final commit**

```bash
git add index.html
git commit -m "feat: professional theme redesign complete — neutral premium palette, Inter font, pill tabs, stat cards, detail drawer, accordion polish"
```
