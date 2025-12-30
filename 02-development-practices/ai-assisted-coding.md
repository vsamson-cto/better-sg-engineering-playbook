# AI-Assisted Coding Guide

**Versioning**: v1.0 | **Status**: Active | **Audience**: All Engineers

> **AI tools are here**: Use GitHub Copilot, Claude, and ChatGPT to write code faster. But stay in control.

We encourage using AI coding assistants at Better.sg. They can significantly speed up development. Just use them wisely.

## What We Use

- **GitHub Copilot** - Code suggestions while you type
- **Claude/ChatGPT** - Exploring ideas, explaining concepts
- **Local LLMs** - For sensitive code

## The Golden Rules

### 1. You Are the Author

You are responsible for every line of code AI generates. Always:
- Understand what the code does
- Verify it works correctly
- Ensure it follows our standards

### 2. Review Everything

AI-generated code needs the same review as human-written code. Code review catches:
- Security vulnerabilities
- Performance issues
- Bugs and edge cases
- Deviations from our standards

### 3. Never Share Secrets

Don't paste:
- API keys or passwords
- Database credentials
- Proprietary algorithms
- Real customer data

AI services may log, store, or train on your inputs.

### 4. Test Thoroughly

AI can:
- Generate plausible-looking but incorrect code
- Miss edge cases
- Introduce subtle bugs

Always test AI-generated code with real inputs.

## Best Practices

### ✅ Good Uses of AI

**Boilerplate Code**
```
Prompt: "Generate a Flask REST API endpoint for user authentication"
Result: Time saved, standard patterns, needs review
```

**Explaining Concepts**
```
Prompt: "Explain how OAuth 2.0 works"
Result: Educational, helps you understand better
```

**Documentation**
```
Prompt: "Write docstrings for this Python function"
Result: Better documentation, saves time
```

**Refactoring Ideas**
```
Prompt: "How would you refactor this JavaScript function for readability?"
Result: Multiple approaches, you choose the best
```

### ❌ Risky Uses of AI

**Copy-Pasting Without Review**
```
❌ AI generates code → You paste it → You submit PR
✅ AI generates code → You review → You test → You submit PR
```

**Trusting AI on Security**
```
❌ Prompt: "Authenticate this user"
✅ Prompt: "Show me password hashing best practices in Python"
```

**Using Without Tests**
```
❌ "AI said it's fine"
✅ Test with unit tests, integration tests, edge cases
```

## Common Pitfalls

### Pitfall 1: Hallucinated Libraries

AI might suggest libraries that don't exist.

```python
# AI generates:
import best_library  # ❌ This doesn't exist!

# Always verify:
pip search best_library
# or check on PyPI.org
```

### Pitfall 2: Outdated Patterns

AI trained on old code might suggest deprecated approaches.

```python
# AI generates (outdated):
using_old_api()  # ❌ Deprecated in v2.0

# Check documentation first
# Use current best practices
using_new_api()  # ✅
```

### Pitfall 3: Insecure Code

AI isn't security-aware and might generate vulnerabilities.

```python
# AI generates (vulnerable):
db.query(f"SELECT * FROM users WHERE id = {user_id}")  # SQL injection!

# Always review for:
# - SQL injection
# - XSS vulnerabilities
# - Hardcoded secrets
# - Command injection
```

### Pitfall 4: Brittle Tests

AI-generated tests might not cover edge cases.

```python
# AI generates:
def test_add():
    assert add(2, 2) == 4  # Passes, but incomplete

# You should add:
def test_add_negative():
    assert add(-1, 1) == 0

def test_add_zero():
    assert add(0, 0) == 0

def test_add_floats():
    assert add(1.5, 2.5) == 4.0
```

## Workflow: Using AI Effectively

### Step 1: Write the Prompt

```
Better: "Write a Python function to validate email addresses using regex. 
Include error handling for invalid input."

Weaker: "Write email validation"
```

### Step 2: Review the Output

- Read every line
- Check for obvious bugs
- Verify it matches your intent
- Check for security issues

### Step 3: Test It

```bash
# Write tests
pytest test_email.py

# Test edge cases
# Test with invalid input
# Test with extreme values
```

### Step 4: Code Review

Submit for review like any other PR. Reviewers will catch:
- Issues you missed
- Style inconsistencies
- Better approaches

## Code Review of AI Code

When reviewing AI-generated code, check extra carefully for:

- [ ] Security vulnerabilities (SQL injection, XSS, etc.)
- [ ] Hardcoded secrets or credentials
- [ ] Proper error handling
- [ ] Edge cases
- [ ] Performance issues
- [ ] Adherence to our coding standards
- [ ] Proper test coverage
- [ ] Documentation

## Real Examples

### Example 1: Good AI Suggestion

**Prompt**: "Generate a Python function to calculate the average of a list"

**AI Output** (good):
```python
def calculate_average(numbers: list[float]) -> float:
    """Calculate the average of a list of numbers."""
    if not numbers:
        raise ValueError("Cannot calculate average of empty list")
    return sum(numbers) / len(numbers)
```

**Your Review**:
- ✅ Type hints included
- ✅ Docstring present
- ✅ Error handling for empty list
- ✅ Clear and simple

**Your Addition**:
Add tests for edge cases

### Example 2: Problematic AI Code

**Prompt**: "Help me fetch user data from an API"

**AI Output** (problematic):
```python
import requests

api_key = "sk_live_abc123def456"  # ❌ Hardcoded!
response = requests.get(f"https://api.example.com/users/{user_id}",
                        headers={"Authorization": f"Bearer {api_key}"})
data = response.json()  # ❌ No error handling!
return data
```

**Your Fixes**:
```python
import os
import requests
from requests.exceptions import RequestException

api_key = os.environ.get('API_KEY')  # ✅ From environment
if not api_key:
    raise ValueError("API_KEY not set")

try:
    response = requests.get(
        f"https://api.example.com/users/{user_id}",
        headers={"Authorization": f"Bearer {api_key}"},
        timeout=10  # Add timeout
    )
    response.raise_for_status()  # ✅ Error handling
    return response.json()
except RequestException as e:
    logger.error(f"API request failed: {e}")
    raise
```

## When NOT to Use AI

- **Security-critical code** - Write it yourself, then ask AI for review
- **Algorithms you don't understand** - Learn first, then use AI for help
- **Code you can't fully review** - Don't use it
- **When you're learning** - Challenge yourself first

## Tips for Success

1. **Use specific prompts**: The better your prompt, the better the output
2. **Ask follow-up questions**: "Why did you choose this approach?"
3. **Mix AI with manual coding**: Combine the best of both
4. **Document AI usage**: If AI wrote code, mention it in comments
5. **Improve your prompts over time**: Learn what works for you

## Questions?

Unsure if an AI suggestion is good? Ask in `#engineering-chat` before merging.

Remember: **You control the AI, not the other way around.**
