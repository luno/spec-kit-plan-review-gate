# spec-kit-plan-review

Spec Kit extension for requiring a plan to be reviewed before proceeding with `/speckit.tasks`.

## What It Does

When `/speckit.tasks` is invoked, this extension runs a mandatory `before_tasks` check that verifies `spec.md` and `plan.md` have been merged to the default branch via a merge request (or pull request). If either file is new (not yet on the default branch), task generation is blocked.

### Flow

1. Run `/speckit.specify` and `/speckit.plan` to create spec artifacts
2. Commit, push, and create an MR/PR for review
3. Get the MR/PR reviewed and merged
4. Now `/speckit.tasks` will pass the gate and generate tasks

## Installation

```bash
specify extension add plan-review --from https://github.com/luno/spec-kit-plan-review/archive/refs/heads/main.zip
```

Or install from a local clone:

```bash
git clone https://github.com/luno/spec-kit-plan-review.git
specify extension add plan-review --dev ./spec-kit-plan-review
```

## How It Works

The extension registers a mandatory `before_tasks` hook. When `/speckit.tasks` runs, it:

1. Locates the feature's `spec.md` and `plan.md`
2. Checks whether both files exist on the default branch (`main`/`master`)
3. If either file is new or has uncommitted changes — **blocks task generation**
4. If both files are merged and clean — **allows task generation to proceed**

There is no skip option. The gate is based on git state, not user confirmation.

## Hooks

| Hook          | Type      | Behaviour                                                        |
|---------------|-----------|------------------------------------------------------------------|
| before_tasks  | Mandatory | Blocks `/speckit.tasks` unless spec.md and plan.md are merged    |

## License

MIT
