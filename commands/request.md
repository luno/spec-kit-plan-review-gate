---
description: "Request a review of spec.md and plan.md before proceeding to task generation"
---

# Plan Review Gate

Ensure the feature specification and implementation plan have been reviewed by a human before proceeding to task generation.

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Steps

### 1. Locate Feature Artifacts

Run `.specify/scripts/bash/check-prerequisites.sh --json` from the repo root and parse the output to determine the FEATURE_DIR. If the script is not available, search for the most recent `specs/*/plan.md` relative to the repo root.

Read the following files from FEATURE_DIR:
- **spec.md** (required)
- **plan.md** (required)
- **research.md** (optional)
- **data-model.md** (optional)

If either spec.md or plan.md is missing, ERROR: "Cannot request review — spec.md and plan.md must both exist. Run `/speckit.specify` and `/speckit.plan` first."

### 2. Present Review Summary

Generate a concise summary of what needs reviewing:

```
## Plan Review Required

**Feature**: {feature name from spec.md}
**Spec**: {FEATURE_DIR}/spec.md
**Plan**: {FEATURE_DIR}/plan.md

### Key Decisions to Review
- **Tech stack**: {extract from plan.md}
- **Data model**: {summarise entities if data-model.md exists}
- **User stories**: {list story titles and priorities from spec.md}
- **Open risks or trade-offs**: {extract any noted risks from plan.md}
```

Keep the summary brief — the reviewer will read the full documents. Focus on highlighting decisions that benefit most from a second opinion.

### 3. Request Review

Present the user with clear options:

```
## How to Get This Reviewed

1. **Share directly** — send the spec.md and plan.md file paths to a colleague for review
2. **Create an MR** — commit the spec artifacts and open a merge request for review
3. **Pair review** — walk through the plan with a teammate in this session

When the review is complete, confirm below to proceed to `/speckit.tasks`.
```

### 4. Wait for Confirmation

Ask the user:

```
Has this plan been reviewed? (yes/skip)
- **yes** — plan has been reviewed, proceed to task generation
- **skip** — skip review for now (not recommended for production features)
```

**If the user confirms "yes"**: Report success and suggest running `/speckit.tasks`.

**If the user says "skip"**: Warn that skipping review increases the risk of rework, then allow proceeding. Add a note to the plan.md frontmatter or a comment at the top:

```
<!-- REVIEW STATUS: Skipped — plan was not reviewed before task generation -->
```

**If the user provides reviewer feedback or changes**: Help incorporate the feedback into spec.md and/or plan.md, then re-present the summary for confirmation.

## Key Rules

- Do NOT proceed to task generation automatically — this is a gate
- Do NOT generate tasks within this command
- Keep the summary focused on decisions, not implementation details
- The goal is to catch misunderstandings and bad assumptions early, before tasks are generated
