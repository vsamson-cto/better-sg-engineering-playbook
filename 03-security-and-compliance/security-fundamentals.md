# Security Fundamentals

**Versioning**: v1.0 | **Status**: Active | **Audience**: All Engineers

Security isn't optional at Better.sg. We handle user data, and that's a responsibility we take seriously. These rules are non-negotiable.

> **Security matters, but we're here to help** 🔒
>
> Yes, security is important. But don't let that intimidate you! Ask questions, flag concerns, and learn together. We've all made security mistakes.


## Why Security Matters

**MUST** follow these security practices because:
- User trust is our most valuable asset
- A single breach can destroy the organization
- Legal/regulatory requirements (Singapore data protection laws)
- Our volunteers deserve protection

**SHOULD** implement:
- Defense in depth (multiple layers of protection)
- Principle of least privilege
- Regular security audits

**NICE-TO-HAVE** additions:
- Penetration testing
- Security training programs
- Bug bounty programs

---

## 1. Core Security Principles

### Principle of Least Privilege (PoLP)

Each user, service, and application should have only the minimum permissions needed to function.

**DO:**
```
Application needs to read user emails: Grant READ permission on /emails only
Not: Grant ADMIN permission to the entire database
```

**AVOID:**
```
Grant root access to everything
Let all services access all data
Give users admin rights by default
```

### Defense in Depth

Don't rely on a single security layer. Use multiple layers:

```
Application Layer   ← Input validation, authentication
Network Layer       ← Firewalls, SSL/TLS encryption
Database Layer      ← Encryption at rest, access controls
Operational Layer   ← Monitoring, logging, incident response
```

### Fail Secure

When something breaks, default to a secure state, not a permissive one.

**GOOD:**
```python
# If we can't verify the token, deny access
if not verify_token(token):
    return HTTPForbidden("Invalid token")
```

**BAD:**
```python
# If verification fails, let them through anyway
try:
    verify_token(token)
except Exception:
    pass  # Continue anyway
```

---

## 2. Data Protection

### Never Log Sensitive Data

**MUST**: Never log passwords, credit cards, API keys, tokens, or personally identifiable information (PII).

**BAD:**
```python
logger.info(f"User login: {email}, password: {password}")
logger.debug(f"Credit card: {card_number}")
```

**GOOD:**
```python
logger.info(f"User login attempt: {email}")
logger.debug(f"Payment processed for user {user_id}")
```

### Encrypt Sensitive Data at Rest

**MUST**: All sensitive data must be encrypted when stored.

- Passwords: Use bcrypt or argon2 (never plaintext, never MD5)
- Credit cards: Tokenize via payment processor, never store raw
- API keys: Encrypt in database, decrypt only when needed
- PII: Encrypt using AES-256

### Encrypt Data in Transit

**MUST**: All data in transit must use TLS/SSL (HTTPS).

- No HTTP for authenticated endpoints
- All API calls: HTTPS only
- Certificate pinning for critical mobile apps

### Data Retention & Deletion

**SHOULD**: Delete data you no longer need.

```python
# Delete old logs after 90 days
if log_entry.created_at < now() - 90.days:
    log_entry.delete()

# Delete user data after account closure
def delete_user_account(user_id):
    user = User.get(user_id)
    # Delete personal data
    user_data.delete()
    # Keep anonymized data for analytics
    analytics.log_anon_event("user_deleted")
```

---

## 3. Authentication & Authorization

### Implement Strong Authentication

**MUST**: Use secure authentication mechanisms:

- OAuth 2.0 for third-party apps
- JWT or session tokens for web apps
- Multi-factor authentication (MFA) for admin accounts
- Never implement custom crypto

**AVOID:**
```python
# Custom authentication is almost always broken
def is_user_valid(password):
    return password == stored_password  # BAD!
```

**DO:**
```python
from argon2 import PasswordHasher

ph = PasswordHasher()
hashed = ph.hash(password)

# Later, to verify:
if ph.verify(hashed, provided_password):
    return True
```

### Implement Authorization Checks

**MUST**: Check permissions before allowing any action.

```python
def delete_user(user_id):
    current_user = get_current_user()
    
    # Authorization check (before deletion)
    if not current_user.is_admin and current_user.id != user_id:
        raise HTTPForbidden("You cannot delete this user")
    
    # Now safe to delete
    User.get(user_id).delete()
```

### Session Management

- Sessions expire after 1 hour of inactivity
- Sessions invalidate on logout
- Require re-authentication for sensitive operations (password change, payment)
- Use secure, httpOnly cookies

```python
response.set_cookie(
    'session_id',
    session_token,
    httponly=True,    # Not accessible to JavaScript
    secure=True,       # HTTPS only
    samesite='Strict', # CSRF protection
    max_age=3600       # 1 hour
)
```

---

## 4. Input Validation & Injection Attacks

### Validate All Inputs

**MUST**: Never trust user input. Validate everything.

**SQL Injection (Prevention):**

```python
# BAD: SQL injection vulnerability
query = f"SELECT * FROM users WHERE email = '{email}'"
db.execute(query)

# GOOD: Parameterized queries
query = "SELECT * FROM users WHERE email = ?"
db.execute(query, (email,))
```

**XSS (Cross-Site Scripting) Prevention:**

