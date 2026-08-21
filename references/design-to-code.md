# Design to Code

## Read the design

1. Call `get_app_state` with the current schema.
2. Read variables with `Print(GetVariables())`.
3. Read the target subtree with a bounded `Get(nodeId, {depth: ...})` query.
4. List reusable components and identify `ref` instances.
5. Screenshot the target to preserve visual context.

Also identify required screen states, interaction intent recorded in `context`
or `metadata`, and differences between responsive artboards. A single happy-path
screenshot is not a complete implementation contract.

## Resolve authority

- User-approved composition and content come from the canonical Pen frame.
- Runtime behavior, accessibility mechanics, APIs, and existing component
  contracts come from the codebase and requirements.
- Token authority is project-specific: use the code theme when it is canonical,
  or map established Pen variables when the design system is Pen-led.
- When Pen and code disagree, surface the conflict; do not silently let either
  side overwrite the other.

## Translate semantics

- Preserve frame/group and reusable-component boundaries in code.
- Map vertical/horizontal frame layout to the target layout system rather than
  reproducing absolute coordinates.
- Map design variables to semantic code tokens. Avoid arbitrary values when a
  matching token exists.
- Treat mobile and desktop artboards as responsive intent, not fixed browser
  widths.
- Reuse the repository's existing components, tokens, fonts, icons, and coding
  conventions before adding dependencies or parallel implementations.
- Map `ref` instances to existing code components and grounded overrides to
  props or variants. Do not flatten every instance into copied markup.
- Preserve reading order, focus order, loading/error behavior, and intentional
  visibility changes across breakpoints.

For Tailwind, map semantic colors, radii, typography, and spacing to the
project's existing theme. Do not assume every variable name should become a new
utility when the codebase already has an equivalent token.

## HTML export

When the user requests Pen-generated HTML rather than framework code:

```javascript
Export(["screenId"], "html-tailwind", "./screen.html")
Export(["screenId"], "html-css", "./screen.html")
```

Inspect the exported result and adapt it to the target project; do not treat an
export as production-ready without review. Use `includeLayerIds: true` when
node-to-code traceability is required.

## Verify implementation

Render the implementation at the target breakpoints and compare it with Pen
screenshots. Check hierarchy, typography, spacing, responsive behavior, assets,
interaction states, accessibility, and overflow. Preserve functional behavior
from the existing application while matching the design.

Verify high-risk states as well as the default state. When the design and live UI
must stay synchronized, record the Pen path, frame IDs, implementation route,
and comparison evidence in the project's established handoff artifact.
