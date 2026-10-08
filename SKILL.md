---
name: pen-design
description: >
  Create, edit, inspect, validate, export, and implement .pen visual designs with
  the pen.dev CLI or Pencil MCP. Use for UI mockups, app and web screens,
  dashboards, design systems, responsive variants, design-to-code work, or any
  task involving pen.dev, Pencil, Pen, or .pen files. Also use when the canvas
  should carry research rather than a design: competitor and vendor comparisons,
  evaluation, planning, meeting prep, and briefs, built on live browser nodes.
---

# Pen Design

Use the current pen.dev runtime as the source of truth. The CLI and Pencil MCP
share `.pen` files but serve different workflows.

## Deliverable first

Decide what the user is asking for before touching the canvas:

- A **product design** — screen, page, deck, design system — is designed.
- **Information** is whiteboarded. Signals: research, compare, evaluate, plan,
  prep, find, decide, summarize, shortlist, schedule, budget, with no mention of
  screens, apps, or websites. A request to evaluate vendors is not a request for
  a procurement tool, and a request to plan a trip is not a request for a
  travel app. Read [Official integration](references/official-integration.md)
  and use live `browser` nodes instead of designing a product.

When the deliverable is genuinely unclear, ask one question rather than guess
the expensive option.

## Choose the surface

- Use `pen` agent mode for prompt-driven generation, headless work, model
  selection, attachments, batch tasks, and direct export.
- Use `pen interactive` for deterministic headless operations or live app mode
  when MCP tools are not directly exposed.
- Use Pencil MCP for live editor state, precise node operations, component and
  variable reuse, layout diagnosis, screenshots, and targeted export.
- Combine them when useful: generate broadly with CLI, refine and verify through
  MCP.
- Never parse, grep, or hand-edit `.pen` files. Use `pen` or Pencil MCP.

Read [CLI workflow](references/cli-workflow.md) for authentication, generation,
iteration, and interactive use. Read [MCP workflow](references/mcp-workflow.md)
for the live editor tools and `execute` API.

## Refresh and bind

This skill is maintained against the current `@pen.dev/cli` release. Runtime
schema, bundled official skill, and command help remain authoritative when the
installed version changes.

Before the first Pen task in a session:

1. For CLI work, run `pen version`, `npm view @pen.dev/cli version`, and
   `pen status`. If behavior differs, inspect `pen --help` and the official
   bundled `SKILL.md`.
2. For MCP work, call `get_app_state` with `include_schema: true`,
   `include_canvas_design: true`, and `include_scripts_and_shaders: false`.
3. Treat the returned schema and tool descriptions as authoritative. Never
   assume a capability is unavailable because a legacy standalone tool name is
   missing.
4. Before writing, prove the requested target is the document being addressed.
   Do not rely on `filePath` alone when the active document differs.

Read [Document safety](references/document-safety.md) before writing to a named
file, editing outside the workspace, using concurrent agents, or resolving a
`.pen` Git conflict.

Read [Official integration](references/official-integration.md) at the start of
the first Pen task in a session. It routes to the official schema and guides
without duplicating their proprietary manuals in this repository.

## Source priority

Resolve design decisions in this order:

1. Explicit user direction, supplied references, and approved brand guidance.
2. Live `.pen` variables, reusable components, imported libraries, and assets.
3. The target repository's tokens, components, fonts, icons, and conventions.
4. Skill defaults only when the first three provide no answer.

Do not overwrite established tokens or components with a generic starter set.
Read [Design governance](references/design-governance.md) for long-lived files,
multi-screen flows, component libraries, naming, and responsive organization.
Read [Platform patterns](references/platform-patterns.md) only when the task is
an admin product, mobile app, mini program, data display, commerce surface,
design system, cross-platform family, or presentation deck.

When the user supplies no brand or style direction, load a ready-made archetype
with `get_style({ name: "..." })` instead of inventing a look. If the style takes
params, the tool returns the options to choose from; call it again with all of
them filled in.

## Required hierarchy

Frames, groups, and their contents must have real parent-child relationships.
Never simulate grouping by placing unrelated nodes next to each other.

- Insert children under the intended frame or group, or move them there.
- Use frames for layout, sizing, clipping, backgrounds, and responsive
  containers.
- Use groups for logical hierarchy without frame layout behavior.
- Verify hierarchy after copying, replacing, moving, or creating nodes.
- Prefer `fill_container` and `fit_content` in layout-driven frames. Do not set
  `x`/`y` on auto-layout children unless `layoutPosition` is `absolute`.
- Give every authored node a meaningful human-readable `name`.
- Only page/screen frames, reusable component frames, and other major container
  frames belong directly under `document`. Never place text, icons, buttons,
  cards, rows, images, or decorative shapes there.
