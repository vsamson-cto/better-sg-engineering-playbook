# Security & Compliance

Protecting user data and maintaining system security is everyone's responsibility at Better.sg.

## Quick Links

- **[Security Fundamentals](security-fundamentals.md)** — Non-negotiable rules
- **[Secrets Management](secrets-management.md)** — Handling credentials safely
- **[Dependency Management](dependency-management.md)** — Managing third-party libraries
- **[Data Protection](data-protection.md)** — PDPA compliance
- **[Vulnerability Response](vulnerability-response.md)** — Found a security issue?
- **[AI Code Security](ai-code-security.md)** — Special security considerations

## Golden Rules

### 1. **No Hardcoded Secrets**
Never commit passwords, API keys, or tokens to code. EVER.

### 2. **Assume All Input is Hostile**
Validate and sanitize every user input. Don't trust anything from the outside.

### 3. **Least Privilege**
Users should only have permissions they actually need.

### 4. **Encrypt Sensitive Data**
In transit (HTTPS/TLS) and at rest (database encryption).

### 5. **Audit Everything**
Log security-relevant actions for accountability and debugging.

### 6. **Report Issues Immediately**
No blame. Just report, fix, and learn.

## Security Checklist for Every Change

- [ ] No secrets in code (keys, passwords, tokens)
- [ ] Input validation on all user-facing inputs
- [ ] Output encoding prevents injection attacks
- [ ] Authentication required for sensitive operations
- [ ] Authorization checked (users only do what they should)
- [ ] Error messages don't leak sensitive info
- [ ] Logging doesn't include sensitive data
- [ ] Dependencies are current and vulnerability-free
- [ ] Tests cover security scenarios

## When to Ask For Help

- **Unsure about security decision?** Ask in #security on Slack
- **Found a vulnerability?** See [Vulnerability Response](vulnerability-response.md)
- **Need to store sensitive data?** Discuss with team leads
- **Using new third-party service?** Check security implications

---

**Security is not an afterthought. It's built in from day one.**
