# tz-skills

> Enterprise coding standards as a Claude Code plugin — skills, rules, and slash commands for consistent, high-quality development.

## Install

```bash
# Reference locally
claude --plugin-dir /path/to/tz-skills

# Or clone and reference
git clone https://github.com/arozz7/skills.git tz-skills
claude --plugin-dir ./tz-skills
```

## Skills

Skills are activated automatically by Claude Code based on context. Invoke with `/tz-skills:<skill-name>`.

| Skill | Description |
|---|---|
| `/tz-skills:tdd-enforcer` | TDD protocol — write failing test, implement, verify |
| `/tz-skills:workspace-janitor` | Two-stage workspace cleanup — identify clutter, remove on approval |
| `/tz-skills:architect-decision-engine` | Architecture decision support |
| `/tz-skills:codebase-mapper` | Map and document codebase structure |
| `/tz-skills:handover` | Generate handover summaries for context transfer |
| `/tz-skills:security-auditor` | Security audit against OWASP and best practices |
| `/tz-skills:visual-qa-check` | Visual QA checks for UI components |

## Commands (Slash Commands)

Slash commands inject standards into your Claude Code conversation. All commands are namespaced under `/tz-skills:`.

### Architecture & Design
| Command | Purpose |
|---|---|
| `/tz-skills:modular-architecture` | Reasoning/Memory/Tools separation of concerns |
| `/tz-skills:refactoring-protocol` | Safe multi-phase file refactoring protocol |

### Languages & Frameworks
| Command | Purpose |
|---|---|
| `/tz-skills:javascript` | JS/Node.js standards (ES6+, async/await, modules) |
| `/tz-skills:python` | Python standards (PEP 8, type hints, Pydantic) |
| `/tz-skills:react` | React standards (functional components, hooks, state) |
| `/tz-skills:stack-typescript` | TypeScript + React standards (strict typing, Zod) |
| `/tz-skills:electron` | Electron standards (IPC, context isolation, security) |

### Testing
| Command | Purpose |
|---|---|
| `/tz-skills:testing-tdd` | TDD Red/Green/Refactor cycle and coverage goals |
| `/tz-skills:testing-master` | Master TDD + robustness combined standard |
| `/tz-skills:testing-robustness` | Unit/integration rules and anti-patterns |

### Workflow
| Command | Purpose |
|---|---|
| `/tz-skills:git-workflow` | Branching strategy and conventional commit protocol |
| `/tz-skills:security` | Global security standards (zero trust, secrets, input validation) |

## Project Structure

```
tz-skills/
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest
├── commands/                # Claude Code slash commands (from rules/)
│   ├── git-workflow.md
│   ├── electron.md
│   ├── javascript.md
│   ├── modular-architecture.md
│   ├── python.md
│   ├── react.md
│   ├── refactoring-protocol.md
│   ├── security.md
│   ├── stack-typescript.md
│   ├── testing-master.md
│   ├── testing-robustness.md
│   └── testing-tdd.md
├── skills/                  # Claude Code agent skills
│   ├── tdd-enforcer/
│   ├── workspace-janitor/
│   ├── architect-decision-engine/
│   ├── codebase-mapper/
│   ├── handover/
│   ├── security-auditor/
│   └── visual-qa-check/
├── rules/                   # Source rules (for Antigravity/Gemini agents)
└── CLAUDE.md                # Global always-on standards
```

## Requirements

- Claude Code CLI v1.0.33 or later (`claude --version`)

## License

MIT
