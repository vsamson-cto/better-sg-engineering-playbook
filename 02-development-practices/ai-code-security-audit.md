# AI-Generated Code Security Audit Guide

**Inspired by: Lovable Builders Security Review Framework**

## Primary Directive

🚨 **YOU ARE NOT TRUSTING THE AI GENERATOR**

- Do not assume AI-generated code is secure
- Do not assume Firestore RLS is correctly implemented  
- Do not assume authentication logic is reliable
- **Assume nothing — validate everything**

---

## Part 1: Core Security Assessment

### 1. Authentication & Authorization

❌ BAD: `app.get('/api/users', (req, res) => { ... })`
✅ GOOD: `app.get('/api/users', requireAuth, (req, res) => { ... })`

**Audit:**
- All sensitive routes require authentication
- Permission checks always server-side
- User can't access other users' data
- No privilege escalation possible
- Token expiration enforced

### 2. Database + RLS Enforcement

❌ VULNERABLE: `mismatch /users/{userId} { allow read; }`
✅ FIXED: `match /users/{userId} { allow read: if request.auth.uid == userId; }`

**Audit:**
- RLS enabled on ALL sensitive tables
- Policies not overly permissive
- No data inference possible
- Subcollections have RLS rules

### 3. API Input Validation

❌ BAD: `const query = req.body.q;`
✅ GOOD: `if (typeof req.body.q !== 'string') throw Error('Invalid');`

**Audit:**
- Validate input type, length, format
- Sanitize special characters
- Error messages don't leak internals
- No internal endpoints exposed

### 4. Front-End Security

❌ BAD: `document.getElementById('name').innerHTML = userData.name;`
✅ GOOD: `document.getElementById('name').textContent = userData.name;`

**Audit:**
- No `.innerHTML` with user data
- No template injection
- No eval() or Function() with input
- Sensitive data not in LocalStorage

### 5. File & Asset Security

❌ BAD: `app.get('/uploads/:filename', (req, res) => { res.sendFile(...); })`
✅ GOOD: Check ownership before serving files

**Audit:**
- File uploads require auth
- Access controlled by ownership
- Files can't be enumerated
- File type validated
- Size limits enforced

### 6. Dependencies

```bash
npm audit  # Must show: up to date, no vulnerabilities
```

**Audit:**
- Run npm audit - zero medium+ vulnerabilities
- All packages updated
- No typosquatted packages
- No deprecated packages

### 7. Secrets Management

❌ CRITICAL: `const apiKey = 'sk_live_123456';`
✅ CORRECT: `const apiKey = process.env.STRIPE_KEY;`

**Audit:**
- No secrets in code
- All secrets in env variables
- `.env` in `.gitignore`
- No secrets logged
- Staging/prod secrets differ

---

## Part 2: Testing Workflow

1. **Auth Test:** Send request without token → Should get 401
2. **Permission Test:** Login as User A, try accessing User B's data → Should fail
3. **Input Test:** Send `<script>alert(1)</script>` → Should be rejected/escaped
4. **Enumeration Test:** Try guessing file IDs → Should fail unless owned by user
5. **Privilege Test:** Try accessing admin endpoints as regular user → Should fail

---

## Part 3: Common AI Mistakes

**#1 Trusting Client Auth**
```javascript
❌ if (localStorage.getItem('userId') === userId) { ... }
✅ const decoded = jwt.verify(token, secret);
```

**#2 Optional RLS**
```javascript
❌ allow read: if true || request.auth != null;  // Always true!
✅ allow read: if request.auth != null && request.auth.uid == userId;
```

**#3 Unsafe Input**
```javascript
❌ db.query(`SELECT * FROM users WHERE name = '${input}'`);  // SQL Injection!
✅ db.query('SELECT * FROM users WHERE name = ?', [input]);
```

**#4 Logging Secrets**
```javascript
❌ console.log('User token:', token);  // Logs sensitive data!
✅ console.log('Auth failed for user:', userId);  // Safe
```

---

## Vulnerability Report Template

```markdown
### Vulnerability: [Name]

**Severity:** Critical/High/Medium/Low
**Location:** file.js line 45
**Problem:** [What's wrong]
**Why it matters:** [Worst-case impact]

**Step-by-Step Fix:**
1. [Action 1]
2. [Action 2]
3. [Test verification]
4. [Deploy plan]

**Preventative Practice:** Always test security with attacker mindset
```

---

## Quick Checklist

- [ ] All sensitive endpoints require auth
- [ ] Permissions checked server-side
- [ ] RLS enabled on sensitive data
- [ ] No hardcoded secrets
- [ ] No XSS vulnerabilities
- [ ] No SQL injection
- [ ] File access controlled
- [ ] Input validated & sanitized
- [ ] Error messages safe
- [ ] Dependencies updated
- [ ] No debug code in production

---

**Golden Rule: YOU are responsible for shipped code security. AI is a tool, not a guarantee.**
