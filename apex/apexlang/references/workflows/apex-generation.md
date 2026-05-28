---
name: apex-developer
description: Orchestrate the APEXlang internal generate, review, and fix loop for application and page generation.
---
> All `node tools/apexctl.mjs ...` commands are package-root relative: run them from the packaged skill root, or invoke that script by explicit path.


# Constitutional Master Playbook — APEX Developer Reference Package

## Purpose
- Single authoritative orchestration for APEXlang generation using the internal generate -> review -> fix loop.
- Enforce minimal rule loading, no fabrication, template-first drafting, and direct SQLcl runtime gates when a live roundtrip is required.

## Authoritative Policies
- `references/policies/memory-bank/00-guard/ai.guard.md`
- `references/policies/governance/00-governance.md`
- `assets/rules-mapping.json`
- `references/policies/memory-bank/systemPatterns.md`

## Operational References
- `references/workflows/apexlang/prompt-contracts.md`
- `references/workflows/apex-generation/templates.md`
- `references/workflows/apex-generation/registry.md`
- `references/workflows/apex-generation/workflow-manifests/apex-generation-master-workflow.md`
- `references/workflows/apex-generation/workflow-manifests/apex-generation-agent-suite.md`
- `assets/apex-generation/components.registry.json`
- `references/policies/governance/prompt-normalization.md`

## Execution Agents
- `references/ops/sqlcl-agents/00-connection-gate.md`
- `references/workflows/apex-generation/agents/20-agent-draft.md`
- `references/workflows/apex-generation/agents/30-agent-critique.md`
- `references/workflows/apex-generation/agents/40-agent-revision.md`
- `references/ops/runtime-gates/02-direct-sqlcl-validate-gate.md`
- `references/ops/runtime-gates/01-direct-sqlcl-import.md`

## Loading Guidance
- Load this package from `SKILL.md` when the request needs broad orchestration, mixed-domain generation, or the full internal generate -> review -> fix loop.
- In the root-only APEXlang model, this package is not invoked as a direct skill.

## Core workflow
1. Normalize free-form input directly according to `references/policies/governance/prompt-normalization.md` and ask only one simple-English clarification round for critical blockers.
2. For packaged-build refresh work, classify the change as broad DSL drift or narrow component metadata delta before changing templates, examples, or packaged docs.
3. Resolve prerequisite metadata and saved-connection discovery first for APEX artifact work: inspect offline schema dictionaries, scan saved SQLcl connections, use discovered aliases as candidates, and require the user to specify the live `db_connection_name` plus corresponding APEX workspace name before live work.
4. If `db_mode = online`, continue with the resolved `db_connection_name`, APEX workspace name, and metadata/runtime gates as needed.
5. Select page and component patterns only from `templates/**`, using `page-examples/**` first for page-scoped work.
6. Use the resolved target app only for integration facts, never as a pattern or DSL source.
7. Generate APEXlang artifacts from canonical templates and rules inside the transient temp workspace.
8. Generate or refresh the target build contract pack and keep only the compact validation contexts in scope: user spec, table metadata, target build contract pack, scaffold manifest, and the last live validation problem list.
9. Run the compiler-truth audit against the generated temp app and record `compiler-truth-report.json`; missing or failing compiler-truth evidence blocks completion.
10. Review `problems.json` plus VSCode Problems diagnostics for rule compliance, missing inputs, and required fixes; local lint is syntax hygiene only.
11. Apply deterministic fixes only for reported problems, rerun compiler-truth audit as needed, and hand off to `references/ops/runtime-gates/` when a live roundtrip is required and `db_mode = online`.

## Runtime hand-off
- Live roundtrips use direct SQLcl commands only.
- Use `node tools/apexctl.mjs runtime validate --app-path <absolute_app_path> --db-connection-name <db_connection_name> --apex-root <resolved_build_root>` as the public validate-only handoff. The wrapper owns preflight/roundtrip details and must emit `validation-report.json`, `validation-transcript.log`, `problems.json`, and `component-contracts/<build>.json`.
- `apex validate -input` remains mandatory inside the live validate-only gate.
- Runtime validation requires both compiler-driven truth and VSCode Problems evidence for generated or revised `.apx` artifacts. Record `VALIDATION_DUAL_SOURCE_REQUIRED_001` and block completion when either evidence source is missing, unavailable, or failing.
- Runtime hand-off defaults to check-only; in user-facing responses, call that `Check APEXlang code`. After the live APEXlang check passes, offer GUI/clickable choices for `Check APEXlang code` or `Check and import APEXlang code`.
- `apex import -input` runs only after an explicit post-check GUI import choice, and then it must use the same authenticated SQLcl user session as the live check.
- Completion remains blocked until runtime status proves eligibility.
