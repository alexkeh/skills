---
templateId: region.classic-report.contextual-info
componentType: region
version: 1.0
imports:
  - classic-report._common.md
description: Contextual-info variant of a classic report — unified minimal shell using the @/contextual-info report template.
---

# Purpose

Document the classic-report qualifier that emits the dedicated `region-contextual-info` template variant. This is a unified lightweight variant for single-row contextual content. For column-based (horizontal) and row-based (vertical) layout specifics, see `classic-report.context-info-column-based.md` and `classic-report.contextual-info-row-based.md`.

# Generation Rules (MANDATORY)

1. Load `classic-report._common.md` first.
2. Use this variant only when the classic report is intentionally rendered through `@/contextual-info`.
3. Keep it to single-row contextual content.
4. This is the explicit exception to the shared Classic Report default `appearance.template`; do not force `@/standard` onto this variant.

# Variable Contract

## Required Variables

- `regionStaticId`
- `name`
- `source.location`
- `layout.sequence`
- `layout.slot`

## Optional Variables

- `appearance.templateOptions`
- `columns`

# Output Template – Full

```apexlang
region {{regionStaticId}} (
  name: {{name}}
  type: classicReport
  layout {
    sequence: {{layout.sequence}}
    slot: {{layout.slot}}
  }
  appearance {
    template: @/contextual-info
    templateOptions: [
      {{appearance.templateOptions}}
    ]
  }
  componentAppearance {
    template: @/standard
    templateOptions: #DEFAULT#
  }
  {{columns}}
)
```

# Conditional Rendering Rules

- Use this variant only when the selected qualifier is `contextual-info`.
- Keep the surrounding region appearance minimal while retaining the required Classic Report component template block.
- Treat this file as the documented override path when a Classic Report must not use the shared `@/standard` default template block.
- When `appearance.templateOptions` contains more than one accepted value, emit bracketed multi-line array syntax with one accepted value per line; never use inline comma-separated arrays.
- Omit `appearance.templateOptions` when no additional modifiers are required.
