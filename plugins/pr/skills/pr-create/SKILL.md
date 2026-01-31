---
description: Create a pull request from the current branch
user-invocable: true
---

Create a pull request for the current branch.

## Steps

1. Run `git status` and `git diff` to understand the current changes
2. Run `git log` and `git diff main...HEAD` (or the appropriate base branch) to understand all commits on this branch
3. Analyze the changes and draft a clear, concise PR title (under 70 characters) and description
4. Push the branch to the remote if not already pushed
5. Create the PR using `gh pr create` with the following format:

```
gh pr create --title "the pr title" --body "$(cat <<'EOF'
## Summary
<1-3 bullet points summarizing the changes>

## Test plan
- [ ] <testing checklist items>
EOF
)"
```

6. Return the PR URL to the user

## Guidelines

- Keep the PR title short and descriptive
- Focus the summary on **why** the changes were made, not just **what** changed
- Include a practical test plan with actionable checklist items
- If there are no changes to push, inform the user
- Do not force push unless explicitly asked
