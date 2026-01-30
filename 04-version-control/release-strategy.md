# Release Strategy

**Versioning**: v1.0 | **Status**: Active | **Audience**: All Engineers

> **Releases are milestones**: They mark significant progress and deliver value to users.

This guide explains our release process, versioning strategy, and how to prepare and deploy releases at Better.sg.

---

## Semantic Versioning

We follow **Semantic Versioning (SemVer)** for all releases:

```
MAJOR.MINOR.PATCH
```

### Version Components

| Component | When to Increment | Example |
|-----------|-------------------|----------|
| **MAJOR** | Breaking changes that affect existing functionality | `1.0.0` → `2.0.0` |
| **MINOR** | New features (backward-compatible) | `1.2.0` → `1.3.0` |
| **PATCH** | Bug fixes (backward-compatible) | `1.2.3` → `1.2.4` |

### Examples

✅ `v1.0.0` - Initial stable release
✅ `v1.1.0` - Added new feature
✅ `v1.1.1` - Fixed bug in v1.1.0
✅ `v2.0.0` - Breaking changes (API redesign)

### Pre-Release Versions

For testing before official release:

- `v1.2.0-alpha.1` - Alpha (internal testing)
- `v1.2.0-beta.1` - Beta (external testing)
- `v1.2.0-rc.1` - Release Candidate (final testing)

---

## Release Types

### Major Release (X.0.0)

**When**: Breaking changes that require user action

**Examples**:
- API endpoint structure changes
- Database schema migrations
- Removing deprecated features
- Changing authentication method

**Timeline**: Every 6-12 months

**Process**:
1. Announce changes 4 weeks in advance
2. Provide migration guide
3. Update all documentation
4. Offer support during transition

---

### Minor Release (x.Y.0)

**When**: New features that don't break existing functionality

**Examples**:
- New API endpoints
- New UI components
- Performance improvements
- New configuration options

**Timeline**: Every 2-4 weeks

**Process**:
1. Gather features from `develop` branch
2. Create release branch
3. Test thoroughly
4. Deploy to production

---

### Patch Release (x.y.Z)

**When**: Bug fixes only

**Examples**:
- Security patches
- Critical bug fixes
- Minor UI fixes
- Documentation corrections

**Timeline**: As needed (can be immediate for critical issues)

**Process**:
1. Create hotfix branch from `main`
2. Fix the bug
3. Test the fix
4. Deploy immediately

---

## Release Process

### Step 1: Prepare for Release

#### 1.1 Create Release Branch

Branch from `develop` for minor/major releases:

```bash
git checkout develop
git pull origin develop
git checkout -b release/v1.2.0
```

For hotfixes, branch from `main`:

```bash
git checkout main
git pull origin main
git checkout -b hotfix/v1.1.1
```

#### 1.2 Update Version Numbers

Update version in:

- `package.json` (Node.js projects)
- `pyproject.toml` or `setup.py` (Python projects)
- `build.gradle` (Java projects)
- `pubspec.yaml` (Flutter projects)

```json
{
  "name": "better-app",
  "version": "1.2.0"
}
```

#### 1.3 Update CHANGELOG.md

Document all changes:

```markdown
# Changelog

## [1.2.0] - 2026-01-30

### Added
- User profile customization
- Dark mode support
- Export data feature

### Changed
- Improved search performance
- Updated UI design

### Fixed
- Login timeout issue
- Mobile layout bug

### Deprecated
- Old authentication method (will be removed in v2.0.0)
```

#### 1.4 Run Final Tests

- [ ] All unit tests pass
- [ ] Integration tests pass
- [ ] Manual QA testing complete
- [ ] Performance benchmarks met
- [ ] Security scan clean
- [ ] Cross-browser testing done

---

### Step 2: Create Pull Request

Open PR from release branch to `main`:

**Title**: `Release v1.2.0`

**Description**:
```
## Release v1.2.0

### What's New
- User profile customization
- Dark mode support
- Export data feature

### Bug Fixes
- Fixed login timeout issue
- Resolved mobile layout bug

### Breaking Changes
None

### Checklist
- [x] Version numbers updated
- [x] CHANGELOG.md updated
- [x] All tests passing
- [x] Documentation updated
- [x] Migration guide prepared (if needed)
```

---

### Step 3: Review & Approve

**Reviewers**: At least 2 senior engineers

**Review Checklist**:
- [ ] Version number is correct
- [ ] CHANGELOG is complete
- [ ] All tests pass
- [ ] No hardcoded secrets
- [ ] Documentation is updated
- [ ] Breaking changes are documented

---

### Step 4: Merge to Main

Once approved:

1. **Merge to `main`**
   ```bash
   git checkout main
   git merge --no-ff release/v1.2.0
   ```

2. **Tag the release**
   ```bash
   git tag -a v1.2.0 -m "Release version 1.2.0"
   git push origin v1.2.0
   ```

3. **Merge back to `develop`**
   ```bash
   git checkout develop
   git merge --no-ff release/v1.2.0
   git push origin develop
   ```

4. **Delete release branch**
   ```bash
   git branch -d release/v1.2.0
   git push origin --delete release/v1.2.0
   ```

---

### Step 5: Deploy

#### Production Deployment

