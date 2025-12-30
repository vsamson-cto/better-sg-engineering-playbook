# Deployment Guide

Better.sg deploys to two primary platforms:
- **Google Firebase** (primary hosting for web apps, serverless functions)
- **Microsoft Azure** (dedicated hosting for critical services)

## 🌟 Google Firebase Deployments

### Firebase Services We Use

**Hosting**
- Static site hosting
- Single Page Applications (SPAs)
- Client-side rendered applications

**Firestore**
- Primary database for mobile apps
- Real-time data synchronization
- Document-based data model

**Firebase Functions**
- Serverless backend functions
- Triggered by HTTP, Pub/Sub, or events
- Auto-scaling

**Authentication**
- Firebase Auth for user management
- Multiple auth providers supported
- Email, Google, GitHub login

### Firebase Deployment Process

**Prerequisites**
```bash
# Install Firebase CLI
npm install -g firebase-tools

# Authenticate
firebase login

# Initialize project (done once)
firebase init
```

**Before Deploying**
1. Ensure all tests pass locally
2. Verify environment variables are set
3. Check that secrets are NOT in code
4. Run build: `npm run build` or equivalent
5. Test the build locally: `firebase serve`

**Deployment Steps**
```bash
# Deploy to staging first (if applicable)
firebase deploy --project=better-sg-staging

# After verification, deploy to production
firebase deploy --project=better-sg-prod

# Deploy only specific resources
firebase deploy --only hosting
firebase deploy --only functions
firebase deploy --only firestore:rules
```

**Post-Deployment**
1. Verify deployment in Firebase Console
2. Test critical user flows
3. Monitor logs for errors
4. Announce in #releases Slack channel

### Firebase Best Practices

**Security**
- Always use Firestore Security Rules
- Never expose Firebase API keys (they're public)
- Enable Firebase Authentication for restricted data
- Use Service Accounts for backend access

**Performance**
- Use Firebase CDN caching for static assets
- Index frequently queried Firestore fields
- Limit real-time listener subscriptions
- Use pagination for large datasets

**Cost Management**
- Monitor quota usage in Firebase Console
- Set up billing alerts
- Optimize database queries
- Remove unused functions

---

## 🔗 Microsoft Azure Deployments

### Azure Services We Use

**App Service**
- Hosting for dedicated services
- Auto-scaling based on load
- Multiple deployment slots (staging, production)

**SQL Database**
- Relational database for critical data
- Automated backups
- Managed service

**Key Vault**
- Secure credential storage
- Rotatable secrets
- Access logging

**Application Insights**
- Application monitoring
- Performance tracking
- Error analysis

### Azure Deployment Process

**Prerequisites**
```bash
# Install Azure CLI
choco install azure-cli  # Windows
brew install azure-cli   # macOS

# Login
az login

# Select subscription
az account set --subscription "subscription-id"
```

**Deployment via GitHub Actions (Recommended)**

Better.sg uses GitHub Actions for automated Azure deployments:

1. Push to main branch triggers workflow
2. Tests run automatically
3. Successful tests trigger Azure deployment
4. Logs available in GitHub Actions
5. Rollback available via GitHub

**Manual Deployment (If Needed)**
```bash
# Deploy to staging first
az webapp deployment source config-zip --resource-group bettersg-rg \
  --name bettersg-staging --src-path ./build.zip

# Verify in staging
# Then deploy to production
az webapp deployment source config-zip --resource-group bettersg-rg \
  --name bettersg-prod --src-path ./build.zip
```

**Database Migrations**
```bash
# Run migrations on staging first
az sql db update-restore-point --resource-group bettersg-rg \
  --server better-sg-staging --name maindb

# Apply migration
# Then replicate to production
```

**Post-Deployment**
1. Check Application Insights for errors
2. Monitor performance metrics
3. Test critical flows
4. Verify database integrity
5. Post update in #releases

### Azure Best Practices

**Security**
- Store all secrets in Key Vault
- Enable managed identities for service-to-service auth
- Use network security groups to restrict access
- Enable SSL/TLS (HTTPS only)
- Regular security audits

**Reliability**
- Use deployment slots for zero-downtime deploys
- Enable auto-scaling based on CPU/memory
- Configure health checks
- Set up alerts for critical metrics
- Regular backup testing

**Cost Management**
- Use App Service Plans appropriate for load
- Monitor resource utilization
- Delete unused resources
- Set up budget alerts
- Right-size database tier

---

## 🔄 Rollback Procedures

### Firebase Rollback
```bash
# View deployment history
firebase deployments:list

# Redeploy previous version
firebase deploy --project=better-sg-prod
```

### Azure Rollback
```bash
# Via GitHub Actions (Recommended)
# Trigger workflow with previous version

# Manual rollback
az webapp deployment slot swap --resource-group bettersg-rg \
  --name bettersg-prod --slot staging
```

---

## 📈 Monitoring & Alerts

**Firebase**
- Monitor in Firebase Console
- Check Cloud Logging
- Set up custom alerts in Cloud Monitoring

**Azure**
- Application Insights dashboard
- Metrics: Response time, error rate, availability
- Alerts configured in Azure Monitor
- Slack integration for alerts

---

## 🚨 Emergency Deployments

**Critical Security Fix**
1. Create emergency branch
2. Add minimal fix only
3. Fast-track code review (1 approval)
4. Deploy to prod immediately
5. Full testing in parallel
6. Document incident

**For Firebase**: Use `firebase deploy --force`
**For Azure**: Use deployment slots to swap quickly

---

**Always test in staging before production. When in doubt, ask in #infrastructure Slack channel.**
