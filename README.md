# spec-kit-plan-review

Spec Kit extension for requiring a plan to be reviewed before proceeding with `/speckit.tasks`.

## What It Does

After `/speckit.plan` completes, this extension automatically:

1. Reads the generated `spec.md` and `plan.md` artifacts
2. Commits the spec artifacts to the current branch
3. Creates a merge request (GitLab) or pull request (GitHub) for code review
4. Gates `/speckit.tasks` — the user must get the plan reviewed before generating tasks

## Installation

```bash
specify extension add plan-review --from https://github.com/luno/spec-kit-plan-review/archive/refs/heads/main.zip
```

Or install from a local clone:

```bash
git clone https://github.com/luno/spec-kit-plan-review.git
specify extension add plan-review --dev ./spec-kit-plan-review
```

## Usage

The extension hooks into the Spec Kit lifecycle automatically. After running `/speckit.plan`, the review gate fires and:

1. Stages and commits all spec artifacts
2. Pushes the branch and creates an MR/PR with a summary of key decisions
3. Stops — you must get the MR/PR reviewed before running `/speckit.tasks`

You can also invoke it manually:

```
/speckit.plan-review.request
```

## Hooks

| Hook        | Event     | Behaviour                                              |
|-------------|-----------|--------------------------------------------------------|
| after_plan  | Mandatory | Fires automatically after `/speckit.plan` completes    |

## Platform Support

- **GitLab** — creates a merge request via `glab` CLI or GitLab MCP tools
- **GitHub** — creates a pull request via `gh` CLI

## License

MIT
