# Lovable Vibe Coding Guide: Practical Prompts & Security

## What is Vibe Coding?

**Vibe Coding** = Using AI tools (Lovable, Copilot, Claude) to rapidly prototype and build features while maintaining code quality and security.

**Golden Rule:** AI is your pair programmer, not your replacement. YOU validate everything.

---

## Part 1: Lovable-Specific Prompts

### Prompt 1: Create Secure API Endpoint

```
Prompt to Lovable/Claude:
"Create a Next.js API endpoint POST /api/profile that:
1. Requires Firebase authentication
2. Only allows users to update their own profile
3. Validates input: name (string, max 100), email (valid email format)
4. Returns 401 if not authenticated
5. Returns 403 if trying to update another user's data
6. Never logs sensitive data
7. Uses TypeScript

Include proper error handling."
```

**What to check after:**
- ✅ Auth check is present
- ✅ Permission check compares `req.user.id` with `userId` param
- ✅ Input validation before processing
- ✅ No secrets in response

### Prompt 2: Firebase RLS with Auth

```
Prompt to Claude:
"Write Firestore Security Rules for a 'users' collection where:
1. Users can only read their own document
2. Users can only update their own document
3. Only admins (user.custom.role == 'admin') can delete any user
4. Timestamps are auto-updated on write
5. Email field is never readable by other users

Use proper RLS syntax."
```

**What to check after:**
- ✅ `request.auth.uid == resource.data.userId` check present
- ✅ Admin check uses custom claims, not user input
- ✅ All sensitive fields protected

### Prompt 3: Input Validation Function

```
Prompt to Copilot:
"Create a TypeScript validation function for user registration that:
1. Validates email format (RFC 5322)
2. Validates password strength (min 12 chars, 1 uppercase, 1 number, 1 special)
3. Validates username (alphanumeric + underscore, 3-30 chars)
4. Escapes HTML special characters
5. Returns detailed error messages
6. Includes unit tests

Use a library like 'validator' or write custom validation."
```

**What to check after:**
- ✅ All fields validated BEFORE use
- ✅ HTML escaping prevents XSS
- ✅ Error messages don't leak internals

---

## Part 2: Tech-Specific Security Links

### NPM & Node.js
- **NPM Security Audit**: https://docs.npmjs.com/cli/v9/commands/npm-audit
- **Node.js Security**: https://nodejs.org/en/docs/guides/security/
- **NPM Package Quality**: https://snyk.io/advisor/
- **OWASP Top 10 Node**: https://owasp.org/Top10/

### Firebase Security
- **Firestore Security Rules**: https://firebase.google.com/docs/rules
- **Firebase Auth Security**: https://firebase.google.com/docs/auth/security-rules
- **Firebase Best Practices**: https://firebase.google.com/docs/security/best-practices
- **Cloud Functions Security**: https://firebase.google.com/docs/functions/security/

### Azure Security
- **Azure Security Baselines**: https://docs.microsoft.com/en-us/security/benchmark/azure/security-baseline
- **SQL Database Security**: https://docs.microsoft.com/en-us/azure/sql-database/sql-database-security-overview
- **App Service Security**: https://docs.microsoft.com/en-us/azure/app-service/overview-security
- **Key Vault Best Practices**: https://docs.microsoft.com/en-us/azure/key-vault/general/best-practices

### Frontend Security
- **OWASP XSS Prevention**: https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html
- **Content Security Policy**: https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP
- **React Security**: https://react.dev/learn/security
- **Next.js Security**: https://nextjs.org/docs/advanced-features/security-headers

### Database Security
- **SQL Injection Prevention**: https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html
- **NoSQL Injection**: https://cheatsheetseries.owasp.org/cheatsheets/NoSQL_Injection_Prevention_Cheat_Sheet.html
- **Data Encryption**: https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html

---

## Part 3: Real-World Vibe Coding Workflow

