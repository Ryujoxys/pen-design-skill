# CLI Workflow

## Source of truth

The official Skill ships with `@pen.dev/cli`:

```bash
pen version
npm view @pen.dev/cli version
pen --help
sed -n '1,240p' "$(npm root -g)/@pen.dev/cli/SKILL.md"
```

If the bundled file is unavailable, use the official latest-published fallback:
[pen.dev CLI Skill](https://unpkg.com/@pen.dev/cli@latest/SKILL.md). Do not
replace this Skill from an unofficial repository.

Upgrade with `npm install -g @pen.dev/cli`, then re-read the bundled Skill and
help output. Do this once per session or after a mismatch, not before every call.

## Authentication

```bash
pen status
pen login
```

`PEN_CLI_KEY` can authenticate CI/CD. The selected agent may use its local login
or `PEN_AGENT_API_KEY`; provider-specific environment variables may also apply.
Never expose stored sessions or API keys.

## Generate

Use agent mode for natural-language creation or broad visual changes. Pass the
user's request directly and do not add invented requirements that conflict with
their direction.

```bash
pen --out design.pen \
  --prompt "<user request>" \
  --agent codex \
  --export design.png \
  --export-scale 2
```

Useful current options:

- `--agent claude|codex|gemini`
- `--model <id>` and `--effort <level>`
- `--prompt-file <path>` for repeatable image or text attachments
- `--repo <path>` to set the agent working directory
- `--tasks <json>` for batch work
- `--usage <path>` for usage output
- `--enable-preview` and `--preview-output <path>` for previews
- `--export-type png|jpeg|webp|pdf`

Use `pen --list-models --agent codex` rather than hardcoding a model list.

## Iterate

```bash
pen --in design.pen \
  --out design-v2.pen \
  --prompt "<requested change>" \
  --agent codex \
  --export design-v2.png \
  --export-scale 2
```

Keep output files in the user's workspace. Allow at least 10 minutes for the
command even when typical work finishes sooner. Inspect and show the exported
image after generation.

Prefer a new output path for iteration. Use the same input and output path only
when the user explicitly wants an in-place update and the original is recoverable.

## Interactive mode

Use app mode for live editing:

```bash
pen interactive --app desktop
```

Use headless mode when no desktop editor is required:

```bash
pen interactive --in input.pen --out output.pen
```

Start with `get_app_state`, use `execute` for reads and writes, call `save()` in
headless mode, then `exit()`.

The interactive tool surface can differ from the desktop MCP wrapper. Read
`pen interactive --help`, then call `get_app_state` with all current required
flags. The returned schema overrides examples in help output; use full function
names such as `Update`, not a legacy alias such as `U`.

In app mode, operate on the document open in Pen and verify its path before a
write. Do not rely on `--in` to switch an already connected desktop app. In
headless mode, `--in` selects the source and `--out` is required; call `save()`
only after inspection and validation.

For read-only work, do not invoke mutation functions or `save()`. For writes,
capture the target tree before and after, query `c.problems`, and call
`TakeScreenshot` on the edited root. Use a PTY-backed interactive session when
available instead of building a shell heredoc from untrusted document text.
