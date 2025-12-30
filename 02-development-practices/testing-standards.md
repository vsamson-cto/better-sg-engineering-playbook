# Testing Standards

**Versioning**: v1.0 | **Status**: Active | **Audience**: All Engineers

> **Start small**: Don't worry about hitting 100% coverage immediately. Even simple tests that cover the happy path are better than none. Tests are a skill you build over time.


Testing isn't a chore—it's insurance against shipping broken code. Tests give us confidence to refactor, deploy, and sleep at night.

## Why Testing Matters

**MUST** write tests because:
- Prevents bugs from reaching production
- Makes refactoring safe
- Serves as living documentation
- Catches regressions early

**SHOULD** aim for:
- Good test coverage (ideally 70%+)
- Fast test suites (run in < 5 seconds for unit tests)
- Readable test names that describe intent

**NICE-TO-HAVE** additions:
- Integration test suites
- Performance benchmarks
- Visual regression tests

---

## 1. Testing Pyramid

We follow the testing pyramid: many unit tests, some integration tests, few E2E tests.

```
        /\          ← E2E Tests (UI, slow, brittle)
       /  \
      /    \       ← Integration Tests (multiple components)
     /------\
    /        \    ← Unit Tests (fast, isolated)
   /          \
```

### Unit Tests (60%)

- **What**: Test a single function or class in isolation
- **Speed**: < 100ms per test
- **Mock**: External dependencies (APIs, databases, filesystems)
- **Why**: Fast feedback, catch bugs early, easy to debug

### Integration Tests (30%)

- **What**: Test multiple components working together
- **Speed**: < 1 second per test
- **Mock**: External services (APIs), use test database
- **Why**: Catch issues between components, realistic scenarios

### E2E Tests (10%)

- **What**: Test entire user workflows end-to-end
- **Speed**: 1-10 seconds per test
- **Mock**: Nothing (or only third-party APIs)
- **Why**: Verify real user scenarios, catch integration issues

---

## 2. Unit Testing Best Practices

### Test Structure: Arrange-Act-Assert

```python
def test_user_can_login_with_correct_password():
    # ARRANGE: Set up test data
    user = User(email="test@example.com", password="secure123")
    
    # ACT: Perform the action
    result = user.authenticate(password="secure123")
    
    # ASSERT: Verify the result
    assert result is True
```

### Descriptive Test Names

Test names should read like a sentence: "[What] [should] [when]"

**GOOD:**
```python
def test_calculate_discount_returns_10_percent_for_premium_users():
    pass

def test_user_registration_fails_with_invalid_email():
    pass

def test_payment_processing_retries_on_network_error():
    pass
```

**AVOID:**
```python
def test_discount():
    pass

def test_user():
    pass

def test_error():
    pass
```

### One Assertion Per Test (or One Logical Assertion)

Keep tests focused. If it fails, you know exactly why.

**GOOD:**
```python
def test_invoice_total_includes_tax():
    invoice = Invoice(subtotal=100, tax_rate=0.15)
    assert invoice.total == 115

def test_invoice_email_is_required():
    with pytest.raises(ValueError, match="email required"):
        Invoice(email="")
```

**AVOID:**
```python
def test_invoice():
    invoice = Invoice(subtotal=100, tax_rate=0.15)
    assert invoice.total == 115
    assert invoice.tax == 15
    assert invoice.is_valid()
    assert invoice.send_email() is True  # Multiple concerns
```

### Use Fixtures for Common Setup

```python
@pytest.fixture
def user():
    """Create a test user."""
    return User(email="test@example.com", name="Test User")

def test_user_has_email(user):
    assert user.email == "test@example.com"

def test_user_name(user):
    assert user.name == "Test User"
```

---

## 3. Integration Testing Best Practices

### Test Real Interactions

Unlike unit tests, integration tests use real components:

```python
def test_user_registration_persists_to_database():
    # This test actually calls the database
    user_service = UserService(db=test_database)
    
    user = user_service.register(
        email="new@example.com",
        password="secure123"
    )
    
    # Verify data was actually saved
    retrieved_user = test_database.query(User).filter_by(
        email="new@example.com"
    ).first()
    
    assert retrieved_user is not None
    assert retrieved_user.id == user.id
```

### Use Test Databases

**MUST**: Never test against the production database.

- Set up a separate test database
- Reset it before each test
- Or use transactions that roll back automatically

```python
@pytest.fixture(autouse=True)
def cleanup_database():
    """Clean up test database between tests."""
    yield
    # Rollback all changes after each test
    db.session.rollback()
```

---

## 4. Error Case Testing

**MUST**: Test both success and failure paths.

