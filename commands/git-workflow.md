---
description: Apply Git & version control standards — branching strategy, conventional commit messages, and staging protocol. Use when managing source control, commits, or PRs.
---

# Git & Version Control Standards

## Branching Strategy
- **Main Branch:** `main` (Protected. NO direct commits).
- **Feature Branches:** Must follow format: `feature/phase-XX-short-desc` or `fix/issue-desc`.
  - Example: `feature/phase-01-scaffold`
  - Example: `fix/auth-token-expiry`

## The Commit Protocol
**Do NOT** generate a generic commit message like "update files."
**Do** use the `aiChangeLog` to generate the message.

### Step 1: Staging
- Run `git status` to verify changes.
- Stage relevant files: `git add <files>` (Avoid `git add .` unless you are certain of all changes).

### Step 2: Message Generation
Read the current `aiChangeLog/phase-XX.md` file, specifically the "Completed Tasks" or "Diff Narrative" section.
Construct the commit message using **Conventional Commits**:

Format:
```
<type>(<scope>): <subject>

<detailed summary from aiChangeLog>
```

Types: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`

### Step 3: Push
- Push to the **feature branch** only: `git push origin feature/phase-XX-short-desc`
- **NEVER** push directly to `main`.

## 🛑 What Not To Do
- Never use `git push --force` on shared branches.
- Never commit `.env` files, secrets, or credentials.
- Never commit directly to `main`.
