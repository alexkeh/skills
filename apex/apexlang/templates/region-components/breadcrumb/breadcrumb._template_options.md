# Breadcrumb Template Options

Use the listed `static_id` as the exact value to pass in the region's `templateOptions`.
Example: `templateOptions: [show-breadcrumbs]`
`#DEFAULT#` remains a standalone entry when used. Do not concatenate it with another token, and do not substitute the emitted CSS class string for the documented accepted value.

## Title Bar (`title-bar`)
Preset: `t-BreadcrumbRegion--useBreadcrumbTitle`

- Show Breadcrumbs | `static_id=show-breadcrumbs` | `css=t-BreadcrumbRegion--showBreadcrumb` | `group=--`
- Use Compact Style | `static_id=use-compact-style` | `css=t-BreadcrumbRegion--compactTitle` | `group=--`
- Alternative | `static_id=alternative` | `css=t-BreadcrumbRegion--headingFontAlt` | `group=Heading Font`
- Use Current Breadcrumb Entry | `static_id=use-current-breadcrumb-entry` | `css=t-BreadcrumbRegion--useBreadcrumbTitle` | `group=Region Title`
- Use Region Title | `static_id=use-region-title` | `css=t-BreadcrumbRegion--useRegionTitle` | `group=Region Title`
