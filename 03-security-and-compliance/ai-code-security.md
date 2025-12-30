# AI Code Security

**Versioning**: v1.0 | **Status**: Active | **Audience**: All Engineers

> **AI tools are powerful but risky**: Use them wisely. Never trust them completely.

AI assistants (GitHub Copilot, Claude, ChatGPT) can help write code faster, but they can also introduce security vulnerabilities. Use them safely.

## Golden Rules

1. **Never put secrets in AI prompts**
2. **Review all AI-generated code carefully**
3. **Don't use proprietary code as context**
4. **Understand the code before using it**
5. **Run security scans on AI-generated code**

## What NOT to Do with AI

### ❌ Don't Paste Secrets

```python
# NEVER do this:
"Here's our API key: sk_live_abc123... Can you help debug?"

# The AI will see your secret
# It may be logged, stored, or used for training
```

**Risk**: Your secrets get exposed or leaked

### ❌ Don't Share Proprietary Code

```python
# NEVER do this:
"Here's our entire user database schema. 
Help me optimize this query."

# You're sharing confidential code
# It could be used to train competitors' models
```

**Risk**: Losing competitive advantage

### ❌ Don't Trust AI Output Blindly

```python
# AI generated this. Don't use it without review:
result = os.system(user_input)  # Vulnerable to command injection!
db.query(f"SELECT * FROM users WHERE id = {user_id}")  # SQL injection!
```

**Risk**: Security vulnerabilities in your code

### ❌ Don't Use Unverified Dependencies

```python
# AI suggests importing unknown libraries
# Check if they're:
# - Legitimate
# - Actively maintained
# - Not typosquatting attacks
```

**Risk**: Installing malicious packages

## What TO Do with AI

### ✅ Use AI for Code Structure

```
Prompt: "Give me the structure for a Flask API with user authentication. 
Don't include real secrets."

Result: Boilerplate code you can customize safely
```

### ✅ Review Generated Code

1. **Read every line**: Understand what it does
2. **Check for vulnerabilities**: SQL injection, XSS, hardcoded secrets
3. **Verify dependencies**: Are they real?
4. **Test it**: Run tests and security scans
5. **Run SAST tools**: Let static analysis find issues

### ✅ Use AI for Learning

```
Prompt: "Explain how SQL injection works and how to prevent it."

Result: Educational explanation (useful!)
```

### ✅ Use Prompts Without Context

```
# GOOD:
"Show me how to hash passwords in Python securely."

# The AI answers without needing your code
```

## Security Checklist for AI Code

Before merging AI-generated code:

- [ ] I've read every line and understand it
- [ ] No hardcoded secrets (API keys, passwords, tokens)
- [ ] No suspicious `os.system()` or `exec()` calls
- [ ] SQL queries use parameterized statements
- [ ] No obvious XSS vulnerabilities
- [ ] Dependencies are verified and legitimate
- [ ] Security scanning tools pass
- [ ] Tests cover the code
- [ ] Code review approved

## Common AI Security Mistakes

### Mistake 1: SQL Injection

**AI Generated** (VULNERABLE):
```python
db.query(f"SELECT * FROM users WHERE email = '{email}'")
```

**What You Should Do**:
```python
db.query("SELECT * FROM users WHERE email = ?", (email,))
```

### Mistake 2: Hardcoded Credentials

**AI Generated** (VULNERABLE):
```python
api_key = "sk_live_abc123def456"
password = "admin123"
```

**What You Should Do**:
```python
import os
api_key = os.environ.get('API_KEY')
password = os.environ.get('DB_PASSWORD')
```

### Mistake 3: Command Injection

**AI Generated** (VULNERABLE):
```python
os.system(f"convert {filename} output.jpg")
```

**What You Should Do**:
```python
import subprocess
subprocess.run(["convert", filename, "output.jpg"], check=True)
```

### Mistake 4: Unverified Dependencies

**AI Suggests**:
```bash
pip install usefull-library  # Typosquatting attack!
```

**What You Should Do**:
```bash
# Check if it's real first
# pip search doesn't work anymore, so use:
# - PyPI.org
# - GitHub (is it maintained?)
# - Community feedback
pip install useful_library  # Verify the correct name
```

## Privacy Considerations

### Data Privacy

**GitHub Copilot**:
- Stores code snippets (even if you don't use them)
- Microsoft/GitHub may use it for training
- Disable Copilot if handling highly sensitive code

**ChatGPT/Claude**:
- Stores conversations (check their privacy policy)
- Conversations may be used for training
- Don't paste real user data or secrets

### Customer Data

NEVER paste:
- Real user information
- Personal data (emails, phone numbers)
- Financial information
- Health data
- Any production data

## AI Tools at Better.sg

**We use**:
- GitHub Copilot (for code suggestions)
- Claude (for documentation)
- ChatGPT (for research)

**Rules**:
- No real secrets in prompts
- Review all output before using
- Run security scans
- Document that AI was used

## Questions?

Unsure if it's safe to share code with AI? Ask first.

Better safe than sorry.
