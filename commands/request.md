---
description: "Create a merge request for spec.md and plan.md review before proceeding to task generation"
---

# Plan Review Gate

Gate task generation behind a code review. After `/speckit.plan` completes, this command commits the spec artifacts and creates a merge request (or pull request) so a teammate can review the plan before implementation begins.

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
- **contracts/** (optional)

If either spec.md or plan.md is missing, ERROR: "Cannot request review — spec.md and plan.md must both exist. Run `/speckit.specify` and `/speckit.plan` first."

### 2. Present Review Summary

Generate a concise summary of what needs reviewing:

```
## Plan Review Required

**Feature**: {feature name from spec.md}
**Artifacts**:
- {FEATURE_DIR}/spec.md
- {FEATURE_DIR}/plan.md
- {list any other artifacts found}

### Key Decisions for Reviewer
- **Tech stack**: {extract from plan.md}
- **Data model**: {summarise entities if data-model.md exists}
- **User stories**: {list story titles and priorities from spec.md}
- **Open risks or trade-offs**: {extract any noted risks from plan.md}
```

### 3. Commit Spec Artifacts

Stage all files in FEATURE_DIR:

```bash
git add {FEATURE_DIR}/
```

Create a commit with a clear message indicating this is a plan for review:

```
{feature-name}: Add spec and plan for review

Spec Kit artifacts for review before task generation.
```

### 4. Create Merge Request / Pull Request

Detect the remote hosting platform:
- **GitLab** (`git remote get-url origin` contains `gitlab`): Create a merge request
- **GitHub** (`git remote get-url origin` contains `github`): Create a pull request

Push the branch and create the MR/PR:

**GitLab:**
```bash
git push -u origin HEAD
glab mr create --title "{feature-name}: Plan review" \
  --description "## Plan Review\n\nPlease review the spec and plan artifacts before task generation begins.\n\n### Artifacts\n- spec.md\n- plan.md\n{other artifacts}\n\n### Key Decisions\n{summary from step 2}" \
  --no-editor
```

**GitHub:**
```bash
git push -u origin HEAD
gh pr create --title "{feature-name}: Plan review" \
  --body "## Plan Review\n\nPlease review the spec and plan artifacts before task generation begins.\n\n### Artifacts\n- spec.md\n- plan.md\n{other artifacts}\n\n### Key Decisions\n{summary from step 2}"
```

If `glab` or `gh` CLI is not available, fall back to using available MCP tools (e.g. GitLab MCP `create_merge_request`).

### 5. Report and Gate

Present the created MR/PR to the user:

```
## MR Created for Plan Review

**MR**: {MR/PR URL}

The spec and plan artifacts are ready for review.
Once the review is complete, run `/speckit.tasks` to generate implementation tasks.
```

**Do NOT proceed to task generation.** The user must explicitly run `/speckit.tasks` in a subsequent step after the review is done.

## Key Rules

- ALWAYS commit and create an MR/PR — this is not optional
- Do NOT proceed to task generation automatically — this is a gate
- Do NOT generate tasks within this command
- Use the project's existing branch naming conventions if detectable
- Keep the MR/PR description focused on decisions that benefit from review
- The goal is to catch misunderstandings and bad assumptions early, before tasks are generated
