# Pen Design Skill

[English](README.md) | 简体中文

一套用于创建、编辑、检查、验证、导出和实现
[pen.dev](https://pen.dev) `.pen` 设计稿的 Agent Skill。

它覆盖 pen.dev 的两类主要工作流：

- 使用 `pen` CLI Agent 模式完成提示词驱动的设计生成、迭代、批量任务和直接导出。
- 使用 `pen interactive` 或 Pencil MCP 完成精确节点编辑、组件复用、层级检查、截图和定向导出。

这套 Skill 特别关注 Frame、Group 与内容之间真实的父子层级关系，同时提供文档安全、组件与变量复用、视觉验证、响应式设计、无障碍设计和研发交付方面的指导。

## 兼容性

已使用当前 npm 正式版本完成验证：

| 组件 | 版本 |
| --- | --- |
| `@pen.dev/cli` | `0.3.3` |
| Skill 格式 | Agent Skills（`SKILL.md`） |

每次工作时，Skill 会检查 `pen version`、npm Registry 最新版本、CLI 帮助以及 Pencil 实时 Schema。如果后续 pen.dev 版本调整了命令或节点 API，应以这些实时信息为准。

## 安装

将仓库克隆到 Agent 支持的 Skill 目录：

```bash
git clone https://github.com/Ryujoxys/pen-design-skill.git \
  ~/.agents/skills/pen-design
```

Codex 用户也可以安装到 `~/.codex/skills/pen-design`：

```bash
git clone https://github.com/Ryujoxys/pen-design-skill.git \
  ~/.codex/skills/pen-design
```

另外需要单独安装并登录 pen.dev CLI：

```bash
npm install -g @pen.dev/cli
pen login
pen status
```

安装完成后，重启或重新加载 Agent，使其发现新 Skill。

## 使用

可以直接调用 `$pen-design`，也可以让 Agent 创建或编辑 `.pen` 设计稿。
[`SKILL.md`](SKILL.md) 是 Skill 的入口文件，具体工作流会按需从
[`references/`](references/) 加载。

编辑指定设计稿之前，Skill 会验证当前活动文档，并始终以实时 Schema 作为能力依据。它不会解析、搜索或手工修改 `.pen` 文件内容。

## 主要能力

- 根据自然语言需求创建和迭代界面、网页、移动端、数据看板、设计系统与演示文稿。
- 优先复用已有组件、变量、图标、图片和项目设计规范。
- 确保 Frame、Group 和内部节点拥有真实且可维护的父子层级。
- 检查裁切、溢出、布局坍塌、错误层级和视觉偏差。
- 覆盖交互状态、异常状态、响应式布局和基础无障碍要求。
- 导出 PNG、JPEG、WEBP、PDF 或 HTML，并验证实际输出文件。
- 将 `.pen` 设计映射到现有前端项目的组件、Token 和实现约定。
- 安全处理目标文档确认、并发写入、持久化状态和 `.pen` Git 冲突。

## 工作原则

- 以用户要求、现有设计资产和项目规范为优先依据，不覆盖已有设计语言。
- 不直接解析或手工编辑 `.pen` 文件，只通过 pen.dev CLI 或 Pencil MCP 操作。
- 每个 Frame、Group 和内容节点都应建立正确的父子关系，不能通过画布位置模拟分组。
- 修改后重新读取节点、检查问题并截图确认，不能只依据工具返回成功判断结果。
- 截图用于视觉检查，导出文件用于正式交付；保存或导出后需要验证磁盘结果。

## 目录结构

```text
.
|-- SKILL.md
|-- agents/openai.yaml
`-- references/
```

`references/` 包含以下专项说明：

- CLI 工作流与身份认证。
- Pencil MCP 和 `execute` API 操作方式。
- 目标文档绑定、写入安全与 Git 冲突处理。
- 设计质量、设计治理、状态和无障碍规范。
- Web、移动端、管理后台、大屏、电商和演示文稿等平台模式。
- 设计转代码、导出与研发交付流程。

## 协议

[MIT](LICENSE)