```python
# BAD: Unescaped user input in HTML
@app.route('/profile')
def show_profile():
    user_input = request.args.get('name')
    return f"<h1>Hello {user_input}</h1>"  # Vulnerable!

# GOOD: Escape user input
from markupsafe import escape

@app.route('/profile')
def show_profile():
    user_input = request.args.get('name')
    return f"<h1>Hello {escape(user_input)}</h1>"
```

### Use Allowlists, Not Blocklists

```python
# BAD: Trying to block bad input
if '<script>' not in user_input:
    allow_input(user_input)  # Attacker can bypass this

# GOOD: Only allow known good patterns
if re.match(r'^[a-zA-Z0-9_]{3,20}$', username):
    allow_username(username)
```

---

## 5. API Security

### Rate Limiting

**SHOULD**: Implement rate limits to prevent abuse.

```python
# Limit to 100 requests per minute per IP
@app.route('/api/users')
@rate_limit(100, per_minute=True)
def get_users():
    return User.all()
```

### API Keys & Tokens

- Store API keys in environment variables, never in code
- Rotate keys regularly
- Implement token expiration
- Require HTTPS for all API calls

```bash
# .env (never commit this)
API_KEY=sk_live_abcd1234efgh5678

# In code
import os
api_key = os.environ.get('API_KEY')
```

### CORS (Cross-Origin Resource Sharing)

```python
# Be restrictive with CORS
from flask_cors import CORS

CORS(app, resources={
    r"/api/*": {
        "origins": ["https://better.sg"],  # Only our domain
        "methods": ["GET", "POST"],
        "credentials": True
    }
})
```

---

## 6. Secrets Management

**MUST**: Never commit secrets to version control.

**Secrets to protect:**
- Database passwords
- API keys & tokens
- Private encryption keys
- OAuth secrets
- Webhook signing keys

**Management:**

```bash
# Use environment variables locally
export DATABASE_PASSWORD="secure_password"

# In production, use a secrets manager
# AWS Secrets Manager, Google Secret Manager, HashiCorp Vault, etc.
```

**Accidentally committed a secret?**

1. Immediately rotate the credential
2. Remove it from Git history: `git filter-branch` or `bfg`
3. Audit logs for unauthorized access
4. Never just delete the file; the secret is still in history

---

## 7. Dependency Management

### Keep Dependencies Updated

**SHOULD**: Update dependencies regularly and promptly.

```bash
# Check for known vulnerabilities
pip audit
npm audit
bundle audit

# Update regularly
pip install --upgrade pip
npm update
```

### Vet New Dependencies

Before adding a new library:
- Is it actively maintained?
- Does it have known security vulnerabilities?
- Is it from a trusted source?
- What permissions does it request?

### Remove Unused Dependencies

```bash
# Identify unused packages
pip list | grep -v -f <(pip show -f package_name | grep Location)

# Remove them
pip uninstall unused_package
```

---

## 8. Error Handling & Logging

### Don't Leak Error Details

**BAD:**
```python
try:
    result = risky_operation()
except Exception as e:
    return {"error": str(e)}  # Might leak sensitive info
```

**GOOD:**
```python
try:
    result = risky_operation()
except SpecificError as e:
    logger.error(f"Operation failed: {e}")  # Log it
    return {"error": "Operation failed"}     # Generic response
```

### Implement Audit Logging

**SHOULD**: Log security-relevant events:

```python
logger.audit(f"User {user_id} logged in from IP {ip_address}")
logger.audit(f"Admin {admin_id} deleted user {user_id}")
logger.audit(f"Payment processed: ${amount} for user {user_id}")
logger.audit(f"Failed login attempt for {email} from IP {ip_address}")
```

---

## 9. Security Checklist

### Before Deploying

- [ ] No hardcoded secrets in code
- [ ] Input validation on all user inputs
- [ ] Output encoding in templates
- [ ] Authentication checks on all protected endpoints
- [ ] Authorization checks (not just auth)
- [ ] HTTPS enabled everywhere
- [ ] Passwords hashed with bcrypt/argon2
- [ ] Sensitive data encrypted
- [ ] Rate limiting implemented
- [ ] CORS properly configured
- [ ] Dependencies are up-to-date
- [ ] Security headers set (CSP, X-Frame-Options, etc.)
- [ ] Logging doesn't leak PII
- [ ] Error messages don't leak internals
- [ ] Database credentials in environment variables

### Regular Maintenance

- [ ] Review logs for suspicious activity (weekly)
- [ ] Update dependencies (weekly)
- [ ] Audit database permissions (monthly)
- [ ] Review active sessions/tokens (monthly)
- [ ] Security training for team (quarterly)
- [ ] Penetration testing (annually)

---

## 10. If There's a Security Incident

1. **Stop the bleeding**: Disable compromised accounts, revoke tokens
2. **Understand the scope**: What data was accessed? How?
3. **Investigate**: Review logs, find the root cause
4. **Document**: Write a post-mortem
5. **Notify**: Inform affected users if required by law
6. **Improve**: Fix the vulnerability and update playbook

**Report security issues confidentially to: security@better.sg**

---

## Questions?

If you have security concerns or questions, reach out to the security team. It's better to ask than to guess on security.
