# tz-skills Plugin

This repo is a Claude Code plugin. Install with: `claude --plugin-dir ./`

**Skills** (activated by context): `tdd-enforcer`, `workspace-janitor`, `architect-decision-engine`, `codebase-mapper`, `handover`, `security-auditor`, `visual-qa-check`

**Commands** (slash commands): `/tz-skills:git-workflow`, `/tz-skills:security`, `/tz-skills:modular-architecture`, `/tz-skills:refactoring-protocol`, `/tz-skills:javascript`, `/tz-skills:python`, `/tz-skills:react`, `/tz-skills:stack-typescript`, `/tz-skills:electron`, `/tz-skills:testing-tdd`, `/tz-skills:testing-master`, `/tz-skills:testing-robustness`

---

# Global Standards

## Core Philosophy
> Readability and Order > Speed and Complex Interconnections

Prioritize maintainability and clarity over clever, hyper-optimized code. Follow SOLID, DRY, and KISS principles. Write clean, self-documenting code with meaningful names and small functions.

- **SOLID Principles:** Single Responsibility, Open-Closed, Liskov Substitution, Interface Segregation, Dependency Inversion
- **DRY (Don't Repeat Yourself):** Extract common logic into reusable functions, classes, or modules
- **KISS (Keep It Simple):** Avoid over-engineering
- **Clean Code:** Readable, self-documenting code with meaningful names, small functions, clear structure

## 🛑 The Golden Rule
**DO NOT WRITE CODE** until an Implementation Plan is explicitly approved by the user (for multi-file or architectural changes).
**NO DELETIONS** are allowed without explicit confirmation.

## Planning Before Coding
For multi-file or architectural changes, propose an **Implementation Plan** before writing code. Wait for approval.

### Plan Structure
1. **Phases** (Epics): Distinct stages of work
2. **Tasks** (Features): Granular steps within phases
3. **TDD Requirement:** Tasks should list unit tests to be written *before* code (when applicable)
4. **Change Log:** Every phase includes updating `aiChangeLog/phase-XX.md`
5. **Documentation:** Keep docs up to date and update README.md

### Architecture Check (Modular Mode)
Before coding, define the Interfaces:
1. Define the `MemoryInterface` (What data do we store/retrieve?)
2. Define the `ToolInterfaces` (What actions do we perform?)
3. Define the `ReasoningLoop` (How does the agent use the above?)

*Proof of Modularity:* Can we write a `MockMemory` and `MockTool` class and run the `ReasoningLoop` without an internet connection? If yes, proceed.

For simple single-file changes, proceed directly without formal planning.

## Modular Architecture (Separation of Concerns)
All systems use three loosely-coupled layers:
1. **Reasoning Layer (Brain)** — Pure business logic. Never hardcode tool implementations in logic. Stateless.
2. **Memory Layer (Context)** — State and storage. Use Repository Pattern (`memory.save(data)` not `redis.set(data)`).
3. **Tools Layer (Hands)** — Side effects (API calls, file I/O). Wrap in interfaces/abstract classes using Adapter Pattern.

Guardrails:
- No direct vendor imports in business logic — wrap in a service (e.g., `LLMService`)
- Use dependency injection — pass tools/memory as arguments, never instantiate inside
- Config-driven — prompts and model configs live in external config files (JSON/YAML), not in code

## Refactoring Protocol
Never split a large file in a single pass. Never modify the original until the new file is created and verified.

### Refactoring Safety Check
If the user asks for a Refactor (Code Cleanup, Optimization, Re-organizing):
1. **Run Tests FIRST:** Ensure all current tests pass. (If they fail, stop and fix them)
2. **Refactor:** Apply the changes
3. **Run Tests AGAIN:** If any test fails, you have broken behavior. **Revert** and analyze

### Steps
1. **Plan** — Identify boundaries, map dependencies, check for circular deps. Propose new structure, wait for approval.
2. **Move** — One chunk at a time. Create destination with imports, verify it compiles, then update original to import new module. Change location only, not logic.
3. **Cleanup** — Remove dead code and unused imports, update tests for moved components.
4. **Log Changes:** Record file mappings in `aiChangeLog/phase-XX.md` (e.g., `Moved 'calculateTax' from 'utils.ts' -> 'tax_service.ts'`)

**DO NOT** delete files during refactoring. Refactor or deprecate instead.

Trigger: Propose this protocol when a file exceeds 400 lines (soft) or 600 lines (hard).

## Error Handling
Implement robust error handling with structured logging. Use low-cardinality, stable message strings:
- `logger.info({id, foo}, 'Message')`
- `logger.error({error}, 'Another message')`

## File Size Limits
- Soft limit: 300 lines — consider splitting
- Hard limit: 600 lines — must split using refactoring protocol

## Mandatory Folder Convention
Ensure the following structure exists or is planned:
- `/aiChangeLog/` — Process tracking
- `/scripts/release/` — Versioning, tagging
- `/scripts/deploy/` — Bootstraps, smoke tests
- `/docs/` — Documentation

## Shell & Terminal Standards (Windows)
- **Primary Shell:** PowerShell (pwsh)
- **Forbidden:** Bash, Sh, Zsh, Unix commands (unless inside WSL)
- **Scripts:** Generate with `.ps1` extension
- **Execution Policy:** Assume unrestricted or bypass flags may be needed (`powershell -ExecutionPolicy Bypass -File script.ps1`)

### Command Translation Guide
- `touch` → `New-Item -ItemType File -Path "filename"` or `echo $null >> filename`
- `mkdir` → `New-Item -ItemType Directory -Path "dirname"`
- `rm -rf` → `Remove-Item -Recurse -Force`
- `cp` → `Copy-Item`
- `mv` → `Move-Item`
- `cat` → `Get-Content`
- `grep` → `Select-String`
- `export VAR=val` → `$env:VAR = "val"`

### Path Handling
- Prefer forward slashes `/` where possible (PowerShell accepts them)
- If backslashes `\` are required, ensure they are double-escaped in strings `\\`

### What Not To Do
- Do not use `sudo`. Use "Run as Administrator" context instructions if privileges are needed
- Do not chain commands with `&&` unless using PowerShell 7+. Use `;` or `if ($?) { ... }` for older compatibility
