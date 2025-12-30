# Secrets Management

**Versioning**: v1.0 | **Status**: Active | **Audience**: All Engineers

> **Keep it simple**: Don't overthink secrets management. Just follow these basic rules and you'll be fine.

Secrets (API keys, database passwords, OAuth tokens) are the keys to our kingdom. Let's protect them.

## Core Rules

### 1. Never Commit Secrets to Git

**NEVER** commit to version control:
- Database passwords
- API keys and tokens
- Private encryption keys
- OAuth client secrets
- Webhook signing keys
- Payment processor tokens

If you accidentally commit a secret:
1. **Immediately rotate the credential** (generate a new key, revoke the old one)
2. **Force push the removal** or use `git filter-branch` to remove it from history
3. **Check logs** to see if the secret was exposed
4. **Alert the team** if it's sensitive

### 2. Use Environment Variables

**Locally**, store secrets in a `.env` file (which is in `.gitignore`):

```bash
# .env (never commit this)
DATABASE_PASSWORD=super_secret_password
API_KEY=sk_live_1234567890
JWT_SECRET=my_jwt_secret
```

**In code**, load from environment:

```python
import os

db_password = os.environ.get('DATABASE_PASSWORD')
api_key = os.environ.get('API_KEY')
```

```javascript
const dbPassword = process.env.DATABASE_PASSWORD;
const apiKey = process.env.API_KEY;
```

### 3. Production Secrets

For production, **use a proper secrets manager**:
- AWS Secrets Manager
- Google Cloud Secret Manager
- HashiCorp Vault
- Azure Key Vault

Don't put secrets directly in environment variables on production servers.

### 4. Secret Rotation

**Regularly rotate secrets**, especially for:
- Database passwords (quarterly)
- API keys (when developers leave the team)
- OAuth credentials (as needed)

### 5. Access Control

**Limit who can access secrets**:
- Only people who need the secret should have it
- Rotate secrets when someone leaves the team
- Use separate secrets for different environments (dev, staging, production)

## Template: .env File

```bash
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/better_sg
DATABASE_PASSWORD=your_secure_password

# APIs
OPENAI_API_KEY=sk_test_...
GOOGLE_API_KEY=AIza...
STRIPE_SECRET_KEY=sk_live_...

# Auth
JWT_SECRET=your_super_secret_jwt_key
OAUTH_CLIENT_ID=client_id
OAUTH_CLIENT_SECRET=client_secret

# Environment
ENVIRONMENT=development
DEBUG=True
```

## .gitignore Setup

Make sure your `.gitignore` includes:

```
# Environment variables
.env
.env.local
.env.*.local

# IDE
.vscode/
.idea/

# Dependencies
node_modules/
venv/
```

## Questions?

If you're unsure whether something is a secret, ask! It's better to be cautious.
