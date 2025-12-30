# Pull Request Process

**Versioning**: v1.0 | **Status**: Active | **Audience**: All Engineers

> **PRs are how we collaborate**: Think of them as a conversation, not a gatekeeper.

A pull request is how you propose changes to our codebase. It's a chance for your teammates to review your work, ask questions, and help make it even better.

## Before Creating a PR

### 1. Make Sure Your Code Works

- [ ] Code runs locally without errors
- [ ] Tests pass locally
- [ ] No console errors or warnings
- [ ] You've tested your changes

### 2. Follow Our Standards

- [ ] Code follows [Coding Standards](../02-development-practices/coding-standards.md)
- [ ] Tests cover your changes
- [ ] No hardcoded secrets
- [ ] Commit messages are clear

### 3. Keep It Focused

One PR should do ONE thing. If you're fixing a bug AND adding a feature, make two PRs.

```
✅ Good: "Fix login button not appearing on mobile"
❌ Bad: "Fix login, add new admin dashboard, refactor auth, update docs"
```

## Creating a PR

### Step 1: Push Your Branch

```bash
git push origin feature/your-feature-name
```

### Step 2: Open the PR

GitHub will usually show a "Compare & Pull Request" button. Click it.

### Step 3: Fill Out the PR Template

Use our PR template to describe:

**Description**
```
What does this PR do?
Why does it need to be done?
How does it solve the problem?
```

**Changes**
```
- Changed X from A to B
- Added Y functionality
- Fixed Z bug
```

**Testing**
```
How did you test this?
What should reviewers test?
```

**Screenshots** (if relevant)
```
Attach before/after screenshots
```

**Checklist**
```
- [ ] Tests pass locally
- [ ] No hardcoded secrets
- [ ] Code follows standards
- [ ] Documentation updated
```

### Step 4: Link Related Issues

```
Fixes #123
Related to #456
```

## What Happens During Review

### Code Review

Reviewers will:
- Read your code
- Ask clarifying questions
- Suggest improvements
- Look for bugs or security issues

**This is NOT a judgment. It's collaboration.**

### Addressing Feedback

When a reviewer requests changes:

1. **Read the feedback** - Understand what they're asking
2. **Ask questions** - If you don't understand, ask
3. **Make the changes** - Implement the improvements
4. **Push again** - Just commit and push normally
5. **Respond to the comment** - Let them know you fixed it

```
Reviewer: "This function is too complex"

You: "Good point! I've split it into three smaller functions.
See commits a1b2c3 and d4e5f6."
```

### Approval

Once reviewers approve:
1. Your PR is ready to merge
2. You (the author) should merge it
3. Don't approve your own PR
4. Delete the branch after merging

## PR Checklist

Before submitting your PR:

- [ ] Branch name is descriptive: `feature/description` or `fix/description`
- [ ] Commit messages are clear and concise
- [ ] Code is formatted correctly
- [ ] Tests pass locally: `pytest` or `npm test`
- [ ] No console errors
- [ ] PR description is clear
- [ ] Screenshot/demo included (if UI change)
- [ ] No hardcoded secrets
- [ ] Documentation updated
- [ ] Addressed any known TODOs

## PR Examples

### Good PR Description

```markdown
## Description
Fixes the bug where users couldn't upload images larger than 5MB.

## Why?
The upload endpoint was rejecting files > 5MB even though the S3 bucket
allows up to 100MB. Users with high-resolution photos were frustrated.

## How?
- Removed the hardcoded 5MB limit in upload-handler.py
- Added validation against actual S3 bucket limits
- Added test for large file uploads

## Testing
✅ Tested locally with 10MB, 50MB, and 100MB files
✅ All existing tests pass
✅ New test added for edge case

Fixes #342
```

### Helpful PR Comment

```
Great work! A few suggestions:

1. The loop on line 45 could be simplified using a list comprehension
2. Missing error handling for the case when the API is down
3. Consider extracting the validation logic into a separate function

What do you think?
```

## Common PR Mistakes

### Mistake 1: "WIP" PRs

❌ Opening a PR that's not ready
✅ Only open when it's ready for review

If you need feedback on work-in-progress:
- Mark it as "Draft" in GitHub
- Ask for feedback in Slack instead

### Mistake 2: Huge PRs

❌ 500 lines of changes
✅ Break into smaller, focused PRs

Large PRs are hard to review and likely to have bugs.

### Mistake 3: Ignoring Feedback

❌ Merging without addressing comments
✅ Respond to every comment

Even if you disagree, explain why.

### Mistake 4: No Tests

❌ "I tested it manually"
✅ Write automated tests

Manual testing is easy to miss edge cases.

## Merging

### Who Merges?

**You** (the PR author) should merge your own PR after approval.

Don't let reviewers merge - they should only review.

### After Merging

1. **Delete the branch** - Keep the repo clean
2. **Close related issues** - If it fixes something
3. **Celebrate** - You shipped code! 🎉

## SLA: How Fast Should This Be?

| PR Type | Review Time | Merge Time |
|---------|-------------|------------|
| Hotfix/Critical Bug | 1-2 hours | Immediate after approval |
| Feature | 24 hours | After 24-48 hour review window |
| Docs/Tests | 3-5 days | Flexible |

## Questions?

Not sure about your PR? Ask in `#engineering-chat`.

Remember: We're all learning. No PR is perfect.
