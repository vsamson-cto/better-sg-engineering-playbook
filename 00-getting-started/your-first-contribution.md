# Your First Contribution 🎯

Welcome! This guide will walk you through making your first pull request (PR) to a Better.sg project. Don't worry if you're new to this — we'll go step-by-step.

## What You'll Learn

- How to fork and clone a repository
- How to create a branch and make changes
- How to commit your work
- How to open a pull request
- How to respond to code review feedback

## Prerequisites

Before you start, make sure you've completed:

- [ ] [Onboarding Checklist](./onboarding-checklist.md)
- [ ] You have Git installed and configured
- [ ] You have access to the project repository
- [ ] You have a GitHub account with 2FA enabled

## The Big Picture

Here's the workflow you'll follow:

```
1. Fork/Clone the repo → Get a copy on your machine
2. Create a branch → Work in isolation
3. Make changes → Write your code
4. Commit → Save your work
5. Push → Send to GitHub
6. Open PR → Request to merge
7. Review → Get feedback
8. Merge → Your code goes live! 🎉
```

---

## Step 1: Find Something to Work On

### Option A: Pick a "Good First Issue"

1. Go to the project repository on GitHub
2. Click **Issues** tab
3. Look for labels like:
   - `good first issue`
   - `help wanted`
   - `beginner-friendly`
4. Comment on the issue: "I'd like to work on this!"

### Option B: Make a Simple Documentation Fix

Perfect for your first PR:
- Fix a typo
- Improve unclear wording
- Add missing examples
- Update outdated links

**Pro Tip**: Start small! Your first PR doesn't need to be complex.

---

## Step 2: Set Up Your Local Repository

### If You Don't Have Write Access (Fork Method)

1. **Fork the repository**
   - Go to the project repository on GitHub
   - Click the **Fork** button (top right)
   - This creates your own copy

2. **Clone your fork**
   ```bash
   git clone https://github.com/YOUR-USERNAME/project-name.git
   cd project-name
   ```

3. **Add upstream remote**
   ```bash
   git remote add upstream https://github.com/better-sg/project-name.git
   git remote -v  # Verify it worked
   ```

### If You Have Write Access (Direct Method)

1. **Clone the repository**
   ```bash
   git clone https://github.com/better-sg/project-name.git
   cd project-name
   ```

2. **Pull latest changes**
   ```bash
   git pull origin main
   ```

---

## Step 3: Create a Branch

**Never work directly on `main`!** Always create a branch.

```bash
# Create and switch to a new branch
git checkout -b feature/your-feature-name

# Or for a bug fix
git checkout -b fix/description-of-fix
```

### Branch Naming Convention

- **Features**: `feature/add-user-profile`
- **Bug Fixes**: `fix/login-button-crash`
- **Documentation**: `docs/update-readme`
- **Refactoring**: `refactor/simplify-auth`

---

## Step 4: Make Your Changes

1. **Open the project in your code editor**
   ```bash
   code .  # For VS Code
   ```

2. **Make your changes**
   - Write code
   - Fix the bug
   - Update documentation

3. **Test your changes locally**
   ```bash
   # Run tests (if the project has them)
   npm test
   # OR
   pytest
   # OR
   python -m unittest
   ```

4. **Check code quality**
   ```bash
   # Lint your code (if applicable)
   npm run lint
   # OR
   flake8
   ```

---

## Step 5: Commit Your Changes

### Stage Your Changes

```bash
# See what you changed
git status

# Add specific files
git add path/to/file.py

# Or add everything (be careful!)
git add .
```

### Write a Good Commit Message

```bash
git commit -m "fix: resolve login button not appearing on mobile"
```

#### Commit Message Format

```
<type>: <short description>

<optional longer description>
```

**Types**:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code formatting (no logic change)
- `refactor`: Code restructuring
- `test`: Adding tests
- `chore`: Maintenance tasks

**Examples**:
```bash
✅ "feat: add user profile page"
✅ "fix: resolve image upload error for files > 5MB"
✅ "docs: update installation instructions"

❌ "updated stuff"
❌ "fix"
❌ "changes"
```

---

## Step 6: Push Your Changes

```bash
# Push your branch to GitHub
git push origin feature/your-feature-name
```

If it's your first time pushing this branch:
```bash
git push -u origin feature/your-feature-name
```

---

## Step 7: Open a Pull Request

1. **Go to GitHub**
   - Navigate to the project repository
   - You should see a banner: "Compare & Pull Request"
   - Click it!

