---
name: workspace-components
description: Manage workspace-level resources including credentials, REST data sources, and generative AI services.
---

# Reference Package — Workspace Components

**Parent Entry:** `references/domains/README.md` (domain), `SKILL.md` (router)

## Purpose
- Configure workspace-level resources (credentials, REST data sources, AI services).
- Manage environment provisioning across dev/test/prod.
- Coordinate with database and connection management.

## When to Trigger
- User requests workspace configuration, credentials, or API integration.
- Combine with application-settings for deployment manifests.

## Required Inputs
- `component_type`: credentials | rest-data-source | generative-ai.
- `connection_details`: host, port, credentials.
- `environment`: dev | test | prod.
- `auth_type`: username-password | API-key | OAuth.

## Authoritative Policies
- `references/policies/memory-bank/00-guard/ai.guard.md`
- `references/policies/memory-bank/10-global/apex.global.md`
- `references/policies/memory-bank/20-data/apex.logic.md`

## Operational References
- `templates/workspace-components/README.md`
- `templates/workspace-components/credentials/`
- `templates/workspace-components/rest-data-source-servers/`

## Guardrails
- Credentials must use secure storage format.
- REST endpoints must validate connectivity.
- AI service config must specify valid service type and credentials.
- Workspace provisioning must respect environment-specific configs.

## Outputs & Workflow
1. Generate workspace component artifacts.
2. Create deployment manifests and environment configurations.
3. Validate with `references/ops/runtime-gates/02-direct-sqlcl-validate-gate.md`.

## References
- `templates/workspace-components/README.md`

Use this package for workspace-level resource configuration.