1. **Deploy to staging first**
   ```bash
   ./scripts/deploy.sh staging v1.2.0
   ```

2. **Run smoke tests on staging**
   - Verify critical features work
   - Check error logs
   - Test integrations

3. **Deploy to production**
   ```bash
   ./scripts/deploy.sh production v1.2.0
   ```

4. **Monitor production**
   - Watch error rates
   - Check performance metrics
   - Verify user flows

---

### Step 6: Create GitHub Release

1. Go to **Releases** in GitHub
2. Click **Draft a new release**
3. Select tag: `v1.2.0`
4. Write release notes:

```markdown
## 🎉 What's New in v1.2.0

### Features
- ✨ User profile customization
- 🌙 Dark mode support
- 📥 Export data feature

### Improvements
- ⚡ Improved search performance
- 🎨 Updated UI design

### Bug Fixes
- 🐞 Fixed login timeout issue
- 📱 Resolved mobile layout bug

### Installation

```bash
npm install better-app@1.2.0
```

### Full Changelog
https://github.com/better-sg/app/compare/v1.1.0...v1.2.0
```

5. **Attach release assets** (if applicable):
   - Compiled binaries
   - Docker images
   - Documentation PDFs

6. Click **Publish release**

---

## Hotfix Process

For **urgent production bugs**:

### 1. Create Hotfix Branch

```bash
git checkout main
git pull origin main
git checkout -b hotfix/v1.1.1
```

### 2. Fix the Bug

```bash
git add .
git commit -m "fix: Resolve critical payment gateway bug"
```

### 3. Update Version & Changelog

Bump PATCH version: `1.1.0` → `1.1.1`

Update CHANGELOG.md:
```markdown
## [1.1.1] - 2026-01-30

### Fixed
- Critical bug in payment gateway causing transaction failures
```

### 4. Deploy Immediately

```bash
git checkout main
git merge --no-ff hotfix/v1.1.1
git tag -a v1.1.1 -m "Hotfix v1.1.1"
git push origin v1.1.1

# Merge back to develop
git checkout develop
git merge --no-ff hotfix/v1.1.1
git push origin develop

# Deploy
./scripts/deploy.sh production v1.1.1
```

### 5. Notify Team

Post in `#engineering-chat`:
```
🚨 Hotfix v1.1.1 deployed to production
Fixed: Critical payment gateway bug
Deployed at: 2026-01-30 14:30 SGT
```

---

## Release Checklist

### Before Release

- [ ] All features tested and working
- [ ] Code reviewed and approved
- [ ] Version numbers updated
- [ ] CHANGELOG.md updated
- [ ] Documentation updated
- [ ] Migration guide written (if breaking changes)
- [ ] Tests passing (unit, integration, e2e)
- [ ] Security scan complete
- [ ] Performance benchmarks met

### During Release

- [ ] Release branch created
- [ ] PR opened and reviewed
- [ ] Merged to `main`
- [ ] Tagged in Git
- [ ] Merged back to `develop`
- [ ] Release branch deleted

### After Release

- [ ] Deployed to staging
- [ ] Smoke tests passed
- [ ] Deployed to production
- [ ] Production monitoring active
- [ ] GitHub release created
- [ ] Team notified
- [ ] Users notified (if applicable)
- [ ] Rollback plan ready

---

## Rollback Strategy

If something goes wrong:

### Immediate Rollback

```bash
# Revert to previous version
./scripts/deploy.sh production v1.1.0
```

### Git Revert

If deployment script isn't available:

```bash
git checkout main
git revert HEAD
git push origin main
./scripts/deploy.sh production
```

### Post-Rollback

1. Investigate the issue
2. Fix in a new branch
3. Test thoroughly
4. Re-release as PATCH version

---

## Release Communication

### Internal (Team)

**Slack Announcement**:
```
🚀 Version 1.2.0 Released!

✨ New Features:
- User profile customization
- Dark mode support

🐞 Bug Fixes:
- Login timeout issue resolved

📅 Deployed: 2026-01-30 10:00 SGT
🔗 Release Notes: https://github.com/better-sg/app/releases/tag/v1.2.0
```

### External (Users)

**Email/Newsletter**:
```
Subject: 🎉 New Features Available: Dark Mode & More!

Hi there,

We're excited to announce version 1.2.0 is now live!

What's New:
• Dark mode for easier nighttime browsing
• Customizable user profiles
• Faster search performance

We've also fixed several bugs to improve your experience.

Check out the full release notes: [link]

Happy using!
- The Better.sg Team
```

---

## Version History Example

```
v2.0.0 - 2027-01-15 - Major redesign
v1.5.0 - 2026-11-01 - Added reporting features
v1.4.2 - 2026-10-15 - Fixed data export bug
v1.4.1 - 2026-10-02 - Security patch
v1.4.0 - 2026-09-20 - Added multi-language support
v1.3.0 - 2026-08-01 - Dashboard improvements
v1.2.0 - 2026-06-15 - Dark mode added
v1.1.0 - 2026-05-01 - User profiles
v1.0.0 - 2026-03-01 - Initial stable release
```

---

## Questions?

Not sure when to release? Ask in `#engineering-chat`.

Remember: **Good releases are predictable, well-tested, and clearly communicated.**
