# APEXlang Prompt Contracts

Canonical contract for APEXlang agent prompting. Use this file as the shared rule source for the router and the Draft, Critique, and Revision agents.

## Purpose

- Reduce prompt drift by centralizing high-value behavior rules.
- Prefer compact, named contracts over repeated narrative guidance.
- Make prompt-governed behavior easier to enforce in critique, validators, and tests.
- Force a rules-first workflow: use the posted rules and workflows before any inference, and treat inference as a bounded last resort.

## Instruction Hierarchy

1. `references/policies/memory-bank/00-guard/ai.guard.md`
2. `references/policies/governance/00-governance.md`
3. This file
4. Phase-specific workflow prompts
5. Templates and examples

## Required Tagged Sections

Use these exact top-level tags when applicable:
- `<authority_rules>`, `<task_scope>`, `<allowed_sources>`
- `<exact_match_policy>`, `<compiler_truth_contract>`
- `<generation_plan_contract>`, `<output_contract>`, `<stop_conditions>`

## Required Intermediate Artifacts

### Compiler Truth Evidence
When a draft introduces non-exact-match structural edit:
- exact `query-valid-props` command
- checked component or parent scope
- conclusion
- emitted decision

### Generation Plan
For non-trivial generation before emitting APEXlang:
- target artifact scope
- template family/variant
- region, item, button inventory
- source mode decisions
- navigation decisions
- compiler-truth references

Response order: Compiler Truth Evidence → Generation Plan → APEXlang

### Application Spec
Before complete app generation:
- source evidence and conflict status
- full page inventory
- application composition plan
- rich UI pattern plan
- LOVs, validation, modal/report-to-form, tests
- missing inputs and blockers
- `.apexlang/app-ux-contract.json` with required fields

## Workflow Precedence

1. Apply guardrails and governance
2. Apply workflow and template family
3. Use compiler-backed truth and contracts
4. Stop with `Missing Inputs` when rules don't answer
5. Use bounded inference only for low-risk details

Do NOT:
- guess when rules/templates don't answer
- skip to "best judgment" because template is "close enough"
- invent target pages/items when workflow can't prove them
- infer missing structure from validator success

## Rule IDs

### DESTINATION_WORKSPACE_NAME_REQUIRED_001
Generate `deployments/default.json` only when workspace name is explicitly specified.

### EXACT_MATCH_TEMPLATE_REQUIRED_001
Reuse templates directly only when family, variant, context, and mode match exactly.

### COMPILER_TRUTH_EVIDENCE_REQUIRED_001
Non-exact-match edits must provide compiler-truth evidence.

### RULES_FIRST_WORKFLOW_REQUIRED_001
Follow rules/workflow first; don't guess before exhausting sources.

### HUMAN_INTERVENTION_REQUIRED_001
Stop for Missing Inputs when rules don't answer high-impact decisions.

### APPLICATION_SPEC_REQUIRED_001
Complete app generation from FR/model needs implementation-ready spec first.

### GENERATION_PLAN_REQUIRED_001
Non-trivial generation needs Generation Plan before APEXlang.

### GENERATION_PLAN_DRIFT_001
Artifact must not drift from frozen Generation Plan.

### DSL_MULTILINE_STRUCTURE_REQUIRED_001
Object-valued properties must use `name: {` with nested properties.

### DECLARATIVE_BUTTON_TARGET_REQUIRED
`redirectThisApp` buttons must use declarative target object syntax.

### TEMPLATE_OPTIONS_MULTILINE_REQUIRED_001
Multi-value `templateOptions` must use bracket format.

### TEMPLATE_OPTIONS_DEFAULT_ATOMIC_001
`#DEFAULT#` must be standalone, not concatenated.

### CLASSIC_REPORT_DEFAULT_TEMPLATE_REQUIRED_001
Classic Report must use canonical `appearance` and `componentAppearance` blocks.

### CLASSIC_REPORT_COMPONENT_APPEARANCE_REQUIRED_001
Classic Report must emit `componentAppearance.template: @/standard`.

### PAGE_ITEM_LAYOUT_LABEL_COL_SPAN_LEGACY_001
Page item layout must use legacy label col span when required.

## Stop Conditions

Stop with `Missing Inputs` when:
- Critical ambiguity remains after one clarification round
- Rule, workflow, template, or compiler-truth doesn't answer
- High-impact decision lacks explicit answer
- No deterministic recipe for reported issue