2. **Fill out the PR template**

   ```markdown
   ## Description
   Brief summary of what this PR does.

   ## Why?
   Explain why this change is needed.

   ## How?
   - Changed X in file Y
   - Added validation for Z
   - Updated documentation

   ## Testing
   - [ ] Tested locally
   - [ ] All tests pass
   - [ ] No console errors

   ## Screenshots (if applicable)
   Attach before/after images

   ## Checklist
   - [ ] Code follows project standards
   - [ ] Tests added/updated
   - [ ] Documentation updated
   - [ ] No hardcoded secrets
   ```

3. **Link related issues**
   ```
   Fixes #123
   Related to #456
   ```

4. **Request reviewers**
   - Add 1-2 reviewers from the team
   - If you're not sure who, ask in `#engineering`

5. **Click "Create Pull Request"**

---

## Step 8: Respond to Feedback

Your PR will be reviewed by teammates. Here's what to expect:

### Types of Feedback

1. **Questions**: Reviewers want to understand your approach
2. **Suggestions**: Ideas for improvement
3. **Required Changes**: Issues that must be fixed
4. **Nitpicks**: Minor style preferences

### How to Respond

**If you agree with the feedback:**
```markdown
Good point! I'll fix that.
```
Then make the changes and push again.

**If you need clarification:**
```markdown
I'm not sure I understand. Could you explain what you mean by "refactor this"?
```

**If you disagree:**
```markdown
I considered that approach, but went with X because Y. What do you think?
```

### Making Changes After Review

1. **Make the requested changes**
   ```bash
   # Edit your files
   # ...

   # Commit the changes
   git add .
   git commit -m "fix: address review feedback on validation"

   # Push to the same branch
   git push
   ```

2. **Respond to each comment**
   - Let reviewers know you fixed it
   - Link to the commit if helpful

3. **Re-request review**
   - Click "Re-request review" button on GitHub

---

## Step 9: Merge Your PR

### When Your PR is Approved

1. **Check CI/CD status**
   - All checks must be green ✅
   - Tests passing
   - No conflicts with `main`

2. **You (the author) should merge**
   - Click **"Squash and merge"** or **"Merge pull request"**
   - Follow the project's merge strategy

3. **Delete the branch**
   - GitHub will prompt you
   - Click "Delete branch"

4. **Update your local repository**
   ```bash
   git checkout main
   git pull origin main
   git branch -d feature/your-feature-name  # Delete local branch
   ```

### Celebrate! 🎉

You just shipped your first contribution to Better.sg!

---

## Common Issues & Solutions

### Issue: "Your branch is behind 'origin/main'"

**Solution**: Update your branch
```bash
git checkout main
git pull origin main
git checkout feature/your-feature
git merge main
```

### Issue: "Merge conflicts"

**Solution**: Resolve conflicts
```bash
git checkout main
git pull origin main
git checkout feature/your-feature
git merge main
# Fix conflicts in your editor
git add .
git commit -m "fix: resolve merge conflicts"
git push
```

### Issue: "I committed to `main` by mistake!"

**Solution**: Move changes to a new branch
```bash
git branch feature/your-feature  # Create branch with current changes
git checkout main
git reset --hard origin/main  # Reset main to match remote
git checkout feature/your-feature  # Go back to your work
```

### Issue: "I want to undo my last commit"

**Solution**: Undo commit but keep changes
```bash
git reset --soft HEAD~1
```

---

## Quick Reference Cheat Sheet

```bash
# Setup
git clone <repo-url>
git checkout -b feature/my-feature

# Work
# ... make changes ...
git status
git add .
git commit -m "feat: description"

# Sync
git pull origin main
git push origin feature/my-feature

# Cleanup
git checkout main
git pull
git branch -d feature/my-feature
```

---

## Next Steps

After your first PR:

1. **Pick another issue** - Keep contributing!
2. **Review others' PRs** - Learn by reading code
3. **Ask questions** - We're here to help
4. **Read more docs**:
   - [Pull Request Process](../04-version-control/pull-request-process.md)
   - [Coding Standards](../02-development-practices/coding-standards.md)
   - [Code Review Guidelines](../04-version-control/code-review-guidelines.md)

---

## Getting Help

**Stuck? That's normal!**

- **Slack**: Ask in `#engineering` channel
- **GitHub**: Comment on your PR or issue
- **Documentation**: Check project README
- **Teammates**: Reach out directly

**Remember**: Every engineer was a beginner once. We're here to support you!

---

## Additional Resources

- [GitHub Flow Guide](https://guides.github.com/introduction/flow/)
- [Git Basics](https://git-scm.com/book/en/v2/Getting-Started-Git-Basics)
- [Writing Good Commit Messages](https://chris.beams.io/posts/git-commit/)
- [Better.sg Engineering Playbook](../README.md)

---

**Ready to start?** Pick an issue and let's go! 🚀

Questions? Ask in `#engineering` on Slack.
