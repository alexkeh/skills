---
name: app-composition-workflows
description: End-to-end workflows for composing complete APEX applications from requirements to APEXlang artifacts.
---

# Reference Package — Application Composition Workflows

**Parent Entry:** `references/domains/README.md` (domain), `SKILL.md` (router)

## Purpose
- Transform functional requirements into complete APEXlang applications.
- Document composition plan, rich UI patterns, and page inventory.
- Generate APEXlang artifacts with compact "Generation Plan".

## When to Trigger
- User provides business requirements with database schema/model.
- Combine with form-crud and reporting-and-dashboards for page generation.

## Required Inputs
- `requirements`: functional requirements (Markdown).
- `model/schema`: authoritative database schema or API contract.
- `target_app_path`: app directory or location context.
- `workspace_name`: APEX workspace for validation/import.
- `db_connection_name`: database connection alias.

## Authoritative Policies
- `references/policies/memory-bank/00-guard/ai.guard.md`
- `references/policies/governance/00-governance.md`
- `references/policies/memory-bank/30-pages/apex.page.md`

## Operational References
- `references/workflows/apexlang/workflow-create-app-from-fr-and-model.md`
- `references/workflows/apexlang/application-spec.template.md`
- `templates/page-examples/`

## Guardrails
- Complete `application-spec.template.md` before generating .apx.
- Write `.apexlang/app-ux-contract.json` (machine-readable).
- Include "Generation Plan" before each non-trivial component.
- Emit `Generation Plan` in response for human review.

## Outputs & Workflow
1. Read requirements and extract page inventory, workflows, business rules.
2. Complete `application-spec.template.md` with composition plan.
3. Write `.apexlang/app-ux-contract.json`.
4. Generate .apx artifacts with Generation Plan.
5. Validate with `references/ops/runtime-gates/02-direct-sqlcl-validate-gate.md`.

## References
- `references/workflows/apexlang/workflow-create-app-from-fr-and-model.md`
- `templates/page-examples/`

Use this package for complete app generation from requirements.
