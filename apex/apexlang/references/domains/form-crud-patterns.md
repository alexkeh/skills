---
name: form-crud-patterns
description: Form-based CRUD workflows with validation, context handling, modal/drawer interactions, master-detail workbenches.
---

# Reference Package — Form CRUD Patterns

**Parent Entry:** `references/domains/README.md` (domain), `SKILL.md` (router)

## Purpose
- Build form CRUD workflows: create, read, update, delete.
- Handle validation, context items, modal/drawer interactions.
- Support master-detail workbenches and parent-child relationships.

## When to Trigger
- User requests form pages, modal editing, CRUD operations.
- Combine with business-logic for validations and dynamic actions.

## Required Inputs
- `operation`: apply (default) | remove.
- `target_pages`: parent page ID, child page ID(s).
- `context_items`: parent ID item, child context items.
- `source`: table/view name or SQL query.
- `validation_rules`: required fields, format constraints, uniqueness.

## Authoritative Policies
- `references/policies/memory-bank/00-guard/ai.guard.md`
- `references/policies/memory-bank/40-components/apex.items.md`
- `references/policies/memory-bank/20-data/apex.logic.md`
- `references/policies/governance/00-governance.md`

## Operational References
- `templates/items/` - Item templates
- `templates/buttons/` - Button templates
- `templates/page-examples/form-page/` - Complete form examples
- `references/domains/business-logic/` - Validations and DA

## Guardrails
- Modal forms use `slot: contentBody`; regular pages use `slot: body`.
- Hidden context items must have `source: previous` and `queryOnly: true`.
- Required items must include `help.helpText` under 60 chars.
- Save buttons need `databaseAction: insert|update|delete`.
- Cancel buttons use `action: cancelDialog`.

## Outputs & Workflow
1. Generate form structure: parent page (IR) + child page (modal form).
2. Add context items for parent-child linkage.
3. Apply validation rules per requirements.
4. Emit save/cancel buttons with correct behavior.
5. Validate with `references/ops/runtime-gates/02-direct-sqlcl-validate-gate.md`.

## References
- `templates/items/`
- `templates/buttons/`
- `templates/page-examples/form-page/`

Use this package for form CRUD generation and modal/drawer workflows.