- Re-read instead of recreating. pen.dev is collaborative and the document can
  change while you work, so a node that is missing or no longer matches what you
  expected is usually someone else's edit, not a bug.

## Schema guardrails

Apply the official `.pen` semantics rather than CSS assumptions:

- Use only schema-supported properties. Percentages, `margin`, baseline/stretch
  alignment, and CSS `calc()` values are invalid.
- `layout` and `padding` belong on frames. Auto-layout ignores child `x`/`y`;
  use `layoutPosition: "absolute"` only when deliberate.
- Text needs an explicit `fill`. Use `textGrowth: "fixed-width"` for wrapping;
  do not guess text box dimensions when layout can determine them.
- New, copied, or modified root frames use `placeholder: true` while being
  built, then clear it as soon as that frame is complete.
- Set `viewBox` on every authored SVG path and prefer layout-driven charts over
  manually positioned data marks.

## Assets

Images and artwork come from the runtime, never from hand-built paths:

- There is no `image` node type. An image is applied as a `fill` on an existing
  node, so insert the frame or rectangle first.
- Never draw logos, illustrations, or mascots yourself. Hand-built drawings look
  bad; when a design genuinely calls for one, use `Generate` with `type: "svg"`
  on a frame. For iconography use `icon` nodes, for decoration use
  frames/shapes/gradient fills, and for photography use image fills.
- `Generate` is asynchronous. The call only does the bookkeeping; the image or
  drawing lands after that `execute` call has returned, so an immediate
  screenshot will not show it. Keep working and check with a cheap read in a
  later call. Never re-issue `Generate` for pending work, never draw the result
  by hand, and never add children to a frame you generated into.
- Failures are silent, and they are the one case where calling `Generate` again
  for the same result is correct.

Read the official `generate.md` through
[Official integration](references/official-integration.md) before calling
`Generate`.

## Working loop

1. Bind the target and inspect the selected or requested subtree.
2. Inventory reusable components, variables, assets, and relevant imports.
3. For ambiguous new work, choose or present a small number of concrete visual
   directions. Skip broad exploration for a precise edit.
4. Build one coherent section at a time with correct hierarchy.
5. Read back the changed subtree, query `c.problems`, and screenshot the smallest
   meaningful completed root.
6. Fix warnings, clipping, collapsed layout, accidental overflow, and broken
   hierarchy before continuing.
7. Export only after the canvas version passes visual inspection.

Use the current schema-documented functions inside `execute`:

```javascript
Get(n => n.reusable && Print(n.id, n.name, n.type))
Print(GetVariables())
sectionId = Insert(parentId, {
  type: "frame",
  name: "Feature section",
  layout: "vertical",
  width: "fill_container",
  gap: 16,
  padding: 24
})
Get(sectionId, (n, c) =>
  c.problems && Print(n.name, "|", c.parentCtx?.node.name, "|", c.problems)
)
TakeScreenshot([sectionId])
```

Keep `execute` snippets small and omit comments from the actual input. A failed
snippet is reverted; repair it with its returned `editId` and `edits` rather
than resending new input. Fix returned warnings in the next call.

## Completeness

- Search reusable components before creating equivalents; use matching `ref`
  instances and grounded descendant overrides.
- Read `GetVariables()` before styling or defining tokens. Merge missing tokens;
  replace the full set only when explicitly intended.
- Reuse logos, icons, images, and components instead of regenerating them.
- For flows or production-ready screens, cover relevant component and screen
  states rather than drawing only the happy path.
- Preserve accessibility intent and interaction behavior in `context` or
  `metadata` when the information is grounded and not visually inferable.

Read [Design quality](references/design-quality.md) when creating or reviewing a
screen. Read [States and accessibility](references/states-accessibility.md) for
forms, flows, interactive components, or production handoff. Read
[Design to code](references/design-to-code.md) only for implementation work.

For specialized work, also read the matching official guide through the routing
table in [Official integration](references/official-integration.md).

## Export and completion

Use `TakeScreenshot` for inspection and `Export` for deliverables. Read
[Export and handoff](references/export-handoff.md) for multi-node, PDF, HTML,
deck, or cross-platform exports.

Before finishing:

- The requested target, parent-child hierarchy, and edited root were verified.
- No reported clipping or unintended overflow remains.
- Existing components, variables, assets, and project conventions were reused.
- Relevant states and accessibility requirements are represented.
- The final target was screenshotted and visually inspected.
- Requested exports were verified on disk and shown or linked to the user.
- Report canvas edits separately from confirmed disk saves or exports.

For a whiteboard deliverable the check is different: every source is a real page
the user can open, each finding sits next to the source it came from, and the
user's own decisions are handed back as open questions rather than made for them.
