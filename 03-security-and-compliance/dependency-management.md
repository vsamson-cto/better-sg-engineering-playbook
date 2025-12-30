# Dependency Management

**Versioning**: v1.0 | **Status**: Active | **Audience**: All Engineers

> **Keep dependencies minimal**: Every dependency is another thing that could break. Only add what you really need.

We depend on third-party libraries for functionality. Let's manage them responsibly.

## Core Principles

### 1. Minimize Dependencies

Before adding a new package:
- Does it solve a real problem?
- Is there a simpler alternative?
- Can we build it ourselves?
- Is it actively maintained?

**Ask yourself**: Will this library still be maintained in 2 years?

### 2. Vet Before Adding

Before adding a dependency, check:

- **Maintenance**: Is it actively maintained? (Check last commit date)
- **Community**: Does it have reasonable usage? (GitHub stars, npm downloads)
- **Security**: Has it had security issues?
- **License**: Is it compatible with our license?
- **Size**: Will it bloat our bundle?

### 3. Keep Dependencies Updated

**Weekly**: Check for security updates

```bash
# Python
pip list --outdated
pip audit

# JavaScript
npm outdated
npm audit
```

**Monthly**: Update minor/patch versions

```bash
# Python
pip install --upgrade package_name

# JavaScript
npm update
```

**Quarterly**: Review and update major versions (test carefully!)

### 4. Remove Unused Dependencies

Regularly audit what you actually use:

```bash
# Python
pip list | grep -v -f <(pip show -f package | grep Location)

# JavaScript
npm ls --depth=0
```

Delete anything you're not using.

## Security Practices

### Check for Vulnerabilities

```bash
# Python
pip audit

# JavaScript
npm audit
```

If vulnerabilities are found:
1. **Understand the risk**: Is it relevant to your code?
2. **Update**: Try updating the dependency first
3. **Patch**: If no update, look for patches
4. **Replace**: Consider using a different library
5. **Ignore (cautiously)**: Only if the vulnerability doesn't apply to you

### Dependency Pinning

**For production**, pin to specific versions:

```bash
# Python (requirements.txt)
flask==2.3.2
requests==2.31.0

# JavaScript (package.json)
"dependencies": {
  "express": "4.18.2",
  "axios": "1.4.0"
}
```

This prevents unexpected breaking changes.

### Use Lock Files

**Always commit lock files**:

```bash
# Python
requirements.txt (or Pipfile.lock)

# JavaScript
package-lock.json
```

Lock files ensure everyone uses the same versions.

## Best Practices

### 1. Don't Commit Dependencies

Add to `.gitignore`:

```
# Python
venv/
__pycache__/
*.pyc
node_modules/

# JavaScript  
node_modules/
.pnp
```

Devs install dependencies locally:

```bash
# Python
pip install -r requirements.txt

# JavaScript
npm install
```

### 2. Separate Dev vs Production

```bash
# Python: dev dependencies only
pip install -r requirements-dev.txt

# JavaScript
npm install --save-dev eslint prettier
```

### 3. Document Why

Add comments to your dependency list:

```
# requirements.txt
flask==2.3.2  # Web framework
psycopg2==2.9.0  # PostgreSQL driver
pydantic==2.0.0  # Data validation
```

## Upgrading Dependencies

### Major Version Upgrade

Be careful with major version bumps:

```bash
# Check what changed
pip index versions package_name

# Read the changelog
# Visit https://github.com/owner/repo/releases

# Upgrade carefully
pip install --upgrade package_name

# Run tests thoroughly
pytest
```

If it breaks, roll back:

```bash
pip install package_name==old_version
```

## Questions?

Unsure about a dependency? Ask in `#engineering-chat` before adding.
