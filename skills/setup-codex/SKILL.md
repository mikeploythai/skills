---
name: setup-codex
description: install, update, preview, or explain mikeploythai's opinionated codex multi-agent setup. use only when the user explicitly invokes $setup-codex or /setup-codex, or explicitly asks for mike's codex setup. do not use for ordinary codex questions or software-development work.
---

# setup codex

install a cost-effective codex configuration with a capable orchestrator,
focused workers, and independent review.

## weekly update check

on mondays, compare this skill's installed `skillFolderHash` with the git tree
sha for `skills/setup-codex` on the `main` branch of `mikeploythai/skills`.
this check must be read-only. do not run `npx skills check`, because it updates
skills.

if the hashes differ, ask the user whether they want to update. only after they
approve, run `npx skills update setup-codex`. stay quiet when the skill is
current or the check cannot run, and do not let the check block the requested
work.

## setup

- orchestrator: `gpt-5.6-sol` at `xhigh`.
- researcher: `gpt-5.6-luna` at `max`, read-only.
- engineer: `gpt-5.6-luna` at `max`, workspace-write.
- reviewer: `gpt-5.6-sol` at `xhigh`, read-only.
- up to four concurrent agent threads.
- on-request approvals, workspace-write sandboxing, live web search,
  concise reasoning summaries, low verbosity, pragmatic personality, and
  experimental context management.

read [the configuration reference](references/configuration.md) for the
exact settings and agent definitions before previewing or applying them.

## marketplace plugins

recommend the GitHub and Linear marketplace plugins. during preview, report
whether each is installed and connected. when available, use GitHub for
repository, issue, and pull-request work, and Linear for project and issue
workflows.

do not install plugins, connect accounts, or change permissions unless the
user explicitly asks. missing plugins do not block the core codex setup.

## safety boundary

treat requests to show, preview, explain, or review the setup as read-only.
modify the user's codex configuration only when they explicitly ask to
install or update the setup.

an explicit install or update request authorizes the non-conflicting changes
defined by this skill. do not ask for another confirmation unless applying
the setup would discard existing configuration or agent instructions.

determine the user's codex home before editing. use the configured codex
home when one exists; otherwise use `~/.codex`.

inspect these before making changes:

- `config.toml`
- `agents/researcher.toml`
- `agents/engineer.toml`
- `agents/reviewer.toml`

preserve unrelated settings, comments, profiles, mcp servers, skills,
providers, and agent files. never replace the entire `config.toml` with the
reference example or append duplicate keys.

if a target agent file already exists:

- leave it unchanged when its effective contents match the reference.
- update it when it is clearly an earlier version of this setup.
- if it contains different custom instructions, show the conflict and ask
  the user whether to preserve, merge, or replace it.

before changing an existing file, create a timestamped backup beside it.

## apply

merge only the settings owned by this setup:

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

install the three agent definitions from the configuration reference.
create missing directories as needed.

write complete files through temporary files and replace their targets only
after the resulting toml parses successfully.

## automatically use mike's way

after setup, explain that the user can optionally make `$mikes-way` the
default for software development work. setup alone does not enable this.
if they ask to "automate mike's way" or otherwise enable that default, add
the following block to `<codex-home>/AGENTS.md`:

```markdown
<!-- MIKES_WAY_START -->
## Mike's way

Use $mikes-way by default for software development work. Its engineering-manager role applies to the primary agent; subagents execute their assigned roles directly.
<!-- MIKES_WAY_END -->
```

confirm that `mikes-way` is installed before reporting it as enabled.
if it is missing, explain that it needs to be installed first.

read the existing global instructions and preserve everything outside this
marked block. create `AGENTS.md` if missing; leave an identical block alone
and update an existing marked block instead of adding a duplicate. use the
backup procedure above before changing an existing file. if the block has
conflicting custom instructions, show the conflict before replacing them.

check for `<codex-home>/AGENTS.override.md`, which can take precedence over
`AGENTS.md`. if it would hide the new block, explain that and resolve the
instruction location with the user rather than claiming the default is active.
see [Codex's instruction discovery rules](https://learn.chatgpt.com/docs/agent-configuration/agents-md).

after an opted-in update, read the file back and verify that the exact block
appears once and unrelated instructions are unchanged. this option changes
global instructions only; it does not rerun the rest of setup.

## verify

after applying the setup:

1. parse every changed toml file.
2. confirm each owned setting has exactly one effective definition and the
   expected value.
3. confirm all three agent files exist with their intended model, reasoning
   effort, sandbox, and instructions.
4. report the files changed, files created, backups written, and conflicts
   left unresolved.
5. tell the user to restart codex and start a new task for the setup to take
   effect. if automatic mike's way is not already enabled, also tell them:
   "you can restart codex now, or ask me to automate mike's way for all
   engineering tasks first, then restart codex." if it is already enabled,
   confirm that and give the restart instruction.

if validation fails, restore the affected file from its backup and report
the failure.
