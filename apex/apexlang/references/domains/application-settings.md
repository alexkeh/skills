---
name: application-settings
description: Configure APEX application-level settings including globalization, session management, security, and app-level metadata.
---

# Reference Package — Application Settings

**Parent Entry:** `references/domains/README.md` (domain), `SKILL.md` (router)

## Purpose
- Configure globalization support (translation methods, format masks).
- Manage session settings (timeout, max length, state protection).
- Set app-level security (checksum salt, auth scheme).
- Coordinate with shared components (app items, processes, build options).

## When to Trigger
- User requests app configuration, globalization, or session settings.
- Combine with shared-components for app items and processes.

## Required Inputs
- `setting_type`: globalization | session | security | runtime.
- `configuration_values`: timeouts, checksums, format masks.
- `auth_scheme`: authentication scheme alias.
- `translation_method`: text messages | direct literals.

## Authoritative Policies
- `references/policies/memory-bank/00-guard/ai.guard.md`
- `references/policies/memory-bank/10-global/apex.global.md`
- `references/policies/memory-bank/20-data/apex.logic.md`

## Operational References
- `templates/shared-components/authentications/`
- `templates/shared-components/authorizations/`
- `references/domains/shared-components/`

## Guardrails
- Globalization must align with translation method (text messages vs direct literals).
- Session timeouts must respect APEX constraints (min/max).
- Checksum salt must be 64-char hex string.
- Compatibility mode must match target APEX version.

## Outputs & Workflow
1. Generate/update `application.apx` with settings sections.
2. Update shared components (app items, processes, build options).
3. Validate with `references/ops/runtime-gates/02-direct-sqlcl-validate-gate.md`.

## References
- `templates/shared-components/README.md`

Use this package for application-level configuration.
