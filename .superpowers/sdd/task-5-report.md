# Task 5 Report: Inline Accordion Polish

## Status
DONE

## Commits Made
- `90dee7a` — refactor: polish accordion expand row styles, use accent token

## Summary of Changes
Four CSS property changes applied to the `/* Expandable mitigation rows */` block in `index.html` (lines 313–321):

1. `.mit-expand-row td` — replaced hardcoded `#bee3f8` border colour with `var(--border-mid)` token.
2. `.mit-expand-content` — padding increased from `14px 18px 16px` to `16px 20px 18px`.
3. `.mit-expand-label` — `margin-bottom` increased from `6px` to `8px`.
4. `.mit-expand-icon` — transition updated from `transform 0.2s` to `transform 0.2s ease`; added `margin-left: 6px`.

The `.mit-expand-icon.open` and `.data-row:hover .mit-expand-icon` rules already used `var(--accent)` (applied by a previous task) so no further changes were needed there.

## Concerns / Observations
None. The existing expand/collapse behaviour is untouched — only CSS presentation was modified. The `--border-mid` token must be defined elsewhere in the stylesheet (confirmed present in earlier task work) for the border change to take effect correctly.
