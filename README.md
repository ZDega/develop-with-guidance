# develop-with-guidance

A reusable skill for Codex, Claude Code, and OpenCode for learning by building. You write the code while your coding assistant explains each step, guides you through real features, and reviews your work. It works with ordinary projects and exercises, starting from a feature described in chat, a local ticket, or an accessible issue reference.

## Install in a project

Download or clone this repository. Choose the instructions for your coding assistant. Replace `/path/to/develop-with-guidance` with your checkout's actual path. If the destination skill already exists, review it before copying an update.

### Codex

From the root of the project where you want guidance, copy the entire Codex skill folder from your checkout:

```sh
mkdir -p .agents/skills
cp -R /path/to/develop-with-guidance/.agents/skills/develop-with-guidance .agents/skills/
```

The installed layout should be:

```text
your-project/
└── .agents/skills/develop-with-guidance/
    ├── SKILL.md
    └── agents/openai.yaml
```

Open Codex in that project. Codex detects skill changes automatically; restart it if the skill does not appear. This repository already contains the skill at the project installation path, so no copy is needed to try it here.

The skill is instruction-only, with no scripts or extra tool dependencies. Keep `agents/openai.yaml` alongside `SKILL.md`: its `policy.allow_implicit_invocation: false` setting makes invocation explicit. Start it with `$develop-with-guidance`. See the [official Codex skills documentation](https://learn.chatgpt.com/docs/build-skills) for local discovery and invocation policy.

### Claude Code

From the root of the project where you want guidance, copy the Claude Code skill folder:

```sh
mkdir -p .claude/skills
cp -R /path/to/develop-with-guidance/.claude/skills/develop-with-guidance .claude/skills/
```

The installed file should be `.claude/skills/develop-with-guidance/SKILL.md`. Open Claude Code in that project and invoke `/develop-with-guidance`. This repository already includes that file, so no copy is needed to try it here.

The Claude Code version includes `disable-model-invocation: true` in its frontmatter to require manual invocation. It contains the same mentoring instructions as the Codex version, with no scripts or extra dependencies. See the [official Claude Code skills documentation](https://code.claude.com/docs/en/skills) for installation locations and invocation controls.

### OpenCode

From the root of the project where you want guidance, copy the shared skill and the OpenCode command:

```sh
mkdir -p .agents/skills .opencode/commands
cp -R /path/to/develop-with-guidance/.agents/skills/develop-with-guidance .agents/skills/
cp /path/to/develop-with-guidance/.opencode/commands/develop-with-guidance.md .opencode/commands/
```

OpenCode discovers `.agents/skills/develop-with-guidance/SKILL.md` directly. The command at `.opencode/commands/develop-with-guidance.md` adds `/develop-with-guidance` and tells the assistant to load that shared skill. If you already installed the Codex version in the same project, just copy the command.

Quit and restart OpenCode after installation, then run:

```text
/develop-with-guidance Help me build the next feature one stage at a time.
```

This repository already includes both files, so no copy is needed to try it here. You can also explicitly ask OpenCode to load the `develop-with-guidance` skill in chat. The skill's description requests explicit activation; Codex's `agents/openai.yaml` policy is specific to Codex and does not enforce invocation behavior in OpenCode.

See the official OpenCode documentation for [skill discovery](https://opencode.ai/docs/skills/) and [custom commands](https://opencode.ai/docs/commands/).

## Use it

Copy this into Codex and replace the placeholders. In Claude Code or OpenCode, replace the first line with `/develop-with-guidance`. You can omit anything your assistant can discover from the repository.

```text
$develop-with-guidance

What I am building: [project and purpose]
Language or framework, if known: [language/framework]
What already works: [existing behavior]
Next feature or ticket: [describe the feature, give a local ticket path, or link an issue]
Relevant files or requirements: [paths, acceptance criteria, constraints, or examples]

Read the relevant project files first, explain how the feature fits,
then guide me through one manageable stage at a time. Let me write the code.
```

For a feature described in chat:

```text
$develop-with-guidance
I am building a React task tracker. Adding and listing tasks already works.
Help me add a filter for completed tasks. Discover the relevant files from
the repository and guide me through the first stage.
```

For a Matt Pocock `to-tickets` ticket:

```text
$develop-with-guidance
Guide me through the ticket at .scratch/task-filters/issues/01-status-filter.md
created by Matt Pocock's to-tickets workflow. Read the ticket and relevant
code, check its requirements and any blockers, then help me implement it myself.
```

The feature and ticket examples above also work in Claude Code and OpenCode with `/develop-with-guidance` as the first line. For example:

```text
/develop-with-guidance
I am building a React task tracker. Adding and listing tasks already works.
Help me add a filter for completed tasks. Read the relevant project files,
then guide me through one manageable stage at a time. Let me write the code.
```

Use your actual ticket path. This skill does not require Matt Pocock's plugin; the ticket supplies the requirements. If your assistant cannot access a ticket or file, it will ask for the specific missing content.

## What to expect

| Your request | Assistant's response |
| --- | --- |
| “Guide me through this feature.” | Reads the project, explains what exists, gives a short roadmap, teaches one stage, then pauses for your attempt. |
| “Check my code” or “check again.” | Reads your latest saved code, runs appropriate available checks, and explains what is correct and what needs fixing. Review is read-only. |
| “Show me a code block for this step.” | Shows the relevant code in chat and explains where it belongs and what it replaces. Project files stay unchanged. |
| “Implement this step for me.” | Edits the requested scope in coherent stages, explains each change before continuing, and verifies the result. Then returns to guidance. |

Each guided stage uses **Your task**, **Necessary information**, **Apply it**, and **How to check it**. Your assistant assumes basic programming knowledge and explains unfamiliar APIs, libraries, framework behavior, and language features as they arise.

Mentor mode stays active throughout the conversation until you say **“exit mentor mode.”** A request for a snippet or implementation applies to that request; it does not permanently change the default.

## Local review

Review these files before installing or sharing:

- [Codex skill instructions](.agents/skills/develop-with-guidance/SKILL.md)
- [Codex explicit-invocation configuration](.agents/skills/develop-with-guidance/agents/openai.yaml)
- [Claude Code skill instructions and invocation setting](.claude/skills/develop-with-guidance/SKILL.md)
- [OpenCode command](.opencode/commands/develop-with-guidance.md)

When updating the mentoring behavior, keep the Markdown body identical in both `SKILL.md` files. Only the frontmatter differs for each tool's invocation syntax and configuration. Codex and OpenCode share the `.agents` version; the OpenCode command loads it without duplicating the mentoring instructions.

For a quick behavioral trial in a project, request guidance, save an attempt and ask for review, ask for a snippet, then explicitly request implementation of one step. Confirm that only the implementation request edits source files and that subsequent work returns to guidance.