### Step 1: Define Requirements
```
BEFORE using Lovable:
- Write clear functional requirements
- Document security requirements
- Specify error handling
- Plan test cases
```

### Step 2: Use Lovable/Copilot
```
Generate code with specific prompts including:
- "Include error handling for [specific case]"
- "Add input validation for [field]"
- "Implement authentication check"
- "Never log passwords or tokens"
```

### Step 3: Security Review (CRITICAL)
```
Before merging, run:
1. npm audit (zero medium+ vulnerabilities)
2. Manual security audit (use checklist from ai-code-security-audit.md)
3. Functional testing
4. Security testing (try to break it)
```

### Step 4: Code Review
```
Review must cover:
- ✅ Auth is enforced
- ✅ Input is validated
- ✅ Permissions checked server-side
- ✅ No hardcoded secrets
- ✅ No XSS/injection vulnerabilities
- ✅ Dependencies up-to-date
```

---

## Part 4: Common Lovable Mistakes

### Mistake #1: Generated Code Looks Secure But Isn't
```javascript
// Lovable might generate this (LOOKS good but isn't)
route.get('/api/user/:userId', (req, res) => {
  const user = await db.collection('users').doc(req.params.userId).get();
  res.json(user.data());
});

// FIX: Add auth + permission check
route.get('/api/user/:userId', requireAuth, (req, res) => {
  if (req.user.id !== req.params.userId) return res.status(403).send('Forbidden');
  const user = await db.collection('users').doc(req.params.userId).get();
  res.json(user.data());
});
```

### Mistake #2: RLS Rules Generated Without Edge Cases
```javascript
// Generated RLS (incomplete)
match /users/{userId} {
  allow read, write: if request.auth.uid == userId;
}

// FIX: Add subcollection rules
match /users/{userId} {
  allow read, write: if request.auth.uid == userId;
  match /preferences/{prefId} {
    allow read, write: if request.auth.uid == userId;
  }
}
```

### Mistake #3: Skipping Tests
```
NEVER skip tests even if Lovable generates code.
Always test:
- Can unauthenticated user access endpoint? (Should fail)
- Can user A access user B's data? (Should fail)
- Does input validation reject malicious input? (Should reject)
```

---

## Part 5: When NOT to Use Vibe Coding

❌ **Don't use AI for:**
- Authentication/authorization logic (too critical)
- Encryption algorithms (use libraries only)
- RLS rules (manually verify every rule)
- Secrets management (write by hand)
- Security-critical algorithms

✅ **OK to use AI for:**
- Boilerplate code
- CRUD operations
- UI components
- Testing code
- Documentation
- Refactoring

---

## Part 6: AI Tool Ecosystem

**Lovable** (Better.sg Primary)
- Best for: Full-stack app generation
- Reference: https://lovable.dev/
- Use case: MVP prototyping, rapid feature building

**GitHub Copilot**
- Best for: Inline code completion
- Reference: https://github.com/features/copilot
- Use case: Writing code faster, exploring APIs

**Claude/ChatGPT**
- Best for: Complex logic, algorithms
- Reference: https://claude.ai/ / https://chat.openai.com/
- Use case: Debugging, explaining code, refactoring advice

---

## Quick Checklist: Before Shipping Lovable Code

- [ ] Code runs locally without errors
- [ ] npm audit shows zero medium+ vulnerabilities
- [ ] All endpoints require authentication (if sensitive)
- [ ] Permission checks done server-side
- [ ] Input validation present on all user input
- [ ] No secrets hardcoded
- [ ] Error messages don't leak internals
- [ ] Tests pass (existing + new)
- [ ] Code reviewed by 2 people
- [ ] Security audit passed (ai-code-security-audit.md)
- [ ] Firebase RLS manually verified
- [ ] Staging deployment successful

---

**Golden Rule: You are responsible for the code you ship. AI is a tool, not a guarantee of quality or security.**
