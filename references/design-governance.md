# Design Governance

Use this reference for long-lived design files, multi-screen flows, responsive
families, reusable libraries, or engineering handoff. Do not impose this
structure on a disposable sketch.

## Authority and inventory

Before creating primitives, inventory:

```javascript
Print(GetVariables())
Get(n => n.reusable && Print(n.id, n.name, n.type, n.context))
Get((n, c) => {
  c.skipChildren()
  Print(n.id, n.name, n.type, n.metadata)
})
```

Prefer the live file, imported libraries, and the repository's actual design
system over packaged defaults. Inspect an unfamiliar reusable component deeply
enough to understand its named descendants, slots, theme behavior, and valid
overrides before creating a `ref`.

When no suitable component exists and the pattern will recur, create a clearly
named reusable component rather than duplicating primitives. Do not fork a
component merely because its name differs from the user's wording.

## Names, context, and metadata

- Follow the file's naming convention when one exists. Otherwise use concise,
  semantic role names rather than `Frame 12`, visual colors, or coordinates.
- Give layout wrappers a role-bearing name when they are necessary.
- Use `context` for behavior or intent that cannot be inferred visually: data
  source, validation timing, permissions, analytics, accessibility role,
  interaction, or conditional visibility.
- Use `metadata` for grounded machine-readable state or workflow information.
  Its current schema requires a `type` string.
- Do not duplicate visible style values in annotations and do not invent product
  behavior that the requirements do not support.

## File organization

For a substantial product file, make source-of-truth and exploratory work easy
to distinguish. Useful logical regions include approved screens, current work,
responsive variants, UX states, components, explorations, and archive. Implement
them with real parent-child groups or frames, not loose layers positioned nearby.

A cover or operating-manual frame can help a shared, long-lived file record
owner, status, version, scope, and links. Add it only when the artifact benefits
from that governance; do not add ceremony to small tasks.

For multi-screen flows, names should expose area, flow, step, state, and
breakpoint in a convention the team can scan. Keep node IDs opaque and stable;
identity should not depend on display names alone.

## Responsive families

Use separate sibling screen frames when layouts change materially across
breakpoints. Use a single fluid frame when the same hierarchy can adapt through
layout sizing alone. Share components and variables across both approaches.

When translating multiple artboards, compare rather than merely scale:

- hierarchy and layout direction
- column count and content order
- visibility and navigation pattern
- type scale, spacing, and gutters
- image crop and density
- interaction changes caused by device constraints

Do not infer CSS breakpoint values mechanically from artboard widths. Read the
target repository's breakpoint configuration and generate mobile-first behavior
unless the project establishes another posture. Consider container queries for
components that adapt to their parent rather than the viewport.

## Visual choices

When the request is genuinely ambiguous and the choice is visual, place two or
three labeled alternatives in a dedicated exploration group. Keep each option
fully editable, verify each screenshot, and point the user to frame names and
canvas locations. Once a direction is selected, apply the chosen design before
removing rejected alternatives; verify references and hierarchy before deleting.

Skip this process for small edits, locked brand systems, or explicit user
direction.

## Handoff contract

For production work, record or report:

- `.pen` path and canonical frame IDs
- screen-to-route or screen-to-flow mapping
- reusable components and relevant overrides
- token authority and theme assumptions
- included states and known exclusions
- screenshot/export evidence
- whether the canvas is only modified in memory or confirmed saved on disk

Do not claim a design is implementation-ready when it covers only tokens but not
the required screen composition.
