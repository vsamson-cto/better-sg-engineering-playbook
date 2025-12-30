# Code Review Guide

**Versioning**: v1.0 | **Status**: Active | **Audience**: All Engineers

Code review is where we learn from each other, catch bugs early, and maintain code quality. It's a collaboration, not a judgment.

## Why Code Review Matters

**MUST** do code reviews because:
- Fresh eyes catch bugs the author missed
- Spreads knowledge across the team
- Enforces consistency in our standards
- Opportunity to mentor and learn

**SHOULD** aim for these in reviews:
- Respectful, constructive feedback
- Focus on code, not the person
- Celebrate good solutions

**NICE-TO-HAVE** additions:
- Mentoring junior engineers through reviews
- Sharing knowledge about domain-specific patterns

---

## 1. The Reviewer's Mindset

### Be Kind, Be Thorough

Remember: The code author worked hard on this. Your job is to make it better, not to show how smart you are.

**DO:**
- "This logic seems complex. Could we simplify it by...?"
- "Great error handling here! One edge case: what if...?"
- "I'm not familiar with this pattern. Can you explain?"

**AVOID:**
- "This is wrong."
- "Obviously, you should..."
- "Why would you ever do this?"

### Differentiate Between Blocking & Suggestions

**Blocking Issues** (MUST fix):
- Security vulnerabilities
- Data integrity risks
- Breaking existing tests
- Hardcoded secrets/credentials

**Suggestions** (SHOULD fix):
- Style inconsistencies
- Potential performance issues
- Missing tests
- Better naming ideas

**Nice-to-haves** (NICE-TO-HAVE):
- Refactoring opportunities for future
- Additional documentation
- Code comments for edge cases

---

## 2. What to Check

### Code Correctness

- [ ] Does the code do what the PR description says?
- [ ] Are there obvious bugs or logic errors?
- [ ] Are error cases handled?
- [ ] Are edge cases covered?
- [ ] Do the tests pass?

### Code Quality

- [ ] Is the code readable?
- [ ] Are variable names clear?
- [ ] Are functions appropriately sized?
- [ ] Is there unnecessary duplication (DRY)?
- [ ] Are comments clear and necessary?

### Security & Data Protection

- [ ] No hardcoded secrets (API keys, passwords)
- [ ] Proper input validation
- [ ] No SQL injection or XSS vulnerabilities
- [ ] Sensitive data not logged
- [ ] Authentication/authorization checks present

### Tests

- [ ] New code has corresponding tests
- [ ] Tests cover happy path AND error cases
- [ ] Tests are understandable
- [ ] No commented-out tests

### Documentation

- [ ] README updated if needed
- [ ] Docstrings for public functions
- [ ] Complex logic has comments explaining "why"

---

## 3. Giving Feedback

### Structure Your Comments

```
[Type] [What I observed] [Why it matters] [Suggestion]
```

**Example:**
```
🔴 BLOCKING: SQL query is vulnerable to injection. 
User input is directly interpolated without parameterization. 
This opens us to data loss or theft.

Suggestion: Use parameterized queries:
  cursor.execute("SELECT * FROM users WHERE id = ?", (user_id,))
```

**Example:**
```
💡 SUGGESTION: This function is doing 3 things (validate, transform, save). 
It's hard to test in isolation. 
Consider splitting into separate functions.
```

### Use Clear Labels

- 🔴 **BLOCKING**: Must fix before merge
- 🟡 **SHOULD**: Should fix, but not a deal-breaker
- 💡 **SUGGESTION**: Nice to address, but optional
- ✅ **PRAISE**: Highlight good code

### Be Specific

**BAD:**
- "This is confusing"
- "Add tests"
- "Bad variable names"

**GOOD:**
- "The logic on line 45 mixes validation and transformation, making it hard to test independently"
- "Missing test for the error case when `user.email` is null"
- `invoice_number` is clearer than `num` since this isn't just any number

---

## 4. Handling Feedback as an Author

### Stay Calm

Code review isn't personal. The reviewer is improving your code, not attacking you.

### Respond to Every Comment

- **If you agree**: "Fixed in commit abc123"
- **If you disagree respectfully**: "I see your point, but I chose X because... [your reasoning]"
- **If you need clarification**: "I'm not sure I understand. Can you clarify what you mean?"

### Don't Be Defensive

**BAD:**
- "It works fine."
- "This is how I always do it."
- "Stop micromanaging."

**GOOD:**
- "You're right, that's clearer. Updated."
- "I hadn't thought of that edge case. Let me fix it."
- "Disagree, here's why..."

---

## 5. The Review Process

### Step 1: Request Review

When creating a PR:
- [ ] Assign 1-2 reviewers (ideally experienced people)
- [ ] Write a clear PR description: what changed and why
- [ ] Link any related issues
- [ ] Set labels (bugfix, feature, docs, etc.)

### Step 2: Respond Promptly

**SHOULD**: Review PRs within 24 hours

**WHY?** Developers can't move forward, and the longer a PR sits, the harder it is to get back into context.

### Step 3: Iterate

Reviewer requests changes → Author makes fixes → Reviewer re-reviews.

**Keep iterating until everyone is satisfied.**

### Step 4: Approve & Merge

- **Approving reviewer** leaves an "Approve" review (don't just leave a comment)
- **Author** merges their own PR (don't approve your own PR)
- **Delete the branch** after merging to keep the repo clean

---

## 6. Code Review SLA

| PR Priority | Review SLA | Merge SLA |
|-------------|-----------|----------|
| 🔴 Hotfix/Critical Bug | 1-2 hours | Immediately after approval |
| 🟡 Feature | 24 hours | After 24-48 hour review window |
| 💚 Non-blocking (docs, tests) | 3-5 days | Flexible |

---

## 7. Red Flags (Escalate to Tech Lead)

If you see these, ask for a tech lead review:

- Architecture changes
- New external dependencies
- Significant refactoring
- Security-sensitive changes
- Changes affecting multiple systems

---

## 8. Common Anti-Patterns to Avoid

### Rubber-Stamping

**BAD:** Glancing at code and approving without really reading it.

**WHY IT'S DANGEROUS:** You become responsible for bugs in code you approved.

### Perfectionism

**BAD:** Blocking a PR because the code isn't "perfect."

**REMEMBER:** Perfect is the enemy of done. A working solution is better than a perfect one that never ships.

### Ignoring Tests

**BAD:** Only reading code, not running tests.

**FIX:** Checkout the branch locally and run the tests.

---

## 9. Tools & Setup

### GitHub Configuration

- Require at least 1 code review before merge
- Dismiss stale reviews when new commits are pushed
- Require status checks to pass before merge

### Review Checklist Template

Add a PR template to `.github/pull_request_template.md`:

```markdown
## Description
Briefly describe what this PR does.

## Testing
How did you test this?

## Checklist
- [ ] Tests added/updated
- [ ] Documentation updated
- [ ] No hardcoded secrets
- [ ] Code follows our standards
```

---

## Quick Checklist: Before Requesting Review

- [ ] Code passes all tests locally
- [ ] Code follows our style standards
- [ ] No commented-out code
- [ ] No debugging logs left in
- [ ] No hardcoded credentials
- [ ] PR description is clear
- [ ] Related issues are linked

---

## Questions?

Code review is a skill that improves with practice. If you're unsure, ask senior engineers or your tech lead. We're here to help.
