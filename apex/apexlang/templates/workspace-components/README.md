# Workspace Components

**Parent Entry:** `references/domains/workspace-components.md`

---

## Purpose
- Configure workspace-level resources like credentials, REST data sources, and generative AI services.
- Manage environment provisioning and deployment strategies across development, testing, and production.
- Coordinate with database and connection management for secure workspace access.

## Template Families
- `credentials/` - Database and API credentials for workspace connections
- `rest-data-source-servers/` - REST data source server configurations
- `generative-ai-services/` - Generative AI service configurations

## Authoritative Policies
- `references/policies/memory-bank/00-guard/ai.guard.md`
- `references/policies/governance/00-governance.md`
- `assets/rules-mapping.json`
- `references/policies/memory-bank/10-global/apex.global.md`
- `references/policies/memory-bank/20-data/apex.logic.md`

## Operational References
- `references/domains/workspace-components.md`
- `templates/workspace-components/workspace-components.registry.json`
- `templates/workspace-components/credentials/`
- `templates/workspace-components/rest-data-source-servers/`
- `templates/workspace-components/generative-ai-services/`

## Required Inputs
- Workspace scope (current workspace or target workspace)
- Configuration type (credentials, REST, AI services)
- Connection details and authentication credentials
- Environment target (dev, test, prod)

## Guardrails
- Credentials must use secure storage and valid connection formats.
- REST data sources must validate endpoint connectivity.
- AI service configuration must specify valid service type and credentials.
- Workspace provisioning must respect environment-specific configurations.

## Agent Pipeline
1. Invoke `references/ops/sqlcl-agents/00-connection-gate.md` for DB context.
2. Run apexdev internal generate -> review -> fix loop.
3. For import-ready runs, execute `references/ops/runtime-gates/02-direct-sqlcl-validate-gate.md`.
4. After runtime gate pass, call `references/ops/runtime-gates/01-direct-sqlcl-import.md`.

## Outputs
- Updated workspace component artifacts.
- Deployment manifests and environment configurations.
- Compact run evidence in temp-runtime logs.

## Examples
- "Configure workspace credentials for production database connection."
- "Set up REST data source for external API integration."
- "Configure generative AI services with OCI credentials."
- "Create deployment manifest for moving from dev to prod."

---

## Sources
- Oracle APEX Documentation: Workspace Management
- APEXlang API Documentation
- `templates/workspace-components/workspace-components.registry.json`
