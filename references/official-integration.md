# Official Integration

This skill adds workflow, safety, and quality guidance around the official
`pen.dev` skill. The installed `@pen.dev/cli` package remains authoritative for
the `.pen` schema, MCP API, model catalog, and command behavior.

## Runtime source

Do not copy the official bundled manuals into this repository. The package is
proprietary and can change independently of this skill. Read the copy shipped
with the installed CLI when available:

```bash
CLI_ROOT="$(npm root -g)/@pen.dev/cli"
sed -n '1,240p' "$CLI_ROOT/SKILL.md"
sed -n '1,240p' "$CLI_ROOT/dist/out/skills/pen-dev/SKILL.md"
```

If the global package is absent, resolve the local package with
`node -p "require.resolve('@pen.dev/cli/package.json')"`, or use the official
published `SKILL.md` URL from [CLI workflow](cli-workflow.md).

## Read by task

The bundled `pen-dev` skill is an index. Read only the references needed for
the current task:

| Task | Official reference |
| --- | --- |
| Any canvas work | `SKILL.md`, `pen-schema.md`, `execute.md` |
| Research, comparison, evaluation, planning, meeting prep | `whiteboard.md` |
| Images and vector artwork | `generate.md` |
| Components and design systems | `guide/components.md`, `guide/design-system.md` |
| Landing pages and web apps | `guide/landing-page.md`, `guide/web-app.md` |
| Mobile screens | `guide/mobile-app.md` |
| Slides | `guide/slides.md` |
| Tables and dashboards | `guide/table.md` |
| Code handoff | `guide/code.md`; add `guide/tailwind.md` for Tailwind v4 |
| Scripts or shaders | `scripts-and-shaders.md` |

Start from the deliverable. The official skill covers two different ones: a
product design, and a whiteboard whose deliverable is information for the user.
`whiteboard.md` is required whenever the user asks you to research, compare,
evaluate, plan, prep, find, decide, summarize, shortlist, schedule, or budget
something, and it is explicitly not a licence to design an app about the topic.

For `pen interactive`, start with `read_skill()`, then read
`pen-schema.md` and `execute.md` before editing. Pencil MCP exposes a different
tool surface, so read the official files through the installed CLI path when
`read_skill` is unavailable. In either surface, load docs on demand; do not
assume every guide is already in the model context.

## Precedence

Resolve conflicts in this order:

1. The user's explicit request and supplied references.
2. The live document's variables, components, assets, and runtime schema.
3. The official bundled skill and current CLI help.
4. This repository's safety, governance, quality, accessibility, and handoff
   overlays.
5. Generic design defaults.

The official schema and returned tool descriptions win for syntax. This skill's
overlays win for when to inspect, how to preserve existing work, and what must
be verified before completion.

## Model and authentication notes

- `--model` is an explicit model ID; otherwise `--agent` selects the provider
  and its default model. Use `pen --list-models --agent <name>` at runtime.
- `PEN_CLI_KEY` authenticates pen.dev. It is separate from the model provider
  credential.
- For Claude, the CLI can use the Claude Code local login/subscription or
  `PEN_AGENT_API_KEY`/`ANTHROPIC_API_KEY`, depending on the selected agent
  configuration. Never print or commit credentials.
- `--custom` may route Claude-compatible deployments such as Bedrock, Vertex,
  or Microsoft Foundry. Follow the current CLI help for required settings.
