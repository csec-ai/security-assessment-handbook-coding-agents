# Theme Redesign — Design Spec
Date: 2026-07-15

## Context

The portal (`index.html`) is a self-contained SPA threat model viewer. The current design is functional but reads as an internal tool — navy gradient header, electric blue accents, system fonts, tight card padding. The goal is to elevate it to a professional deliverable aesthetic (consulting-grade, executive-presentable) while keeping technical depth accessible via progressive disclosure. The "INTERNAL USE" badge is removed. Mobile compatibility is a requirement.

## Design Direction

**Default view (B — Executive):** clean, spacious, authoritative. A stakeholder opening the page sees a polished product, not an internal dashboard.

**Expanded view (C — Auditor/Client):** clicking into rows or titles reveals full technical depth — MITRE mappings, attack steps, implementation examples — without cluttering the default view.

---

## 1. Color & Visual Language

| Token | Light | Dark |
|---|---|---|
| `--bg-page` | `#f8f9fa` | `#0d0f12` |
| `--bg-panel` | `#ffffff` | `#1a1d23` |
| `--bg-subtle` | `#f1f3f5` | `#22272e` |
| `--border` | `#e5e7eb` | `#2d3748` |
| `--border-mid` | `#d1d5db` | `#3d4a5c` |
| `--text-primary` | `#0d0f12` | `#f1f5f9` |
| `--text-secondary` | `#374151` | `#cbd5e0` |
| `--text-muted` | `#6b7280` | `#94a3b8` |
| `--text-faint` | `#9ca3af` | `#64748b` |
| `--accent` | `#0f4c81` | `#3b82f6` |
| `--accent-bg` | `#eff6ff` | `#1e3a5f` |

Header: `linear-gradient(135deg, #0d0f12 0%, #1a1d23 100%)` — near-black, no blue cast.

Accent color: `#0f4c81` (deep authoritative blue) in light mode; `#3b82f6` in dark mode.

Removed: "INTERNAL USE" badge. Removed: existing bright blue gradient header.

---

## 2. Typography

Fonts loaded via single Google Fonts `<link>` (no build step):
- **Inter** — all UI text, body, labels
- **JetBrains Mono** — threat IDs, MITRE codes, monospace values

Scale:
| Element | Size | Weight |
|---|---|---|
| Page title (h1) | `1.75rem` | 700 |
| Subtitle | `0.9rem` | 400, muted |
| Tab labels | `0.875rem` | 500, uppercase, `0.08em` spacing |
| Section headings | `1rem` | 600 |
| Body / table text | `0.875rem` | 400 |
| Monospace (IDs, codes) | `0.8rem` | — |
| Small badges | `0.72rem` | — |

---

## 3. Layout & Cards

**Tab bar:**
- Sits below header with `padding: 0 40px`
- Tabs are pill-shaped buttons
- Active tab: filled `#0f4c81` background, white text
- Inactive tab: transparent background, `--text-secondary` text

**Cards / panels:**
- `border-radius: 12px`
- `padding: 24px`
- `box-shadow: 0 1px 3px rgba(0,0,0,0.06), 0 4px 16px rgba(0,0,0,0.04)`
- `border: 1px solid var(--border)` (down from 1.5px)

**Stat chips (mitigations tab):**
- Promoted to stat cards in a grid row
- Large number (`1.5rem`, weight 700), label below (`0.75rem`, muted)

---

## 4. Progressive Disclosure

### Inline Accordions (chevron expand)

- Each threat/mitigation row has a `›` chevron on the right
- Click chevron → row expands downward, 200ms ease transition
- Expanded panel: `--bg-subtle` background, slightly indented, shows MITRE tactics/techniques, implementation examples, step breakdowns
- One row expanded at a time per section (auto-collapse previous)

### Side Drawer (title click)

- Click the row *title* → full-height drawer slides in from the right
- Desktop: `360px` wide, overlaps content, no backdrop
- Mobile: full-width, with `rgba(0,0,0,0.3)` backdrop overlay
- Drawer content: complete technical profile — threat description, attack path summary, all mitigations, MITRE mappings, full step list
- Close via: `×` button, `Escape` key, or clicking outside (mobile backdrop)
- Animation: `300ms ease` `transform: translateX()`

### Distinction

| Interaction | Result |
|---|---|
| Click chevron `›` | Inline accordion expands below the row |
| Click row title | Side drawer opens with full deep-dive |

---

## 5. Removed Elements

- "INTERNAL USE" badge (header)
- Bright blue gradient header

---

## 6. Preserved Elements

- All data arrays (`TRUST_BOUNDARIES`, `THREATS_DATA`, `ATTACK_CHAINS`, `MITIGATIONS_DATA`, `COMPONENTS`, `DIAG`) — untouched
- Three-tab structure and all panel content
- Dark/light theme toggle
- SVG diagrams and interactive filtering
- Mobile layout (drawer goes full-width, tabs scroll horizontally)

---

## 7. Verification

1. Open `index.html` in a browser
2. Confirm header is near-black (no blue gradient), no "INTERNAL USE" badge
3. Confirm Inter font loads, typography scale is visually correct
4. Confirm tabs are pill-shaped, active tab filled
5. Click a chevron → accordion expands inline, collapses previous
6. Click a row title → drawer slides in from right with full detail
7. Press Escape → drawer closes
8. Resize to mobile width → drawer goes full-width with backdrop
9. Toggle dark mode → all tokens flip correctly
10. SVG diagrams and threat filters still function
