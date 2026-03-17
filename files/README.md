# SecureOps Portal — UX Evaluation Submission

**Role:** UI/UX Engineer / Product Designer (ServiceNow & Enterprise Portal Experience)  
**Exercise:** Enterprise Workflow UX Pack  
**Candidate:** Aditya V.  
**Time spent:** ~2.5 hours  
**Tools used:** Figma (Now Design System), Claude AI (ideation, content, Figma automation)

---

## What's in this submission

| # | Screen | Description |
|---|--------|-------------|
| 01 | [Assumptions & Scope](screens/01-assumptions-and-scope.jpg) | Persona, platform context, data domains, RBAC, explicit scope decisions |
| 02 | [Information Architecture](screens/02-information-architecture.jpg) | 7 content zones, reading order, interaction model, action types |
| 03 | [Dashboard Wireframe](screens/03-dashboard-wireframe.jpg) | Full 1440px mid-fi screen — nav, status bar, filter bar, findings table, sidebar widgets, drill-down panel |
| 04 | [Annotations & Edge Cases](screens/04-annotations-edge-cases.jpg) | 4 design rationale annotations including the all-clear edge case |
| 05 | [Implementation Notes](screens/05-implementation-notes.jpg) | 6 engineering-ready SNDS component mappings, tokens, async/loading, RBAC, accessibility |

---

## Screens

### 01 — Assumptions & Scope
![Assumptions & Scope](screens/01-assumptions-and-scope.jpg)

### 02 — Information Architecture
![Information Architecture](screens/02-information-architecture.jpg)

### 03 — Dashboard Wireframe (Mid-Fidelity)
![Dashboard Wireframe](screens/03-dashboard-wireframe.jpg)

### 04 — Annotations & Edge Cases
![Annotations & Edge Cases](screens/04-annotations-edge-cases.jpg)

### 05 — Implementation Notes
![Implementation Notes](screens/05-implementation-notes.jpg)

---

## Design Decisions

### Persona
**Primary:** Compliance Manager at a mid-large enterprise. Reviews security posture weekly, escalates critical findings to CISO. Needs clarity over density.  
**Secondary:** IT Security Analyst who triages incidents daily and needs fast row-level actions.

### Platform
ServiceNow Employee Center / Service Portal · 1440px desktop · Now Design System (SNDS) · WCAG 2.1 AA

### Scope decision
One strong, complete screen with full depth > multiple shallow screens.

Delivered: primary dashboard + Finding Detail drill-down + IA diagram + assumptions + annotations + implementation notes.

Intentionally excluded: mobile, dark mode, full export flow, bulk actions.

---

## Key Design Rationale

**Status Summary Bar (above the fold)**  
Six metrics always visible at page load: Critical/High/Medium/Low counts + Compliance Score + SLA Breach Rate. Compliance Managers start their day with a posture scan — this answers "how bad is it today?" without any click or scroll.

**Unassigned = Red**  
"Unassigned" in the Owner column renders in danger red — an intentional deviation from the gray default. Unowned findings are the #1 cause of SLA breaches in enterprise workflows. Passive visual urgency beats a separate notification.

**Side Panel over New Page for drill-down**  
Finding detail uses a slide-in Now Panel, preserving filter state so the user can act on one finding while keeping full list context visible. Page navigation would require rebuilding filter context on return.

**Edge Case — All Clear state**  
If Critical + High both hit 0: cells show "0" with green fills and "✓ Within target" sub-label. The bar always renders complete — its absence would itself be ambiguous to a manager who expects it.

---

## Implementation Notes

| Component | SNDS Mapping |
|-----------|-------------|
| Findings table | `sn-list` (List v3) — row click triggers panel, no custom JS |
| Finding Detail panel | `sn-panel` — slide-in, `?finding=SEC-004` URL state for shareability |
| Status badges | SNDS semantic tokens: `$color-danger-1`, `$color-warning-1`, `$color-caution-1`, `$color-positive-1` |
| Loading states | `sn-skeleton` for status bar + table; sidebar widgets lazy-load staggered |
| RBAC | ACL on GlideRecord query (not UI hiding); export is server-side role-gated via Scripted REST |
| Accessibility | `<th scope="col">`, `aria-label` on all actions, 3px focus ring, Escape closes panel with focus returned to originating row (WCAG 2.1 SC 2.4.3) |
