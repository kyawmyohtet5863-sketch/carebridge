# Git Workflow

## Protected branches

### `main`

- Stable and demonstration-ready
- Pull requests required
- Direct pushes disabled
- CI checks required
- Release tags created from this branch

### `develop`

- Integration branch
- Pull requests required
- Feature branches merge here
- Must remain deployable to the shared development environment

## Feature lifecycle

```text
develop → feature branch → pull request → develop → milestone pull request → main
```

## Recommended branch protection

- Require at least one approval.
- Dismiss stale approval after new commits.
- Require resolved conversations.
- Require status checks.
- Prevent force pushes and deletion.
- Require linear history when squash merging is used.
