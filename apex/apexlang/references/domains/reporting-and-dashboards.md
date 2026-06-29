---
name: reporting-and-dashboards
description: Interactive reports, dashboards, charts, maps, and data visualization.
---

# Reference Package — Reporting and Dashboards

**Parent Entry:** `references/domains/README.md` (domain), `SKILL.md` (router)

## Purpose
- Build interactive reports (IR) with columns, filtering, sorting, export.
- Create dashboards with KPI cards, charts, and summary regions.
- Add maps with location data and area visualizations.
- Configure calendar regions for events and appointments.

## When to Trigger
- User requests reports, dashboards, charts, maps, or calendars.
- Combine with form-crud for drill-down from reports to forms.

## Required Inputs
- `region_type`: interactiveReport | chart | map | calendar.
- `source`: table/view name, SQL query, or REST source.
- `chart_type`: bar | line | pie | donut (for charts).
- `layout_slot`: body (regular pages), contentBody (modals).
- `map_layers`: markers, areas, heatmaps.

## Authoritative Policies
- `references/policies/memory-bank/00-guard/ai.guard.md`
- `references/policies/memory-bank/40-components/apex.regions.md`
- `references/policies/memory-bank/20-data/apex.sql.md`
- `references/policies/governance/00-governance.md`

## Operational References
- `templates/region-components/` - Region templates
- `templates/page-examples/` - Page examples

## Guardrails
- IR must define `column` blocks for every SQL projection.
- Hidden columns need `type: hidden` with `heading.heading`.
- Charts need `chart.type` (bar, line, pie, donut) with `series` blocks.
- Maps need `mapLayers` configuration for markers or areas.
- All regions need `help.helpText` under 60 chars.

## Outputs & Workflow
1. Select region type and source.
2. Define columns for IR (hidden, plainText with dataType).
3. Configure chart with `chart.type` and `series` blocks.
4. Apply layout slot and appearance.
5. Validate with `references/ops/runtime-gates/02-direct-sqlcl-validate-gate.md`.

## References
- `templates/region-components/`
- `templates/page-examples/`

Use this package for report, dashboard, chart, map, and calendar generation.
