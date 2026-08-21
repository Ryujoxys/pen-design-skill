# Document Safety

## Bind the target

Pencil can keep document state in the app, while an MCP wrapper or interactive
CLI supplies a `filePath`. A successful tool response is not by itself proof
that the requested file was addressed.

Before the first write to a named target:

1. Normalize the requested target and the active path reported by
   `get_app_state`.
2. If they match, bind the session to that target.
3. If they differ, run a targeted top-level probe against both paths where the
   current wrapper permits it:

```javascript
Get((n, c) => {
  c.skipChildren()
  Print(n.id, "|", n.name, "|", c.bounds.x, c.bounds.y, c.bounds.width, c.bounds.height)
})
```

4. If the target probe unexpectedly matches the active document, fail closed;
   the target may be unopened or silently routed to the active canvas.
5. Rebind after a document switch or an unexplained response. Keep the last
   verified top-level probe as a lightweight anchor.

An explicit user-provided path authorizes work on that path. If the target is
outside the workspace and was inferred rather than named, confirm it before
mutating.

## One writer per document

Do not let multiple agents or CLI sessions write the same open document at the
same time. Parallelize research or code inspection, not canvas mutation. When a
handoff occurs, re-read the target and changed roots before continuing.

## Read-only inspection

For an inspection-only request:

- Use only `Get`, `GetVariables`, `Print`, `TakeScreenshot`, or export functions.
- Do not call mutation functions or `save()`.
- Search by bounded subtree, name predicate, type predicate, or `reusable`; an ID
  is not required when the schema-supported visitor can identify the node.
- Return the matched IDs and names so the user can distinguish similar nodes.

## Safe writes

- Read the target immediately before changing it; do not update a node inferred
  only from an old screenshot or name.
- Use meaningful `name` values on every inserted node and child.
- Keep a call scoped to one coherent section. Current `execute` reverts its
  modifications and created globals when the snippet fails.
- When failure returns `editId`, patch the failed snippet using `editId` plus
  `edits`. Do not submit the whole operation again as new input.
- Read every response and fix warnings in the next call.
- Read back the affected subtree after high-risk `Replace`, `Move`, `Delete`,
  bulk update, or variable work.

Each `execute` call has a fresh local scope. Assign without `let` or `const` only
when an ID must persist to a later call. Prefer explicit IDs returned by the
previous response when persistence is unnecessary.

When constructing snippets programmatically, serialize document-derived IDs,
paths, names, and content as JSON string literals. Never interpolate them into
JavaScript syntax positions. In shell heredocs, use a quoted delimiter and
reject values that can terminate the heredoc.

## Memory versus disk

Distinguish these outcomes:

- `execute` success proves the open canvas changed.
- `save()` in headless interactive mode proves the requested CLI output was
  written when the command completes successfully.
- A live app canvas may still require the editor's save action before the `.pen`
  file on disk changes.
- `Export` success must be followed by a filesystem existence and size check.

Never report "saved" or "exported" based only on an in-memory edit or a success
message. Verify the actual destination when persistence matters.

## Git conflicts

Never resolve a `.pen` conflict by inserting conflict markers or text-merging
its bytes.

1. Preserve base, ours, and theirs as separate temporary `.pen` files from Git's
   index stages.
2. Inspect each version through headless `pen interactive` or an isolated editor
   session.
3. Choose the structurally sound version as the base.
4. Reapply the other side's intended changes with schema-valid `execute`
   operations.
5. Save to a fresh result, inspect the final tree, run `c.problems`, screenshot
   changed roots, then replace the conflicted path only after verification.

Do not run two app-connected conflict inspections concurrently; document routing
and shared app state make the result ambiguous.
