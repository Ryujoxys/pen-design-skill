# MCP Workflow

## Current surface

The current baseline exposes `get_app_state`, `execute`, and `browser`, with
additional tools such as `get_style` or `read_skill` depending on the connected
CLI/editor surface. Discover the available tools at runtime; the schema returned
by `get_app_state` overrides this document when they differ.

`get_style` returns ready-made visual style archetypes with configurable fonts,
colors, and imagery. Use one when the user has no brand or style direction,
rather than inventing a look: `get_style({ name: "..." })`. When the style takes
params, the tool returns the options to choose from, so call it again with all of
them filled in. The archetype list ships with the runtime; read it there instead
of relying on any list in this repository.

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
- `Generate(type, …)` — `type` comes first; see below
- `SetVariables(definitions, replace?)`

Persist IDs across `execute` calls by assigning without `let` or `const` when
the runtime supports persistent globals. Insert and move children under their
actual frame/group parent.

### Generate

`type` is always the first argument, and there is no `image` node type: an image
is applied as a `fill` on a node that already exists.

```ts
Generate("ai", nodeId, prompt)                    // new image from a prompt
Generate("stock", nodeId, query)                  // 1-3 keyword Unsplash query
Generate("svg", nodeId, prompt)                   // vector artwork in a frame
Generate("vectorize-image", nodeId, imageUrl)     // transform an existing image
Generate("remove-background", imageUrl)           // returns the new asset url
Generate("replace-background", imageUrl, prompt)  // returns the new asset url
```

The last two target no node because they return a url for you to apply, so the
source url follows `type` directly. Leave unused arguments out rather than
passing `undefined`.

Every type is asynchronous: the call only writes the fill or returns the url, and
the asset itself lands after that `execute` call has returned. Check for arrival
with a cheap read in a later call — `placeholder` on the frame for the frame
types, the `fill` url for the fill types — never with an immediate screenshot,
which will not show it. Never re-issue `Generate` for pending work and never
draw the result by hand; a result that never arrived is the one case where
calling it again is correct.

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

A `browser` node is a live page on the canvas that the user can scroll, click,
and sign in on:

```javascript
Insert(parentId, { type: "browser", name: "Vendor pricing", url, width, height })
```

The `browser` tool takes the node id as `nodeId`: `load-page` navigates,
`return-screenshot` and `return-element` read the page back, `cdp` acts inside
it, and `screenshot-to-canvas` copies a region into the document. `get_app_state`
reports existing browser nodes and the user's selection, so reuse a page the user
already opened (`target: "selection"`) instead of opening another. This tool is
desktop-only; elsewhere browse with whatever tools you have and build the board
the same way.

Canvas artifacts for board work are frames, text, tables, `icon` nodes, and
`note` nodes — a sticky note carrying `content`, `width`, and `height`.
