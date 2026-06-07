# MDW Agents Setup

Reference copies of the `CLAUDE.md` instruction files used to guide Claude Code across global, workspace, and project scopes.

Claude Code uses `CLAUDE.md` files as persistent context. They are loaded into the session so Claude can follow preferred style, workflow, security, and project conventions. They are behavioural instructions, not hard enforcement controls. Use permissions, hooks, managed settings, or repository controls when a rule must be technically enforced.

## Repository Layout

| Folder | Source location on this machine | Intended active location | Purpose |
| --- | --- | --- | --- |
| `global/` | `~/.claude/CLAUDE.md` | `~/.claude/CLAUDE.md` | Personal defaults loaded for all Claude Code sessions for this user. Use for Australian English, preferred tone, workflow expectations, security posture, and collaboration preferences that apply across projects. |
| `workspace/` | `/Users/maddog/Documents/Claudius/CLAUDE.md` | A shared parent folder, for example `/Users/maddog/Documents/Claudius/CLAUDE.md` | Workspace-level guidance loaded when Claude is started inside that folder tree. Use for conventions that apply across a group of related repositories or work areas. |
| `project example/` | `/Users/maddog/Documents/Claudius/Code Projects/Security/CLAUDE.md` | The root of a repository, for example `./CLAUDE.md` or `./.claude/CLAUDE.md` | Project-specific instructions that should travel with the repository. Use for architecture notes, common commands, coding standards, testing expectations, and project-specific security or operational constraints. |

## How Claude Loads These Files

When Claude Code starts in a working directory, it reads relevant `CLAUDE.md` files and adds them to the session context.

The usual layering is:

1. Global user instructions from `~/.claude/CLAUDE.md`.
2. Parent-directory `CLAUDE.md` files found by walking down the directory tree towards the current working directory.
3. The repository or current-directory `CLAUDE.md`.
4. Local-only files such as `CLAUDE.local.md`, where used and not excluded.

More specific files are read later in the context, so project-level guidance can refine broader global or workspace defaults. The files are combined; they do not replace each other.

For example, when starting Claude Code in:

```text
/Users/maddog/Documents/Claudius/Code Projects/Security
```

Claude may use:

```text
~/.claude/CLAUDE.md
/Users/maddog/Documents/Claudius/CLAUDE.md
/Users/maddog/Documents/Claudius/Code Projects/Security/CLAUDE.md
```

## What Belongs Where

Use `global/CLAUDE.md` for:

- Personal communication style and spelling preferences.
- Default security and engineering principles.
- Cross-project workflow preferences.
- Collaboration expectations between Claude, Codex, and the user.

Use `workspace/CLAUDE.md` for:

- Instructions that apply to a whole work area.
- Shared file handling conventions.
- References to canonical workspace-wide guidance.
- Defaults that should apply across several related repositories.

Use `project example/CLAUDE.md` as a model for:

- Repository purpose and scope.
- Supported platforms and runtime assumptions.
- Coding conventions.
- Build, test, lint, or validation commands.
- Security and compliance constraints.
- Collaboration notes that are specific to that repository.

## Operational Notes

- Keep each `CLAUDE.md` concise and factual. Large instruction files consume context and are easier to contradict over time.
- Do not store secrets, tokens, private keys, tenant IDs, or production connection details in any `CLAUDE.md`.
- Keep project `CLAUDE.md` files suitable for source control. Put machine-specific or private notes in `CLAUDE.local.md` and keep that file ignored.
- Review these files periodically so stale guidance does not override current repository behaviour.
- If another agent uses `AGENTS.md`, Claude Code can read that content by importing it from `CLAUDE.md` with `@AGENTS.md`.

## Reference

- Anthropic Claude Code documentation: [How Claude remembers your project](https://code.claude.com/docs/en/memory)
