# Coding Standards

**Versioning**: v1.0 | **Status**: Active | **Audience**: All Engineers

> **A friendly guide** — These are best practices to help us write clearer, more maintainable code. If you're new to any of these concepts, don't worry! We're here to help.


We believe great code is readable, maintainable, and kind to the next person who has to work with it (often that's future you!).

## Why This Matters

 follow these standards:
- Ensures consistency across the codebase
- Reduces cognitive load for code reviewers
- Prevents entire classes of bugs (formatting, naming confusion)
- Makes onboarding faster—new team members can read code without explanation

**SHOULD** follow these practices:
- Use meaningful variable names (even if they're longer)
- Comment the "why", not the "what"
- Keep functions small and focused

**NICE-TO-HAVE** optimizations:
- Performance micro-optimizations (profile first!)
- Fancy architectural patterns (YAGNI principle applies)

---

## 1. General Principles

### Readability > Cleverness

Code is read far more often than it's written. A slightly longer, clearer solution beats a clever one-liner.

**DO:**
```python
# Clear variable name, easy to understand
user_has_active_subscription = user.subscription.is_active and user.subscription.expires_at > now
```

**AVOID:**
```python
# Too clever, requires understanding complex conditions
has_sub = all([u.sub.active, u.sub.exp > n]) if u else False
```

### DRY (Don't Repeat Yourself)

: Don't copy-paste code. Extract into reusable functions or utilities.

**SHOULD**: Abstract repeated patterns into helpers, even if it's just 2–3 occurrences.

### Fail Fast, Fail Clearly

Invalid states should cause immediate, obvious failures, not silent bugs.

```python
# GOOD: Explicit validation
if not user.email:
    raise ValueError("User email is required")

# BAD: Silent failure, hard to debug
if user.email:
    send_notification(user.email)
# What happens if email is None? Confusing.
```

---

## 2. Naming Conventions

### Variables & Functions

- **Python**: `snake_case` for variables and functions
  ```python
  user_count = 10
  def calculate_total_price():
      pass
  ```

- **JavaScript**: `camelCase` for variables and functions
  ```javascript
  const userCount = 10;
  function calculateTotalPrice() { }
  ```

### Classes & Enums

- **Python & JavaScript**: `PascalCase`
  ```python
  class UserService:
      pass
  ```

### Constants

- All UPPERCASE with underscores
  ```python
  MAX_RETRY_ATTEMPTS = 3
  API_BASE_URL = "https://api.example.com"
  ```

### Boolean Variables

Prefix with `is_`, `has_`, `can_`, `should_`:
```python
is_active = True
has_admin_rights = False
can_delete_user = True
should_retry = True
```

---

## 3. Code Organization

### File Structure

Keep files focused on a single responsibility:

```
services/
  ├── user_service.py        # User-related business logic
  ├── billing_service.py     # Billing-related logic
  └── notification_service.py # Notifications
```

**AVOID**: 500+ line files. If a file is getting large, it's a sign to split it.

### Import Organization

**Order**: Standard library → Third-party → Local imports

```python
import os
import json

import requests
from flask import Flask

from services.user_service import UserService
from utils.helpers import parse_date
```

---

## 4. Function Design

### Size

: Keep functions small (ideal: < 20 lines)

**WHY?** Small functions are easier to test, understand, and modify.

### Parameters

**SHOULD**: Limit to 3 parameters. Use dictionaries/objects for more:

```python
# AVOID: Too many parameters
def create_user(name, email, phone, address, city, zip_code):
    pass

# BETTER: Use an object
class UserData:
    def __init__(self, name, email, phone, address, city, zip_code):
        self.name = name
        # ...

def create_user(user_data: UserData):
    pass
```

### Return Values

- Functions should have a clear, single responsibility
- Return one type consistently (avoid mixing `None` and a list)

```python
# AVOID: Inconsistent returns
def get_user(user_id):
    if not found:
        return None  # ← Sometimes None
    return user_list  # ← Sometimes a list

# BETTER: Be consistent
def get_user(user_id) -> Optional[User]:
    if not found:
        return None
    return user
```

---

## 5. Comments & Documentation

### Comment the "Why", Not the "What"

**BAD:**
```python
# Increment counter
counter += 1
```

**GOOD:**
```python
# Retry logic: only count failed login attempts, not all attempts
failed_login_attempts += 1
```

### Docstrings

 for public functions and classes:

```python
def calculate_discount(price: float, user_tier: str) -> float:
    """
    Calculate discount based on user tier.
    
    Args:
        price: Original price in SGD
        user_tier: 'basic', 'premium', or 'vip'
    
    Returns:
        Discounted price
    
    Raises:
        ValueError: If user_tier is invalid
    """
    pass
```

---

## 6. Error Handling

: Never silently fail.

```python
# AVOID: Silent failure
try:
    process_payment()
except Exception:
    pass  # ← What? Why? This is dangerous.

# BETTER: Log and re-raise if critical
try:
    process_payment()
except PaymentError as e:
    logger.error(f"Payment failed for user {user_id}: {str(e)}")
    raise  # Re-raise so caller knows it failed
except Exception as e:
    logger.warning(f"Unexpected error: {str(e)}")
    # Only suppress if we've explicitly decided it's safe
```

---

## 7. Testing Expectations

: Test at least the happy path and one error case.

```python
class TestUserService:
    def test_create_user_success(self):
        # Happy path
        pass
    
    def test_create_user_invalid_email(self):
        # Error case
        pass
```

---

## 8. Language-Specific Guides

### Python

- Follow [PEP 8](https://pep8.org/) (use tools like `black` for auto-formatting)
- Use type hints for better IDE support and fewer bugs
  ```python
  def greet(name: str) -> str:
      return f"Hello, {name}"
  ```
- Use f-strings for string formatting
  ```python
  message = f"User {name} has {count} items"  # ✓ Good
  message = "User {} has {} items".format(name, count)  # ✗ Old style
  ```

### JavaScript

- Use `const` by default, `let` if reassignment needed, avoid `var`
- Use async/await instead of `.then()` chains
  ```javascript
  // GOOD
  const data = await fetchUser(userId);
  
  // AVOID
  fetchUser(userId).then(data => { ... });
  ```
- Use template literals
  ```javascript
  const message = `User ${name} has ${count} items`;  // ✓
  ```

---

## 9. Automation & Tooling

We use automated tools to enforce standards (remove subjectivity):

- **Python**: `black` (formatter), `pylint` (linter), `pytest` (testing)
- **JavaScript**: `Prettier` (formatter), `ESLint` (linter), `Jest` (testing)

**Your CI/CD pipeline will fail if standards aren't met.** This is a feature, not a bug—it keeps our codebase healthy.

---

## Quick Checklist Before Code Review

- [ ] Code is readable without requiring the author to explain it
- [ ] No magic numbers or unexplained logic
- [ ] Functions are small and focused (< 20 lines each)
- [ ] Meaningful variable and function names
- [ ] Error handling is explicit, not silent
- [ ] Tests cover the happy path and at least one error case
- [ ] No commented-out code (delete it; version control has history)
- [ ] Code follows language-specific standards (PEP 8, Prettier, etc.)

---

## Questions?

Codebase standards evolve. Found something unclear or missing? File an issue or ask in `#engineering-chat`.
