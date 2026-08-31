# Contributing to CareBridge

## Branches

- `main`: stable, reviewed, demonstration-ready releases
- `develop`: integration branch for the next release
- `feature/<issue>-<short-name>`: new features
- `fix/<issue>-<short-name>`: non-emergency fixes
- `docs/<issue>-<short-name>`: documentation-only changes

Examples:

```text
feature/12-attendance-entry
feature/18-parent-report-view
fix/31-guardian-access-check
docs/7-api-conventions
```

## Standard workflow

1. Update local `develop`.
2. Create a short-lived feature branch from `develop`.
3. Commit one logical change at a time.
4. Push the branch and open a pull request into `develop`.
5. Complete automated checks and obtain review.
6. Squash-merge after approval.
7. Merge `develop` into `main` only for tested milestones or releases.

## Commit convention

Use Conventional Commit-style messages:

```text
feat(attendance): add daily class entry endpoint
fix(auth): enforce guardian student relationship
test(reports): cover returned to draft transition
docs(api): document pagination response
chore(repo): add development environment
```

## Pull request requirements

- Explain the problem and solution.
- Link the issue or task.
- Identify affected roles and permission boundaries.
- Add or update tests.
- Update API or database documentation when contracts change.
- Include UI evidence for visible changes.
- Confirm that no real student data or secrets are committed.

## Definition of done

A task is done when code is reviewed, tests pass, authorization is tested, documentation is current, migrations are reversible, and the feature works in the shared integration environment.
