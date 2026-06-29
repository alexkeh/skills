# APEXlang Domain References

Canonical packaged entrypoint for APEXlang domain guidance.

## Domain Index

| Domain | Purpose |
|--------|---------|
| Page Components | Regions, items, buttons, page patterns |
| Business Logic | Validations, computations, dynamic actions, processes |
| Shared Components | Lists, breadcrumbs, LOVs, authorizations, translations |
| Template Components | Template selection and configuration |
| Universal Attribute Config | Help text, server-side conditions, page sequencing |
| Debugging | Validation/import failure diagnosis and fixes |
| Application Settings | Globalization, session, security, runtime config |
| Workspace Components | Credentials, REST sources, generative AI services |
| Application Composition Workflows | End-to-end app generation from requirements |
| Form CRUD Patterns | Modal forms, master-detail, validation workflows |
| Reporting and Dashboards | Interactive reports, charts, maps, KPI cards |

## Domains

### Page Components
- `page-patterns` - Page structure and layout patterns
- `regions` - Region definitions (IR, chart, map, etc.)
- `page-items` - Page items (text fields, select lists, etc.)
- `buttons` - Button types and actions
- `screenshot-to-layout` - Screenshot-driven page design

### Business Logic
- `validations` - Item and page validations
- `computations` - Item and page computations
- `dynamic-actions` - Client-side and server-side DAs
- `processes` - Page and AJAX processes

### Shared Components
- `lists` - Navigation lists and menus
- `breadcrumbs` | `lovs` | `authorizations` | `authentications`
- `translations` | `email-templates` | `task-definitions`

### Template Components
- Template options, layout slots, inheritance

### Universal Attribute Config
- Server-side conditions, help text, page sequencing

### Debugging
- Failure-map, fix-patterns, check-commands

### Application Settings
- Globalization, session management, security, checksum

### Workspace Components
- Credentials, REST data sources, generative AI services

### Application Composition Workflows
- Complete app generation from requirements

### Form CRUD Patterns
- Modal/drawer forms, master-detail workbenches

### Reporting and Dashboards
- Interactive reports, charts, maps, calendar

## Loading Guidance
1. Load `SKILL.md` first for routing and runtime context
2. Load domain-specific package for requested scope
3. Use `templates/*/` for executable APEXlang blocks
4. Validate with `references/ops/runtime-gates/02-direct-sqlcl-validate-gate.md`

## Authoritative Policies
- `references/policies/memory-bank/00-guard/ai.guard.md`
- `references/policies/governance/00-governance.md`
- Domain-specific policies in `references/policies/memory-bank/`
