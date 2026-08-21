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

Validated with the current npm release:

| Component | Version |
| --- | --- |
| `@pen.dev/cli` | `0.3.3` |
| Skill format | Agent Skills (`SKILL.md`) |

The skill checks `pen version`, the npm registry version, CLI help, and the live
Pencil schema at runtime. Those sources take priority if a newer pen.dev release
changes the available commands or node APIs.

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
from [`references/`](references/) only when relevant.

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
