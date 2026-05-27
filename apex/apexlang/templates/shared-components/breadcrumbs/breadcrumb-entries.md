---
templateId: breadcrumb-entries
componentType: breadcrumb-entries
version: 1.0
---

# Purpose

The breadcrumb entries component is embedded inside a breadcrumb component.  It contains and entry per page that is referenced.  It is possible that an entry has a parent entry, but not required.

---

# Generation Rules (MANDATORY)

1. Create the name and displayName
2. All updates should be made to the breadcrumbs.apx file found under an application's /shared-components directory

---

# Variable Contract

## Required Variables

- entry
  - Type: text
  - The entry name; all lowercase, no spaces
  - Used whe referencing a parent entry

- entry.name
    - Friendly name for the breadcrumb

- entry.pageNumber
    - Page Number that the breadcrumb entry is current for

- entry.execution.sequence
    - Sequence of breadcrumb entry; used to order them

---

# Conditional Rendering Rules

- entry.appearance.parentEntry
    - @entry of parent breadcrumb entry
    - must always start with @
    - entire `appearance` block only required when there is a parent present
    - Common mistake: do not place `parentEntry` in `execution`; that is invalid.

- entry.link.target
    - Link that when clicked, the user will follow
    - Typically uses the stadard APEX nominclature for links: f?p=&APP_ID.:1:&APP_SESSION.::&DEBUG.:::
    - if not needed, the entire `link` block does not need to be rendered

---

# Output Template - No Parent
```
   entry {{entry}} (
        name: {{entry.Name}}
        pageNumber: {{entry.pageNumber}}
        execution {
            sequence: {{entry.execution.sequence}}
        }
        link {
            target: {{entry.link.target}}
        }
    )
```

---

# Output Template - With Parent
```
   entry {{entry}} (
        name: {{entry.Name}}
        pageNumber: {{entry.pageNumber}}
        appearance {
            parentEntry: @{{entry.appearance.parentEntry}}
        }
        execution {
            sequence: {{entry.execution.sequence}}
        }
        link {
            target: {{entry.link.target}}
        }
    )
```
