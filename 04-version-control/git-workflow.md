# Git Workflow

**Versioning**: v1.0 | **Status**: Active | **Audience**: All Engineers

> **Think of Git as a time machine**: Every change is tracked, and you can always go back.

This guide explains our branching strategy, branch naming conventions, and how to work with Git at Better.sg.

---

## Branching Model

We use a simplified **Git Flow** model with two main branches:

### Main Branches

- **`main`** - Production-ready code. Always stable.
- **`develop`** - Next release. Integration branch for features.

### Supporting Branches

These branches are temporary and should be deleted after merging:

- **`feature/*`** - New features
- **`fix/*`** - Bug fixes
- **`hotfix/*`** - Emergency production fixes
- **`docs/*`** - Documentation updates
- **`chore/*`** - Maintenance tasks (refactoring, dependencies)

```
main (production-ready)
  └─ develop (next release)
     ├─ feature/add-user-auth (new features)
     ├─ fix/login-bug (bug fixes)
     ├─ hotfix/security-patch (urgent fixes)
     └─ docs/update-readme (documentation)
```

---

## Branch Naming Conventions

### Format

```
<type>/<short-description>
```

### Examples

**Features**: `feature/short-description`
- ✅ `feature/add-user-auth`
- ✅ `feature/payment-gateway`
- ❌ `feature/new-stuff` (too vague)
- ❌ `add-user-auth` (missing type)

**Bug Fixes**: `fix/short-description`
- ✅ `fix/login-button-error`
- ✅ `fix/mobile-layout`
- ❌ `fix-bug` (not descriptive)

**Hotfixes**: `hotfix/short-description`
- ✅ `hotfix/security-vulnerability`
- ✅ `hotfix/payment-failure`

**Documentation**: `docs/short-description`
- ✅ `docs/update-api-guide`
- ✅ `docs/add-setup-instructions`

**Chores**: `chore/short-description`
- ✅ `chore/update-dependencies`
- ✅ `chore/refactor-auth-module`

### Rules for Branch Names

- Use **lowercase** only
- Use **hyphens** to separate words, not underscores or spaces
- Keep it **short but descriptive** (3-5 words max)
- No special characters except hyphens

---

## Working with Branches

### Starting New Work

#### 1. Create a Branch from `develop`

```bash
git checkout develop
git pull origin develop
git checkout -b feature/your-feature-name
```

#### 2. Work on Your Branch

Make changes, commit often:

```bash
git add .
git commit -m "Add user authentication logic"
```

#### 3. Push Your Branch

```bash
git push origin feature/your-feature-name
```

#### 4. Open a Pull Request

Go to GitHub and open a PR from your branch to `develop`.

---

### Hotfix Workflow

For **urgent production bugs**, branch from `main`:

```bash
git checkout main
git pull origin main
git checkout -b hotfix/critical-bug
```

After fixing:

1. Open PR to `main`
2. **Also merge back to `develop`** so the fix doesn't get lost

---

## Commit Guidelines

### Good Commits

- **One logical change per commit**
- **Clear, descriptive messages**
- **Reference issues** (e.g., "Fixes #123")
- **Atomic** (can stand alone)

### Commit Message Format

```
<type>: <short description>

<optional detailed explanation>

Fixes #<issue-number>
```

#### Examples

```
feat: Add user authentication with JWT

Implements login/logout endpoints and token validation.
Users can now authenticate via email and password.

Fixes #45
```

```
fix: Resolve mobile layout overflow issue

The navigation bar was extending beyond screen width on mobile.
Applied flexbox layout to fix.

Fixes #67
```

```
docs: Update README with setup instructions

Added step-by-step guide for local development setup.
```

### Commit Types

- `feat` - New feature
- `fix` - Bug fix
- `docs` - Documentation changes
- `style` - Code formatting (no functional changes)
- `refactor` - Code restructuring (no behavior change)
- `test` - Adding or updating tests
- `chore` - Maintenance tasks

---

## Keeping Your Branch Updated

### Sync with `develop` Regularly

If your feature branch is long-lived, keep it updated:

```bash
git checkout develop
git pull origin develop
git checkout feature/your-feature
git merge develop
```

Or use rebase (cleaner history):

```bash
git checkout feature/your-feature
git rebase develop
```

---

## Merge Conflicts

### What Are Conflicts?

Conflicts happen when two branches change the same part of a file.

### Resolving Conflicts

1. **Pull the latest changes**
   ```bash
   git pull origin develop
   ```

2. **Git will mark conflicts** in your files:
   ```
   <<<<<<< HEAD
   Your changes
   =======
   Their changes
   >>>>>>> develop
   ```

3. **Edit the file** to resolve the conflict

4. **Mark as resolved** and commit:
   ```bash
   git add <conflicted-file>
   git commit
   ```

---

## Deleting Branches

### After Merging

Always delete merged branches to keep the repository clean:

```bash
git branch -d feature/your-feature
git push origin --delete feature/your-feature
```

GitHub usually prompts you to delete after merging a PR.

---

## Best Practices

- ✅ **Branch from `develop`** for new features
- ✅ **Branch from `main`** for hotfixes
- ✅ **Keep branches short-lived** (merge within a week)
- ✅ **Pull from `develop` daily** to avoid conflicts
- ✅ **Write clear commit messages**
- ✅ **Delete branches after merging**
- ❌ **Don't commit directly to `main` or `develop`**
- ❌ **Don't push WIP (work-in-progress) commits** without a clear message
- ❌ **Don't keep branches open for months**

---

## Workflow Diagram

```
main (production)
  ├─ hotfix/critical-bug → merge back to main & develop
  └─ develop (staging)
       ├─ feature/add-auth → PR to develop
       ├─ fix/login-bug → PR to develop
       └─ docs/update-readme → PR to develop
```

---

## Common Commands Cheat Sheet

| Task | Command |
|------|----------|
| Create new branch | `git checkout -b feature/name` |
| Switch branch | `git checkout branch-name` |
| See all branches | `git branch -a` |
| Delete local branch | `git branch -d branch-name` |
| Delete remote branch | `git push origin --delete branch-name` |
| Pull latest changes | `git pull origin develop` |
| Push your branch | `git push origin feature/name` |
| See branch status | `git status` |
| View commit history | `git log --oneline` |

---

## Questions?

Not sure which branch to use? Ask in `#engineering-chat`.

Remember: **Git is forgiving. You can always undo.**
