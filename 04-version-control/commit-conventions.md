# Commit Conventions

**Versioning**: v1.0 | **Status**: Active | **Audience**: All Engineers

> **Clear commits = clear history**: Your commit messages tell the story of your code.

Good commit messages help everyone understand what changed, why it changed, and how it affects the project. This guide covers our commit message format and best practices.

---

## Why Good Commit Messages Matter

Commit messages help:

- **Future you** understand what you did 6 months ago
- **Your teammates** review your code faster
- **Project managers** track progress
- **Bug hunters** find when something broke
- **Release managers** generate changelogs

---

## Commit Message Format

### Basic Structure

```
<type>: <short summary>

<optional body>

<optional footer>
```

### Example

```
feat: Add user authentication with JWT

Implements login and logout endpoints using JSON Web Tokens.
Users can now authenticate with email/password and receive
a token valid for 24 hours.

Fixes #45
Closes #52
```

---

## Commit Types

Use these prefixes to categorize your commits:

| Type | When to Use | Example |
|------|-------------|----------|
| `feat` | New feature | `feat: Add payment gateway integration` |
| `fix` | Bug fix | `fix: Resolve login button not responding on mobile` |
| `docs` | Documentation | `docs: Update API documentation` |
| `style` | Code formatting (no logic change) | `style: Fix indentation in auth module` |
| `refactor` | Code restructuring (no behavior change) | `refactor: Simplify user validation logic` |
| `perf` | Performance improvement | `perf: Optimize database query in user search` |
| `test` | Adding or updating tests | `test: Add unit tests for payment service` |
| `chore` | Maintenance tasks | `chore: Update dependencies to latest versions` |
| `ci` | CI/CD changes | `ci: Add GitHub Actions workflow for testing` |
| `build` | Build system changes | `build: Update webpack configuration` |
| `revert` | Reverting previous commit | `revert: Revert "feat: Add beta feature"` |

---

## Writing the Summary Line

### Rules

1. **Keep it short**: 50 characters max
2. **Use imperative mood**: "Add" not "Added" or "Adds"
3. **No period at the end**
4. **Be specific**: Explain WHAT changed
5. **Start with lowercase** after the type prefix

### Good Examples

✅ `feat: Add user profile page`
✅ `fix: Resolve crash when uploading large files`
✅ `docs: Add setup instructions to README`
✅ `refactor: Extract validation logic into helper`

### Bad Examples

❌ `feat: Added new stuff` (vague)
❌ `fix: Fixed bug` (not descriptive)
❌ `Updated files.` (no type prefix)
❌ `feat: Add user profile page.` (has period)
❌ `feat: Adding user profile page` (wrong tense)

---

## Writing the Body (Optional)

### When to Include a Body

- **Complex changes** that need explanation
- **Context** about why the change was made
- **Trade-offs** or alternative approaches considered
- **Breaking changes** that affect existing functionality

### Body Guidelines

- **Wrap at 72 characters** per line
- **Explain WHY**, not WHAT (the code shows what)
- **Use bullet points** for multiple points
- **Leave a blank line** after the summary

### Example with Body

```
feat: Add Redis caching for user sessions

Previously, user session data was stored in PostgreSQL,
causing slow response times during peak traffic. This
change introduces Redis caching to reduce database load
and improve performance.

Measured improvements:
- Login response time: 450ms → 80ms
- Session validation: 200ms → 15ms

Related to #78
```

---

## Footer (Optional)

### Linking to Issues

Use the footer to reference or close issues:

```
Fixes #123
Closes #456, #789
Related to #42
See also #100
```

### Keywords that Close Issues

- `Fixes #123` - Closes issue when merged to main
- `Closes #123` - Same as Fixes
- `Resolves #123` - Same as Fixes

Multiple issues:
```
Fixes #123, #456
```

### Breaking Changes

If your commit introduces breaking changes:

```
feat: Change API response format to JSON:API spec

BREAKING CHANGE: All API endpoints now return JSON:API
formatted responses instead of plain JSON. Clients must
update to parse the new format.

Migration guide: docs/migration-v2.md
```

---

## Examples

### Simple Feature

```
feat: Add dark mode toggle
```

### Bug Fix with Context

```
fix: Prevent duplicate email registrations

The registration form was allowing users to sign up with
the same email multiple times due to missing uniqueness
validation. Added unique constraint to email field and
proper error handling.

Fixes #234
```

