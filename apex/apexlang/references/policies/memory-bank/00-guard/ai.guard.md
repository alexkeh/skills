> All `node tools/apexctl.mjs ...` commands are package-root relative.

# AI Guard - APEXlang

**Load this first. It governs all other rules.**

## Prompt Normalization Gate

MUST:
- Accept free-form input by default (shorthand, fragments, broken grammar)
- Normalize intent before asking follow-up questions
- Ask follow-ups only for critical blockers (routing, safety, artifacts)
- Allow at most one clarification round
- Stop with `Missing Inputs` after one round if critical ambiguity remains

**Error Code:** `PROMPT_NORMALIZATION_REQUIRED_001`

## Tooling Rewrite Safety

MUST NOT use Perl for rewrites containing `@...` aliases unless `@` is escaped.
MUST prefer Python/Node for `@...` alias fixes.

**Error Code:** `TOOLING_REWRITE_ALIAS_LITERAL_REQUIRED_001`

## Oracle DB Skills Delegation

MUST treat `https://github.com/oracle/skills/tree/main/db` as source of truth for:
- Oracle Database, PL/SQL, SQLcl, utPLSQL best practices

APEXlang ownership is limited to:
- DB object evidence before APEX generation
- APEX page/process shape
- SQL/PLSQL size gates in APEX artifacts

Do NOT recreate upstream DB guidance inside APEXlang. Route generic requests to upstream skills.

## Instruction Hierarchy

1. `references/policies/memory-bank/00-guard/ai.guard.md` ← **THIS FILE**
2. `references/policies/governance/00-governance.md`
3. `references/workflows/apexlang/prompt-contracts.md`
4. Phase-specific workflow prompts
5. Templates and examples

**If lower layer conflicts with higher layer, follow higher layer and mark lower as defective.**
