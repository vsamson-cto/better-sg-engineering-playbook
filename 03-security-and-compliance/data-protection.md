# Data Protection & PDPA Compliance

**Versioning**: v1.0 | **Status**: Active | **Audience**: All Engineers

> **PDPA matters**: Singapore's Personal Data Protection Act protects user privacy. Respect it.

We collect and process personal data from our users. We must protect it properly and comply with Singapore's PDPA.

## PDPA Quick Overview

The Personal Data Protection Act (PDPA) requires:

1. **Consent**: Get explicit permission before collecting personal data
2. **Purpose Limitation**: Only use data for stated purposes
3. **Notification**: Tell users what data you're collecting
4. **Access & Correction**: Users can request and correct their data
5. **Accuracy**: Keep data accurate and up-to-date
6. **Protection**: Secure personal data from misuse/loss
7. **Retention**: Don't keep data longer than necessary
8. **Transfer**: Only transfer data with proper safeguards

## What is Personal Data?

Personal data includes:
- Names, email addresses, phone numbers
- ID numbers (NRIC, passport)
- Date of birth
- Home addresses
- Bank account information
- IP addresses (in some cases)
- Cookies that identify a person

## Core Practices

### 1. Get Consent Before Collecting

Before collecting personal data:
- **Inform** the user what you're collecting
- **Explain** why you need it
- **Get permission** (checkbox, explicit opt-in)
- **Document** the consent

NEVER collect data without clear consent.

### 2. Encrypt Sensitive Data

Encrypt at rest:
- Passwords: bcrypt or Argon2
- Credit cards: PCI-DSS compliance or tokenization
- IDs: AES-256 encryption
- Email addresses: Only if combined with other data

Encrypt in transit:
- HTTPS everywhere
- TLS 1.2 or higher

### 3. Limited Access

Only give access to those who need it:
- Admin dashboards: Limited to authorized staff
- Database access: Principle of least privilege
- Third-party integrations: Review before granting access

### 4. Deletion & Retention

Delete data when no longer needed:

```python
# Example: Auto-delete old logs after 90 days
if log_entry.created_at < now() - timedelta(days=90):
    log_entry.delete()
```

Retention guidelines:
- User account data: Keep until account closed
- Transaction logs: 7 years (for financial records)
- Marketing data: Until user unsubscribes
- Security logs: 1-3 years

### 5. User Rights

**You must allow users to**:
- View their data (Subject Access Request)
- Correct inaccurate data
- Request deletion (Right to be Forgotten)
- Know how you use their data
- Opt-out of marketing

Implement these in your app.

### 6. Data Breach Response

If personal data is breached:
1. **Assess the breach**: What data? How many users affected?
2. **Notify affected users**: Without unreasonable delay
3. **Report to PDPC**: If the breach is serious
4. **Fix the issue**: Prevent future breaches
5. **Document everything**: For compliance audit

## Practical Checklist

Before launching a feature that collects data:

- [ ] Privacy Policy updated
- [ ] Consent mechanism implemented
- [ ] Data encrypted at rest
- [ ] Data encrypted in transit (HTTPS)
- [ ] Access controls in place
- [ ] Retention policy documented
- [ ] Deletion mechanism implemented
- [ ] User data access/correction feature built
- [ ] Third-party vendors reviewed for PDPA compliance
- [ ] Data breach response plan documented

## Examples

### DO: Collect with Consent

```
We collect your email to send you updates. 
You can unsubscribe anytime.
[✓] I agree to receive updates
```

### DON'T: Collect Without Consent

```
We track your IP, device, and browsing history.
(No opt-in, hidden in privacy policy)
```

### DO: Encrypt Sensitive Data

```python
from argon2 import PasswordHasher
ph = PasswordHasher()
hashed_password = ph.hash(user_password)
db.save(hashed_password)  # GOOD
```

### DON'T: Store Plaintext Passwords

```python
db.save(user_password)  # BAD!
```

## Questions?

Consult the official PDPA guide: https://www.pdpc.gov.sg/

If unsure about data practices, ask before implementing.
