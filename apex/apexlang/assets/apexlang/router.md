# Router Layer for APEXlang Skills

## Purpose

The router determines **when to load each domain file** based on user intent, minimizing token usage by only loading what's needed.

## How It Works

### 1. Intent Detection

When a user provides a request, the router:
1. Normalizes the text to extract intent keywords
2. Matches keywords against the `domains-catalog.json` and `router.json`
3. Loads only the **specific skill** with the highest priority match

### 2. Load Strategy

**Least Context First** - Load the smallest possible skill file:
- Specific pattern → Specific skill file (e.g., `form-crud-patterns.md`)
- Generic pattern → Generic skill file (e.g., `page-components/README.md`)

### 3. Priority System

Routers are processed in priority order:
1. **intent-based** (priority 10) - Exact keyword matches
2. **fallback-readme** (priority 1) - Wildcard matches

## Examples

| User Request | Router Match | File Loaded |
|--------------|--------------|-------------|
| "create a form with validation" | `form-crud-patterns` | `form-crud-patterns.md` (57 lines) |
| "add a button that submits" | `page-components/buttons` | `page-components/buttons.md` (66 lines) |
| "fix a runtime error" | `debugging` | `debugging/README.md` (75 lines) |
| "build a dashboard with charts" | `reporting-and-dashboards` | `reporting-and-dashboards.md` (55 lines) |
| "create master-detail pages" | `app-composition-workflows` | `app-composition-workflows.md` (53 lines) |

## Token Efficiency Gains

**Before Router** (load everything):
```
page-components/README.md        74 lines
business-logic/README.md        0 lines (doesn't exist)
shared-components/README.md     0 lines (doesn't exist)
debugging/README.md            0 lines (doesn't exist)
... (15 domain files)          3157 total lines
```

**After Router** (load only what's needed):
```
form-crud-patterns.md           57 lines  (for form requests)
reporting-and-dashboards.md     55 lines  (for dashboard requests)
application-settings.md         50 lines  (for settings requests)
workspace-components.md         49 lines  (for workspace requests)
app-composition-workflows.md    53 lines  (for workflow requests)
page-components/README.md       74 lines  (for generic page requests)
```

**Average per request: ~55 lines vs 3157 lines = 98% reduction**

## Integration Points

The router integrates with:

1. **SKILL.md** - Loads `domains-catalog.json` as the primary router
2. **Runtime** - `apexctl.mjs` uses router to select skills for generation
3. **Agent workflows** - Agents consult router before loading domain guidance

## Extensibility

Add new domains to `domains-catalog.json`:
```json
{
  "id": "new-domain",
  "summary": "Description",
  "keywords": ["keyword1", "keyword2"],
  "load_path": "references/domains/new-domain.md"
}
```

The router automatically picks up new entries without code changes.
