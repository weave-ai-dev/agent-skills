# Code Review Workflow

After a plan is approved, agents write code and push file diffs back to Pylon for human review. Humans review in a read-only Monaco editor with line-specific threaded comments.

## Lifecycle

```
1. Plan approved → Agent writes code
2. push_code_review(files=[...], plan_document_id="<plan-id>", source="backend-dev", group="my-project")
   → Creates code review document, returns code_review_id + URL
3. Human reviews at /code-reviews/{id}
   - Sees side-by-side diffs in Monaco editor (read-only)
   - Selects lines and leaves threaded comments
   - Clicks "Approve" or "Request Changes"
4. pull_code_feedback(code_review_id="<id>")
   → Gets review_status + line-specific feedback grouped by file
5. If changes_requested: agent fixes code, pushes updated diffs:
   push_code_review(code_review_id="<id>", files=[...])
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
