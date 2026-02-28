# Code Review Workflow

After a plan is approved, agents write code and push file diffs back to Weave for human review. Humans review in a read-only Monaco editor with line-specific threaded comments.

## Lifecycle

```
1. Plan approved → Agent writes code
2. push_code_review(files=[...], plan_document_id="<plan-id>", source="backend-dev", group="my-project")
   → Always pass plan_document_id if this code review is linked to a plan
   → Creates code review document, returns code_review_id + URL
3. Share the URL with the user and STOP.
   → Do NOT poll, wait, loop, or use extended thinking while waiting for review.
   → The user will come back and ask you to pull when they are ready.
4. pull_code_feedback(code_review_id="<id-from-step-2>")
   → Always pass code_review_id explicitly — don't rely on session memory
   → Returns review_status + line-specific feedback grouped by file
5. If changes_requested: agent fixes code, pushes updated diffs:
   push_code_review(code_review_id="<id-from-step-2>", files=[...])
   → Updates the code review, creates next version
6. Repeat steps 3-5 until approved
```

## Reading File Contents

When preparing files for `push_code_review`:

- `old_content`: Original file before changes (use empty string `""` for new files)
- `new_content`: File after changes (use empty string `""` for deleted files)
- `status`: `"added"` (new), `"modified"` (changed), `"deleted"` (removed), `"renamed"` (moved)
- Full file contents are required — Monaco needs both sides for side-by-side diff

## Multi-Agent Code Reviews

Multiple agents can push separate code reviews under the same project:

```
backend-dev:  push_code_review(files=[...], source="backend-dev",  group="auth-refactor", plan_document_id="<id>")
frontend-dev: push_code_review(files=[...], source="frontend-dev", group="auth-refactor", plan_document_id="<id>")
```

Each agent gets its own code review document. The human sees all reviews grouped under the project.

## Review Statuses

| Status | Meaning |
|---|---|
| `open` | Awaiting review |
| `approved` | Human approved the changes |
| `changes_requested` | Human wants modifications |
| `rejected` | Human rejected the changes |
