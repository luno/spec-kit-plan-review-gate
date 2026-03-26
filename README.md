# spec-kit-plan-review

Spec Kit extension for requiring a plan to be reviewed before proceeding with `/speckit.tasks`.

## What It Does

After `/speckit.plan` completes, this extension automatically fires a mandatory review gate that:

1. Reads the generated `spec.md` and `plan.md` artifacts
2. Presents a summary of key decisions (tech stack, data model, user stories, risks)
3. Asks the user to get the plan reviewed by a human before proceeding
4. Blocks `/speckit.tasks` until the review is confirmed or explicitly skipped

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

The extension hooks into the Spec Kit lifecycle automatically. No manual invocation needed.

After running `/speckit.plan`, the review gate fires and presents options:

- **Share directly** — send spec.md and plan.md to a colleague
- **Create an MR** — commit spec artifacts and open a merge request
- **Pair review** — walk through the plan with a teammate

You can also invoke it manually:

```
/speckit.plan-review.request
```

## Hooks

| Hook         | Event        | Behaviour |
|-------------|-------------|-----------|
| after_plan  | Mandatory   | Fires automatically after `/speckit.plan` completes |

## License

MIT