### Documentation Update

```
docs: Update deployment guide with Docker instructions

Added step-by-step guide for deploying with Docker
Compose, including environment variable configuration
and common troubleshooting tips.
```

### Refactoring

```
refactor: Extract authentication logic into middleware

Auth logic was duplicated across multiple route handlers.
Extracted into reusable middleware to improve code
maintainability and reduce duplication.
```

### Performance Improvement

```
perf: Lazy load images on homepage

Reduced initial page load time by implementing lazy
loading for product images. Images now load only when
they enter the viewport.

Before: 3.2s load time
After: 1.1s load time
```

### Chore

```
chore: Update Node.js to v20 LTS

Upgraded Node.js from v18 to v20 for latest security
patches and performance improvements. Updated CI/CD
pipelines accordingly.
```

---

## Commit Best Practices

### ✅ Do This

- **One logical change per commit**
  - If you can't describe it in one sentence, split it
- **Commit early, commit often**
  - Don't wait until you've written 1000 lines
- **Write in imperative mood**
  - "Add feature" not "Added feature"
- **Reference issues**
  - Link to relevant GitHub issues
- **Test before committing**
  - Make sure your code works
- **Use present tense**
  - "Change" not "Changed"

### ❌ Don't Do This

- **Don't commit broken code**
  - Every commit should compile/run
- **Don't use vague messages**
  - "Updated stuff" tells us nothing
- **Don't commit commented-out code**
  - Git keeps history, delete it
- **Don't commit secrets**
  - API keys, passwords, tokens
- **Don't mix refactoring and features**
  - Separate into different commits
- **Don't commit large files**
  - Use Git LFS or cloud storage

---

## Atomic Commits

### What Are Atomic Commits?

Each commit should be **self-contained** and **independent**:

- Can be reverted without breaking other changes
- Passes all tests on its own
- Makes one logical change

### Example: Good Atomic Commits

```
Commit 1: feat: Add database migration for user roles
Commit 2: feat: Implement role-based access control
Commit 3: test: Add tests for RBAC
Commit 4: docs: Document role permissions
```

### Example: Bad Non-Atomic Commit

```
Commit 1: Add RBAC, fix login bug, update docs, refactor auth
```

This should be **4 separate commits**.

---

## Commit Frequency

### How Often to Commit?

- **Feature work**: Commit every 30-60 minutes
- **Bug fixes**: Commit when the bug is fixed
- **Refactoring**: Commit after each logical step
- **Documentation**: Commit after each section

### Signs You Should Commit

✅ You just made a test pass
✅ You completed a small feature
✅ You're about to try something risky
✅ You're switching tasks
✅ You've been coding for an hour

---

## Amending Commits

### Fix Your Last Commit

If you forgot something in your last commit:

```bash
git add forgotten-file.js
git commit --amend
```

**Warning**: Only amend commits that haven't been pushed.

### Changing Commit Messages

```bash
git commit --amend -m "New commit message"
```

---

## Common Mistakes

### Mistake 1: Vague Messages

❌ `git commit -m "Fixed stuff"`
✅ `git commit -m "fix: Resolve null pointer in user service"`

### Mistake 2: Too Much in One Commit

❌ `git commit -m "Add feature, fix bugs, update docs"`
✅ Separate into 3 commits

### Mistake 3: Wrong Tense

❌ `git commit -m "feat: Added user login"`
✅ `git commit -m "feat: Add user login"`

### Mistake 4: No Context

❌ `git commit -m "Update code"`
✅ `git commit -m "refactor: Simplify payment processing logic"`

---

## Pre-Commit Checklist

Before committing, verify:

- [ ] Code compiles/runs without errors
- [ ] Tests pass locally
- [ ] No console errors or warnings
- [ ] Commit message follows conventions
- [ ] Commit is atomic (one logical change)
- [ ] No secrets or sensitive data
- [ ] Code is formatted properly
- [ ] Changes are related to the commit message

---

## Tools

### Commitlint

Automatically enforce commit message format:

```bash
npm install --save-dev @commitlint/cli @commitlint/config-conventional
```

### Commitizen

Interactive commit message builder:

```bash
npm install -g commitizen
git cz
```

---

## Questions?

Not sure how to phrase your commit? Ask in `#engineering-chat`.

Remember: **A good commit message is a gift to your future self.**
