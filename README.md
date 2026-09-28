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
| [Angular-style commit message generator](agents/angular-style-commit-message-generator.agent.md) | 0.1.0 | Generates concise, meaning-first Angular-style commit messages from selected or staged changes. Supports independent outcomes and opt-in file persistence. |
| [Generic code reviewer](agents/generic-code-reviewer.agent.md) | 0.1.0 | Reviews supplied changes or repository scopes for high-confidence correctness, regression, compatibility, reliability, and performance defects with coverage disclosure. |
| [Generic security reviewer](agents/generic-security-reviewer.agent.md) | 0.1.0 | Audits supplied changes or repository scopes for high-confidence exploitable vulnerabilities with evidence, coverage disclosure, severity, and remediation guidance. |

Example requests:

- **`angular-style-commit-message-generator`:** Generate a commit message for
  the staged changes.
- **`angular-style-commit-message-generator`:** Generate separate commit
  messages for each independent change.
- **`angular-style-commit-message-generator`:** Generate the commit messages
  and append them to
  `.semrelease/this_release`.
- **`generic-code-reviewer`:** Review the attached changes since the last
  commit for high-confidence code defects.
- **`generic-code-reviewer`:** Audit the whole repository for high-confidence
  code defects and report examined areas and coverage limitations.
- **`generic-security-reviewer`:** Review the attached changes since the last
  push for exploitable security vulnerabilities.
- **`generic-security-reviewer`:** Audit the whole repository for exploitable
  security vulnerabilities and report examined areas and coverage limitations.

The reviewer agents intentionally expose only read and search tools. Supply the
change context for change reviews. Whole-repository audits must be requested
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
/agent generic-code-reviewer
/agent generic-security-reviewer
```

See GitHub's
[custom agent documentation](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-custom-agents)
and
[configuration reference](https://docs.github.com/en/copilot/reference/custom-agents-configuration)
for current client support and profile options.

## Agent conventions

Agents contributed to this repository should:

- Have one clear responsibility and a concise description.
- Use a lowercase, kebab-case filename ending in `.agent.md`.
- Request only the tools and permissions needed for their task.
- State important boundaries, expected outputs, and validation steps.
- Avoid secrets and assumptions that only apply to one private environment.
- Be useful without requiring proprietary project context.

A minimal profile looks like this:

```markdown
---
name: example-agent
description: Explain the specific task this agent performs and when to use it.
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