```python
# Test the happy path
def test_payment_processing_succeeds_with_valid_card():
    payment = process_payment(card=valid_card, amount=100)
    assert payment.status == "completed"
    assert payment.amount == 100

# Test error cases
def test_payment_processing_fails_with_invalid_card():
    with pytest.raises(PaymentError, match="Invalid card"):
        process_payment(card=invalid_card, amount=100)

def test_payment_processing_fails_with_insufficient_funds():
    with pytest.raises(PaymentError, match="Insufficient funds"):
        process_payment(card=low_balance_card, amount=1000)

def test_payment_processing_retries_on_network_error():
    with patch('payment_api.charge') as mock_charge:
        mock_charge.side_effect = [
            NetworkError("Timeout"),
            PaymentResponse(status="success")  # Retries succeed
        ]
        
        payment = process_payment(card=valid_card, amount=100)
        assert payment.status == "completed"
        assert mock_charge.call_count == 2  # Tried twice
```

---

## 5. Mocking & Test Isolation

### Mock External Dependencies

Don't make real API calls, file system calls, or database queries in unit tests.

**GOOD:**
```python
from unittest.mock import patch, MagicMock

def test_user_notification_sent_on_registration():
    with patch('email_service.send_email') as mock_email:
        user = register_user(email="new@example.com")
        
        # Assert email service was called
        mock_email.assert_called_once_with(
            to="new@example.com",
            subject="Welcome!"
        )
```

**AVOID:**
```python
def test_user_notification_sent_on_registration():
    user = register_user(email="new@example.com")
    
    # This actually calls your email provider!
    # Slow, unreliable, expensive
    email_received = gmail_api.check_inbox("new@example.com")
    assert "Welcome" in email_received
```

### Keep Tests Independent

Tests should not depend on each other. Run them in any order.

```python
# AVOID: Tests that depend on execution order
def test_1_create_user():
    global user_id
    user_id = create_user()

def test_2_fetch_user():
    user = get_user(user_id)  # Depends on test_1!

# GOOD: Each test is self-contained
def test_create_user():
    user_id = create_user()
    assert user_id is not None

def test_fetch_user():
    user_id = create_user()  # Set up its own data
    user = get_user(user_id)
    assert user is not None
```

---

## 6. Test Coverage

**SHOULD**: Aim for 70%+ code coverage.

```bash
# Run tests with coverage report
pytest --cov=app --cov-report=html

# JavaScript
npm test -- --coverage
```

**Coverage Goals:**
- Critical paths (auth, payments, data): 90%+
- Core features: 70%+
- Utilities: 60%+
- Styles/configs: Doesn't matter

**Note**: 100% coverage doesn't guarantee bug-free code. Focus on testing important logic, not just hitting a number.

---

## 7. Performance Testing

### Load Testing Critical Endpoints

```python
import pytest
import time

@pytest.mark.performance
def test_user_search_completes_under_500ms():
    start = time.time()
    results = search_users(query="test")
    duration = time.time() - start
    
    assert duration < 0.5, f"Search took {duration}s, expected < 0.5s"
    assert len(results) > 0
```

### Benchmark Comparisons

```python
# Compare performance between versions
pytest-benchmark test_search_performance.py
```

---

## 8. Testing Database Migrations

**MUST**: Test that migrations work correctly.

```python
def test_migration_adds_email_column():
    # Run migration up
    migrate_to("202401_add_email_column")
    
    # Verify column exists
    assert "email" in get_table_columns("users")
    
    # Run migration down
    migrate_to("before_202401")
    
    # Verify column is removed
    assert "email" not in get_table_columns("users")
```

---

## 9. Continuous Integration

### Run Tests Automatically

Every PR should:
- Run all unit tests
- Run integration tests
- Check code coverage
- Run linters

Failing tests = PR cannot be merged.

### Example GitHub Actions Workflow

```yaml
name: Tests
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-python@v2
        with:
          python-version: 3.9
      - run: pip install -r requirements.txt
      - run: pytest --cov=app
      - run: pylint app/
```

---

## 10. Quick Checklist: Before Committing

- [ ] All unit tests pass locally
- [ ] All integration tests pass
- [ ] Code coverage is above 70%
- [ ] No skipped tests (`@pytest.mark.skip`)
- [ ] Test names are descriptive
- [ ] No hardcoded test data in production code
- [ ] Tests are independent (can run in any order)
- [ ] Error cases are tested

---

## Languages & Frameworks

### Python

- **Framework**: `pytest` (unit & integration)
- **Mocking**: `unittest.mock` or `pytest-mock`
- **Fixtures**: `pytest` fixtures
- **Coverage**: `pytest-cov`

### JavaScript

- **Unit Testing**: `Jest` or `Vitest`
- **Integration**: `Supertest` (for APIs)
- **E2E**: `Cypress` or `Playwright`
- **Coverage**: Built into Jest

---

## Questions?

Testing saves time and reduces bugs. Invest in good tests early, and future you will be grateful.
