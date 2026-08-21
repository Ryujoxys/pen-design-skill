# MCP Workflow

## Current surface

The current baseline exposes `get_app_state`, `get_guidelines`, `execute`, and
`browser`. The schema returned by `get_app_state` overrides this document when
they differ.

Always start with:

```text
get_app_state({
  include_schema: true,
  include_canvas_design: true,
  include_scripts_and_shaders: false
})
```

Use scripts/shaders instructions only when the task requires them.

Before writing to a named target, follow
[Document safety](document-safety.md). The current wrapper requires `filePath`,
but a required parameter is not proof that the app routed to the intended open
document.

## Inspect

Pass the target `.pen` path as `filePath` to MCP `execute` calls.

```javascript
Get((n, c) => { c.skipChildren(); Print(n.id, n.name, n.type) })
Print(Get("screenId", { depth: 3, resolveVariables: true }))
Get(n => n.reusable && Print(n.id, n.name, n.type))
Print(GetVariables())
```

Do not request the entire document without a visitor. Keep reads targeted and
use `c.skipChildren()` for a top-level inventory. Search with a visitor when an
ID is unknown rather than guessing from a screenshot or display name.

## Edit

Use only functions present in the current schema. Common functions include:

- `Insert(parentId, nodeData)`
- `Update(path, updateData)`
- `Copy(path, parentId, overrides)`
- `Replace(path, nodeData)`
- `Move(path, parentId, index?)`
- `Delete(path)`
- `Generate(nodeId, "ai|svg|stock", prompt)`
- `SetVariables(definitions, replace?)`

Persist IDs across `execute` calls by assigning without `let` or `const` when
the runtime supports persistent globals. Insert and move children under their
actual frame/group parent.

Each call has a fresh local scope. A failed call reverts its modifications and
created globals. If `execute` returns an `editId`, repair the failed snippet with
that `editId` and `edits`; do not resend new input. Read warnings and fix them in
the next call.

Use human-readable `name` on every inserted node. Keep snippets focused on one
coherent section, use loops or helpers for repeated structure, and omit comments
from the actual input.

## Validate

```javascript
Get("screenId", (n, c) =>
  c.problems && Print(n.name, "|", c.parentCtx?.node.name, "|", c.problems),
  { depth: 5 }
)
TakeScreenshot(["screenId"])
```

`TakeScreenshot` is the current canvas screenshot mechanism. A missing legacy
`get_screenshot` tool does not mean screenshots are unavailable.

`c.problems` currently reports `partially clipped` or `fully clipped`; it is not
a complete overlap, spacing, or accessibility audit. Always inspect the rendered
screenshot as well.

## Export

```javascript
Export(["screenId"], "png", "./exports")
Export(["screenId"], "pdf", "./screen.pdf")
Export(["screenId"], "html-tailwind", "./screen.html")
Export(["screenId"], "html-css", "./screen.html")
```

Use screenshots for inspection and exports for handoff. Read
[Export and handoff](export-handoff.md) for multi-node or persistent exports.

## Browser

Use `browser` only for the integrated Pen browser: loading a URL, returning a
targeted element or screenshot, or importing a page/element to the canvas. Load
the page before requesting its DOM or screenshot. Prefer a CSS query or selected
element over a full-page DOM return.
