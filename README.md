# Pen Design Skill

English | [简体中文](README.zh-CN.md)

An agent skill for creating, editing, inspecting, validating, exporting, and
implementing [pen.dev](https://pen.dev) `.pen` designs.

It supports both major pen.dev workflows:

- `pen` CLI agent mode for prompt-driven generation, iteration, batch work, and
  direct exports.
- `pen interactive` or Pencil MCP for precise node edits, component reuse,
  hierarchy checks, screenshots, and targeted exports.

The skill emphasizes real frame/group parent-child relationships, document
safety, reusable components and variables, visual verification, responsive
design, accessibility, and implementation handoff.

## Compatibility

Validated against the local CLI during maintenance:

| Component | Version |
| --- | --- |
| `@pen.dev/cli` | `0.3.6` |
| Skill format | Agent Skills (`SKILL.md`) |

The skill checks `pen version`, the npm registry version, CLI help, the bundled
official skill, and the live Pencil schema at runtime. Those sources take
priority if a newer pen.dev release changes commands or node APIs. See
[Official integration](references/official-integration.md) for the routing and
licensing boundary between the official docs and this repository's overlays.

## Install

Clone the repository into a skill directory supported by your agent:

```bash
git clone https://github.com/Ryujoxys/pen-design-skill.git \
  ~/.agents/skills/pen-design
```

For Codex, you can instead install it under `~/.codex/skills/pen-design`:

```bash
git clone https://github.com/Ryujoxys/pen-design-skill.git \
  ~/.codex/skills/pen-design
```

Install and authenticate the pen.dev CLI separately:

```bash
npm install -g @pen.dev/cli
pen login
pen status
```

Restart or reload your agent after installation so it discovers the skill.

## Use

Invoke the skill as `$pen-design`, or ask the agent to create or edit a `.pen`
design. The entrypoint is [`SKILL.md`](SKILL.md); detailed workflows are loaded
from [`references/`](references/) only when relevant. Official CLI guides are
read from the installed package at runtime rather than copied into this
repository.

Before editing a named design, the skill verifies the active document and uses
the live schema as the source of truth. It never parses or hand-edits `.pen`
files.

## Structure

```text
.
|-- SKILL.md
|-- agents/openai.yaml
`-- references/
```

## License

[MIT](LICENSE)
