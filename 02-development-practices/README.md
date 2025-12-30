# Development Practices

How we write, test, review, and ship code at Better.sg.

## Quick Links

- **[Coding Standards](coding-standards.md)** — Language-agnostic best practices
- **[AI-Assisted Coding](ai-assisted-coding.md)** — Using Copilot, Claude, etc. safely  
- **[Code Review Guide](code-review-guide.md)** — PR review process
- **[Testing Standards](testing-standards.md)** — Unit, integration, E2E expectations
- **[Documentation](documentation-practices.md)** — Code and technical documentation

## Core Principles

### 1. Security
Assume all inputs are malicious. Validate everything.

### 2. Clarity
Code is read more often than it's written. Write for humans first.

### 3. Testability
If it's hard to test, redesign it. Tests catch bugs early.

### 4. Maintainability
Write for the next person who reads this code (it might be you in 6 months).

### 5. Documentation
Your code should be self-documenting with clear comments on the "why".

## Development Workflow

1. **Create Issue** - What problem are we solving?
2. **Create Branch** - feature/add-auth, fix/login-bug
3. **Write Tests** - TDD where possible
4. **Write Code** - Commit frequently with clear messages
5. **Self Review** - Before requesting review
6. **Code Review** - Get feedback from 2+ people
7. **Tests Pass** - All automated tests must pass
8. **Merge & Deploy** - To staging, then production

## Standards by Section

See the linked sections above for detailed guidelines on:
- Coding standards (language-specific and general)
- AI-assisted development practices
- Code review process
- Testing expectations
- Documentation standards

---

The best code is code that's secure, clear, tested, maintainable, and documented.
