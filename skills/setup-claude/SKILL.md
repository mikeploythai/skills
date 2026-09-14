---
name: setup-claude
description: Install, update, preview, or explain mikeploythai's opinionated Claude Code multi-agent setup. Use only when the user explicitly invokes $setup-claude or /setup-claude, or explicitly asks for Mike's Claude Code setup. Do not use for ordinary Claude Code questions or software-development work.
---

# Setup Claude

Install a cost-effective Claude Code configuration with a capable orchestrator, focused workers, and independent review.

## Weekly update check

On Mondays, compare this skill's installed `skillFolderHash` with the Git tree SHA for `skills/setup-claude` on the `main` branch of `mikeploythai/skills`. This check must be read-only. Do not run `npx skills check`, because it updates skills.

If the hashes differ, ask the user whether they want to update. Only after they approve, run `npx skills update setup-claude`. Stay quiet when the skill is current or the check cannot run, and do not let the check block the requested work.

## Setup

- Orchestrator: Opus 5 at `xhigh`.
- Researcher: Sonnet 5 at `max`, no file edits.
- Engineer: Sonnet 5 at `max`, edits accepted inside the workspace.
- Reviewer: Opus 5 at `xhigh`, no file edits.
- Prompted approvals, Bash sandboxing where the platform supports it, and visible thinking summaries.

Claude Code has no session cap on concurrent subagents and no global default subagent model. Parallelism comes from batching agent calls in a single turn, and each agent file carries its own model and effort. Web search, web fetch, and context compaction are built in and need no configuration.

Read [the configuration reference](references/configuration.md) for the exact settings and agent definitions before previewing or applying them.

## Built-in agents and skills

Claude Code ships `Explore` and `Plan` subagents and a `/code-review` skill. Prefer them for quick lookups, planning, and routine diff review. The installed agents are for work that should carry mikeploythai's rules end to end: sustained investigation, a bounded implementation slice, and review against agreed acceptance criteria.

## Plugins and MCP servers

Recommend the GitHub and Linear MCP servers. During preview, report whether each is configured and connected. When available, use GitHub for repository, issue, and pull-request work, and Linear for project and issue workflows.

Do not install servers, connect accounts, or change permissions unless the user explicitly asks. Missing servers do not block the core setup.

## Safety boundary

Treat requests to show, preview, explain, or review the setup as read-only. Modify the user's Claude Code configuration only when they explicitly ask to install or update the setup.

An explicit install or update request authorizes the non-conflicting changes defined by this skill. Do not ask for another confirmation unless applying the setup would discard existing configuration or agent instructions.

Determine the user's Claude home before editing. Use `CLAUDE_CONFIG_DIR` when it is set; otherwise use `~/.claude`.

Inspect these before making changes:

- `settings.json`
- `agents/researcher.md`
- `agents/engineer.md`
- `agents/reviewer.md`

Preserve unrelated settings, hooks, permission rules, environment variables, status lines, MCP servers, plugins, skills, and agent files. Never replace the entire `settings.json` with the reference example.

If a target agent file already exists:

- Leave it unchanged when its effective contents match the reference.
- Update it when it is clearly an earlier version of this setup.
- If it contains different custom instructions, show the conflict and ask the user whether to preserve, merge, or replace it.

Before changing an existing file, create a timestamped backup beside it.

## Apply

Merge only the settings owned by this setup:

- `model`
- `effortLevel`
- `showThinkingSummaries`
- `permissions.defaultMode`
- `sandbox.enabled`
- `sandbox.autoAllowBashIfSandboxed`

Install the three agent definitions from the configuration reference. Create missing directories as needed.

Write complete files through temporary files and replace their targets only after the resulting JSON or YAML frontmatter parses successfully.

## Automatically use Mike's way

After setup, explain that the user can optionally make `/mikes-way` the default for software development work. Setup alone does not enable this. If they ask to "automate Mike's way" or otherwise enable that default, add the following block to `<claude-home>/CLAUDE.md`:

```markdown
<!-- MIKES_WAY_START -->
## Mike's way

Use /mikes-way by default for software development work. Its engineering-manager role applies to the primary agent; subagents execute their assigned roles directly.
<!-- MIKES_WAY_END -->
```

Confirm that `mikes-way` is installed before reporting it as enabled. If it is missing, explain that it needs to be installed first.

Read the existing global instructions and preserve everything outside this marked block. Create `CLAUDE.md` if it is missing. Leave an identical block alone and update an existing marked block instead of adding a duplicate. Use the backup procedure above before changing an existing file. If the block has conflicting custom instructions, show the conflict before replacing them.

Project and directory `CLAUDE.md` files load alongside this one rather than replacing it, so a project file can override the default in that repository. Point that out instead of claiming the default always wins. See [Claude Code's memory docs](https://code.claude.com/docs/en/memory).

After an opted-in update, read the file back and verify that the exact block appears once and unrelated instructions are unchanged. This option changes global instructions only; it does not rerun the rest of setup.

## Verify

After applying the setup:

1. Parse every changed JSON file and every agent file's YAML frontmatter.
2. Confirm each owned setting has exactly one effective definition and the expected value.
3. Confirm all three agent files exist with their intended model, effort, tool limits, and instructions.
4. Report the files changed, files created, backups written, and conflicts left unresolved.
5. Tell the user that settings and agent files are read when a session starts, so they should start a new Claude Code session for the setup to take effect. If automatic Mike's way is not already enabled, also tell them: "You can start a new session now, or ask me to automate Mike's way for all engineering tasks first, then start a new session." If it is already enabled, confirm that and give the restart instruction.

If validation fails, restore the affected file from its backup and report the failure.
