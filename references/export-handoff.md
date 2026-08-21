# Export and Handoff

Use screenshots for inspection and exports for persistent deliverables. Current
desktop MCP exposes both through `execute`:

```javascript
TakeScreenshot([screenId])
Export([screenId], "png", outputPath, { scale: 2 })
Export([screenId], "pdf", pdfPath)
Export([screenId], "html-tailwind", htmlPath, {
  includeLayerNames: true,
  includeLayerIds: true
})
```

Confirm every name and option against the live schema.

## Select and order nodes

- Export requested screen frames, not every top-level frame. Shared components,
  covers, notes, and explorations often sit beside deliverables.
- Resolve IDs from the live document and print the final ordered list before a
  multi-node export.
- Preserve intentional document or slide order explicitly; do not trust opaque
  node IDs or incidental filesystem ordering.
- Screenshot the smallest meaningful target before export so a structurally
  valid but visually wrong frame does not become the deliverable.

## Formats

- Use PNG for lossless UI review and JPEG/WEBP when file size matters.
- Use PDF for multi-page review, decks, or print-oriented handoff. Verify page
  count and ordering after export.
- Use `html-tailwind` or `html-css` only when the user wants markup. Set
  `includeLayerIds: true` when downstream tooling must map exported elements
  back to Pencil nodes; it defaults to false.
- Treat HTML as a structural starting point, not production framework code.
  Check relative image and font references before handing it off.

## Paths and persistence

Export path normalization can differ between the app host, WSL, containers, and
the caller's filesystem. Prefer an absolute path native to the host running Pen.
After every export:

1. Check that the expected file or directory exists.
2. Check that outputs are non-empty and were modified by the current operation.
3. Open or render representative outputs; for a sequence, verify at least the
   first and last item plus any high-risk state.
4. For PDF, validate the document and page count.
5. Report the resolved output paths, not only the requested strings.

Do not trust a success message when the destination cannot be found. On WSL,
stage to a Windows-native path and collect from `/mnt/<drive>` when the Pen app
runs on Windows.

## Engineering handoff

Include the `.pen` path, canonical frame IDs, exports, breakpoint/state coverage,
component and token sources, and any grounded `context` or `metadata`. If the
artifact remains only in the editor's memory, say so explicitly and do not imply
that Git or another process can read the latest canvas state.
