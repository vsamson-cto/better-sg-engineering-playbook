# Version Control & Git Workflow

How we use Git and manage code changes at Better.sg.

## Quick Links

- **[Git Workflow](git-workflow.md)** — Branching strategy & branch naming
- **[Commit Conventions](commit-conventions.md)** — How to write clear commits  
- **[Pull Request Process](pull-request-process.md)** — Making & reviewing PRs
- **[Release Strategy](release-strategy.md)** — Versioning & releases

## Core Concepts

### Branching Model

```
main (production-ready)
  └─ develop (next release)
     ├─ feature/add-user-auth (new features)
     ├─ fix/login-bug (bug fixes)
     └─ docs/update-readme (documentation)
```

### Branch Naming

- **Features**: `feature/short-description`
- **Bugs**: `fix/short-description`
- **Docs**: `docs/short-description`
- **Chores**: `chore/short-description`

### Commit Quality

- One logical change per commit
- Clear, descriptive messages
- References to issues (e.g., "Closes #123")
- Atomic commits (work independently)

### Code Review

- Every change gets reviewed
- Minimum 2 approvals before merge
- Tests must pass
- No hardcoded secrets
- Consider edge cases

### Release Process

1. Create release branch from `develop`
2. Update version numbers (semantic versioning)
3. Update CHANGELOG.md
4. Merge to `main` with release tag
5. Merge back to `develop`

---

See relevant sections for detailed guidelines on each topic.
