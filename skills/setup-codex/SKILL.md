---
name: setup-codex
description: Install, update, preview, or explain mikeploythai's opinionated Codex multi-agent setup. Use only when the user explicitly invokes $setup-codex or /setup-codex, or explicitly asks for Mike's Codex setup. Do not use for ordinary Codex questions or software-development work.
---

# Setup Codex

Install a cost-effective Codex configuration with a capable orchestrator, focused workers, and independent review.

## Weekly update check

On Mondays, compare this skill's installed `skillFolderHash` with the Git tree SHA for `skills/setup-codex` on the `main` branch of `mikeploythai/skills`. This check must be read-only. Do not run `npx skills check`, because it updates skills.

If the hashes differ, ask the user whether they want to update. Only after they approve, run `npx skills update setup-codex`. Stay quiet when the skill is current or the check cannot run, and do not let the check block the requested work.

## Setup

- Orchestrator: `gpt-5.6-sol` at `xhigh`.
- Researcher: `gpt-5.6-luna` at `max`, read-only.
- Engineer: `gpt-5.6-luna` at `max`, workspace-write.
- Reviewer: `gpt-5.6-sol` at `xhigh`, read-only.
- Up to four concurrent agent threads.
- On-request approvals, workspace-write sandboxing, live web search, concise reasoning summaries, low verbosity, pragmatic personality, and experimental context management.

Read [the configuration reference](references/configuration.md) for the exact settings and agent definitions before previewing or applying them.

## Marketplace plugins

Recommend the GitHub and Linear marketplace plugins. During preview, report whether each is installed and connected. When available, use GitHub for repository, issue, and pull-request work, and Linear for project and issue workflows.

Do not install plugins, connect accounts, or change permissions unless the user explicitly asks. Missing plugins do not block the core Codex setup.

## Safety boundary

Treat requests to show, preview, explain, or review the setup as read-only. Modify the user's Codex configuration only when they explicitly ask to install or update the setup.

An explicit install or update request authorizes the non-conflicting changes defined by this skill. Do not ask for another confirmation unless applying the setup would discard existing configuration or agent instructions.

Determine the user's Codex home before editing. Use the configured Codex home when one exists; otherwise use `~/.codex`.

Inspect these before making changes:

- `config.toml`
- `agents/researcher.toml`
- `agents/engineer.toml`
- `agents/reviewer.toml`

Preserve unrelated settings, comments, profiles, MCP servers, skills, providers, and agent files. Never replace the entire `config.toml` with the reference example or append duplicate keys.

If a target agent file already exists:

- Leave it unchanged when its effective contents match the reference.
- Update it when it is clearly an earlier version of this setup.
- If it contains different custom instructions, show the conflict and ask the user whether to preserve, merge, or replace it.

Before changing an existing file, create a timestamped backup beside it.

## Apply

Merge only the settings owned by this setup:

- `model`
- `model_reasoning_effort`
- `agents.enabled`
- `agents.default_subagent_model`
- `agents.default_subagent_reasoning_effort`
- `agents.max_concurrent_threads_per_session`
- `approval_policy`
- `sandbox_mode`
- `sandbox_workspace_write.network_access`
- `web_search`
- `model_verbosity`
- `model_reasoning_summary`
- `personality`
- `features.context_management.experimental_mode`

Install the three agent definitions from the configuration reference. Create missing directories as needed.

Write complete files through temporary files and replace their targets only after the resulting TOML parses successfully.

## Automatically use Mike's way

After setup, explain that the user can optionally make `$mikes-way` the default for software development work. Setup alone does not enable this. If they ask to "automate Mike's way" or otherwise enable that default, add the following block to `<codex-home>/AGENTS.md`:

```markdown
<!-- MIKES_WAY_START -->
## Mike's way

Use $mikes-way by default for software development work. Its engineering-manager role applies to the primary agent; subagents execute their assigned roles directly.
<!-- MIKES_WAY_END -->
```

Confirm that `mikes-way` is installed before reporting it as enabled. If it is missing, explain that it needs to be installed first.

Read the existing global instructions and preserve everything outside this marked block. Create `AGENTS.md` if it is missing. Leave an identical block alone and update an existing marked block instead of adding a duplicate. Use the backup procedure above before changing an existing file. If the block has conflicting custom instructions, show the conflict before replacing them.

Check for `<codex-home>/AGENTS.override.md`, which can take precedence over `AGENTS.md`. If it would hide the new block, explain that and resolve the instruction location with the user rather than claiming the default is active. See [Codex's instruction discovery rules](https://learn.chatgpt.com/docs/agent-configuration/agents-md).

After an opted-in update, read the file back and verify that the exact block appears once and unrelated instructions are unchanged. This option changes global instructions only; it does not rerun the rest of setup.

## Verify

After applying the setup:

1. Parse every changed TOML file.
2. Confirm each owned setting has exactly one effective definition and the expected value.
3. Confirm all three agent files exist with their intended model, reasoning effort, sandbox, and instructions.
4. Report the files changed, files created, backups written, and conflicts left unresolved.
5. Tell the user to restart Codex and start a new task for the setup to take effect. If automatic Mike's way is not already enabled, also tell them: "You can restart Codex now, or ask me to automate Mike's way for all engineering tasks first, then restart Codex." If it is already enabled, confirm that and give the restart instruction.

If validation fails, restore the affected file from its backup and report the failure.
