# GHCP-Agents

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE.md)
[![Contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg)](#contributing)

A curated collection of reusable custom agents for common software development
tasks with GitHub Copilot. Use the agents as-is, adapt them to your project, or
use them as examples for building your own.

This project is free and open source under the [MIT License](LICENSE.md).

## Why use custom agents?

Custom agents give GitHub Copilot focused instructions for a particular role or
workflow. A well-scoped agent can help:

- Apply consistent engineering practices across repositories.
- Reuse proven prompts instead of recreating them for every task.
- Keep planning, implementation, testing, review, and documentation workflows
  focused.
- Define clear responsibilities, tool access, validation steps, and expected
  outputs.

## Available agents

| Agent | Version | Description |
| --- | --- | --- |
| [Angular-style commit message generator](agents/angular-style-commit-message-generator.agent.md) | 0.1.1 | Generates concise, meaning-first Angular-style commit messages with normal or strict type lists. Supports independent outcomes and opt-in file persistence. |
| [Agent update reminder](agents/agent-update-reminder.agent.md) | 0.1.0 | Checks installed ghcp-agents profiles for newer versions and relevant update notes, with configurable reminder thresholds and optional CLI startup checks. |
| [Generic code reviewer](agents/generic-code-reviewer.agent.md) | 0.1.1 | Reviews supplied changes or repository scopes for high-confidence correctness, regression, compatibility, reliability, and performance defects with coverage disclosure. |
| [Generic security reviewer](agents/generic-security-reviewer.agent.md) | 0.1.0 | Audits supplied changes or repository scopes for high-confidence exploitable vulnerabilities with evidence, coverage disclosure, severity, and remediation guidance. |
| [C# code reviewer](agents/csharp-code-reviewer.agent.md) | 0.1.1 | Reviews C# and .NET changes or repository scopes for high-confidence correctness, compatibility, reliability, and performance defects across ASP.NET Core, EF Core, Blazor, and runtime code. |
| [C# security reviewer](agents/csharp-security-reviewer.agent.md) | 0.1.0 | Audits C# and .NET changes or repository scopes for high-confidence exploitable vulnerabilities across ASP.NET Core, EF Core, Blazor, and runtime boundaries. |
| [Python code reviewer](agents/python-code-reviewer.agent.md) | 0.1.1 | Reviews Python changes or repository scopes for high-confidence correctness, compatibility, reliability, and performance defects across Django, Flask, FastAPI, SQLAlchemy, Pydantic, packaging, and runtime behavior. |
| [Python security reviewer](agents/python-security-reviewer.agent.md) | 0.1.0 | Audits Python changes or repository scopes for high-confidence exploitable vulnerabilities across Django, Flask, FastAPI, SQLAlchemy, Pydantic, packaging, and runtime boundaries. |

Example requests:

- **`angular-style-commit-message-generator`:** Generate a commit message for
  the staged changes.
- **`angular-style-commit-message-generator`:** Generate a commit message in
  strict mode for the staged changes.
- **`angular-style-commit-message-generator`:** Generate separate commit
  messages for each independent change.
- **`angular-style-commit-message-generator`:** Generate the commit messages
  and append them to
  `.semrelease/this_release`.
- **`agent-update-reminder`:** Check installed ghcp-agents profiles for updates.
- **`agent-update-reminder`:** Remind me only about minor or major updates.
- **`agent-update-reminder`:** Save my reminder threshold as major in the
  instructions file I specify.
- **`generic-code-reviewer`:** Review the attached changes since the last
  commit for high-confidence code defects.
- **`generic-code-reviewer`:** Audit the whole repository for high-confidence
  code defects and report examined areas and coverage limitations.
- **`generic-security-reviewer`:** Review the attached changes since the last
  push for exploitable security vulnerabilities.
- **`generic-security-reviewer`:** Audit the whole repository for exploitable
  security vulnerabilities and report examined areas and coverage limitations.
- **`csharp-code-reviewer`:** Review the attached C# and .NET changes for
  high-confidence ASP.NET Core, EF Core, Blazor, and runtime defects.
- **`csharp-security-reviewer`:** Audit the attached C# and .NET changes for
  exploitable ASP.NET Core, EF Core, Blazor, and runtime vulnerabilities.
- **`python-code-reviewer`:** Review the attached Python changes for
  high-confidence framework, ORM, packaging, and runtime defects.
- **`python-security-reviewer`:** Audit the attached Python changes for
  exploitable framework, ORM, packaging, and runtime vulnerabilities.

The reviewer agents intentionally expose only read and search tools. Use the
C# reviewers for C# and .NET scopes, the Python reviewers for Python scopes,
and the generic reviewers for other or mixed ecosystems. Supply the change
context for change reviews. Whole-repository audits must be requested
explicitly and report their coverage limitations.

See [ROADMAP.md](ROADMAP.md) for planned agent categories and future work.

## Getting started

### 1. Choose an agent

Browse the [available agents](#available-agents) or the `agents/` directory and
select the `.agent.md` profile that matches your task.

### 2. Add it to your project

Copy the selected profile into `.github/agents/` in the repository where you
want to use it:

```text
your-project/
└── .github/
    └── agents/
        └── agent-name.agent.md
```

You may edit the copied profile to include project-specific tools, conventions,
commands, or constraints.

For example, copy
`agents/angular-style-commit-message-generator.agent.md` to
`.github/agents/angular-style-commit-message-generator.agent.md`.

### 3. Use the agent

Open the repository in a GitHub Copilot client that supports custom agents and
select the installed agent. In GitHub Copilot CLI, run `/agent` to browse and
select available agents.

To select an agent directly:

```text
/agent angular-style-commit-message-generator
/agent agent-update-reminder
/agent generic-code-reviewer
/agent generic-security-reviewer
/agent csharp-code-reviewer
/agent csharp-security-reviewer
/agent python-code-reviewer
/agent python-security-reviewer
```

See GitHub's
[custom agent documentation](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-custom-agents)
and
[configuration reference](https://docs.github.com/en/copilot/reference/custom-agents-configuration)
for current client support and profile options.

## Commit-message modes

The Angular-style commit message generator supports two header-type modes:

- **Normal (default):** Common Angular-style types plus canonical extensions
  recognized by [action-semrelease](https://github.com/btnguyen2k/action-semrelease),
  such as `breaking`, `deprecate`, `patch`, `improvement`, `optimize`, `security`,
  and `deps`.
- **Strict:** Only Angular's eight regular header types: `build`, `ci`, `docs`,
  `feat`, `fix`, `perf`, `refactor`, and `test`. Types such as `chore`, `style`,
  `revert`, and the normal-mode extensions are excluded.

Choose a mode explicitly in your request, or pre-set a default in an
instructions file your client loads:

```text
For angular-style-commit-message-generator, use strict mode by default.
```

An explicit mode in the current request overrides the configured default for
that invocation only. Both modes keep `type(scope): short summary`, an optional
scope, single-line output, and opt-in file writing. Strict mode changes the
regular type list, not Angular-specific scopes or full body/footer rules.

In normal mode, release impact follows the action's type rules: major-impact
types are reserved for actual compatibility breaks, `refactor` and `deprecate`
are minor, and `security` and `deps` are patch. Types such as `docs` and `chore`
do not themselves trigger a release. Words in the summary cannot change that
classification; use a major-impact type rather than `fix` to request a breaking
release. Strict single-line headers do not encode that extra release metadata.

## Agent update reminders

Install
[`agents/agent-update-reminder.agent.md`](agents/agent-update-reminder.agent.md)
in the destination repository's `.github\agents\` directory. Select
`/agent agent-update-reminder`, then ask it to check for updates.

The reminder inspects only installed profiles whose `metadata.repository`
identifies `btnguyen2k/ghcp-agents`; unrelated agents are ignored. It compares
each agent's own `metadata.version` with a pinned snapshot of upstream `main`
through GitHub's API, not the repository's overall release number. Checks are
version-based: different content with the same version is not an update.
Installed profiles and local customizations are never automatically replaced.

GitHub Releases are the preferred source for "what's new" because notes are
associated with published tags. `RELEASE-NOTES.md` is the fallback. Notes are
matched to the affected agents, and missing notes or unverified release status
are disclosed rather than guessed. A newer version on `main` can trigger a
reminder even before it appears in a published release.

### Reminder thresholds

| Threshold | Notifications |
| --- | --- |
| `any` (default) | Any newer semantic version, including patch updates and prerelease-to-stable promotions. |
| `minor` | Minor or major increases. |
| `major` | Major increases only. |

Ask for a threshold for one check, or explicitly ask Copilot to remember it
with repository or user scope when client memory support is available. You can
also save this sentence in an instructions file your client loads:

```text
For ghcp-agents update reminders, use threshold minor.
```

An explicit threshold for the current check takes precedence over saved
preferences; repository-specific preferences take precedence over user-wide
preferences. Nothing is persisted without an explicit request.

### Optional Copilot CLI startup checks

Copy the
[`hooks/agent-update-reminder.json`](hooks/agent-update-reminder.json)
template to `.github\hooks\agent-update-reminder.json` in the destination
repository after installing the agent. Preserve any existing hook configuration.
The template is not enabled in this repository.

The `sessionStart` prompt hook submits the check automatically on **new
interactive sessions** in CLI versions that support prompt hooks. It does not
run on resumed sessions or in non-interactive `-p` mode. An agent profile alone
does not auto-start. Startup checks use AI credits like other model requests
and remain subject to workspace trust, tool availability, and network approval.

The checker reports incomplete or failed checks instead of claiming everything
is up to date, and it does not modify agents or save preferences at startup.
Remove the installed hook file to disable automatic checks; manual checks
remain available.

See the official
[Copilot hooks reference](https://docs.github.com/en/copilot/reference/hooks-configuration#prompt-hooks)
for startup-hook support and configuration.

## Agent conventions

Agents contributed to this repository should:

- Have one clear responsibility and a concise description.
- Use a lowercase, kebab-case filename ending in `.agent.md`.
- Request only the tools and permissions needed for their task.
- State important boundaries, expected outputs, and validation steps.
- Avoid secrets and assumptions that only apply to one private environment.
- Be useful without requiring proprietary project context.
- Include a version number, author, and repository URL in `metadata`. The
  creator is responsible for maintaining the version number.

A minimal profile looks like this (replace the metadata placeholders):

```markdown
---
name: example-agent
description: Explain the specific task this agent performs and when to use it.
metadata:
  version: "<version-number>"
  author: "<author>"
  repository: "<repository-url>"
---

Define the agent's role, workflow, constraints, and expected output here.
```

## Contributing

Contributions are welcome. To propose a new agent or improve an existing one:

1. Fork the repository and create a focused branch.
2. Add or update an agent profile under `agents/`.
3. Do not copy it into this repository's `.github/agents/` directory unless
   repository maintainers explicitly request local enablement.
4. Test the profile against a representative development task.
5. Update the agent catalog in this README when applicable.
6. Open a pull request describing the use case and expected behavior.

Please keep each pull request focused on one agent or one closely related
improvement. By contributing, you agree that your contribution will be
licensed under the MIT License.

Use
[GitHub Issues](https://github.com/btnguyen2k/ghcp-agents/issues)
to report a problem, request an agent, or suggest an improvement.

## License

Distributed under the [MIT License](LICENSE.md). You are free to use, copy,
modify, merge, publish, and distribute these agents subject to the license
terms.

## Disclaimer

This is a community-maintained project and is not affiliated with or endorsed
by GitHub. GitHub and GitHub Copilot are trademarks of GitHub, Inc.
