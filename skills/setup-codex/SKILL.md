---
name: setup-codex
description: install, update, preview, or explain mikeploythai's opinionated codex multi-agent setup. use only when the user explicitly invokes $setup-codex or /setup-codex, or explicitly asks for mike's codex setup. do not use for ordinary codex questions or software-development work.
---

# setup codex

install a cost-effective codex configuration with a capable orchestrator,
focused workers, and independent review.

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

## verify

after applying the setup:

1. parse every changed toml file.
2. confirm each owned setting has exactly one effective definition and the
   expected value.
3. confirm all three agent files exist with their intended model, reasoning
   effort, sandbox, and instructions.
4. report the files changed, files created, backups written, and conflicts
   left unresolved.
5. tell the user to start a new codex task for the configuration to take
   effect.

if validation fails, restore the affected file from its backup and report
the failure.
