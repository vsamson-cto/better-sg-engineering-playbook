# Communication Channels & Guidelines

Better.sg uses **Slack** as our primary communication platform for real-time collaboration.

## 📱 Slack Channels

### Primary Channels

**#engineering**
- Main engineering team discussions
- Technical questions and debates
- Code review discussions
- Architecture decisions

**#general**
- Better.sg-wide announcements
- Non-technical discussions
- Community updates

**#random**
- Off-topic conversations
- Memes, celebrations, casual chat
- No engineering discussions here

### Project-Specific Channels

**#project-[name]**
- Dedicated channels for specific projects
- Project planning and discussions
- Sprint updates
- Blocking issues

### Functional Channels

**#security** (Private)
- Security vulnerabilities and fixes
- Security reviews and discussions
- Sensitive security information only

**#infrastructure** (Private)
- Infrastructure changes and planning
- Firebase and Azure configuration
- Deployment notifications
- On-call rotation and incidents

**#releases**
- Deployment announcements
- Release notes
- Rollback notifications

## 📋 Slack Best Practices

### Threading

✅ **Use threads for**
- Follow-up discussions on a topic
- Questions about a specific message
- Long conversations
- Keeping channels organized

❌ **Don't create new threads for**
- Quick clarifications
- One-word responses (just reply)

### Messaging Guidelines

**Provide Context**
```
❌ "The test failed"
✅ "The test failed in deploy-service (link). Error: timeout on connection pool. Likely related to yesterday's config change."
```

**Be Respectful of Time Zones**
- Don't expect immediate responses
- Important info should be documented (GitHub/Confluence)
- Schedule sync meetings that work for most people

**Sensitive Information**
- Never post credentials, API keys, or secrets in Slack (even private channels)
- Use private channels for security discussions
- Reference documentation instead of pasting secrets

### Channel Etiquette

1. **Use appropriate channels** - Don't post general topics in #security
2. **Search before asking** - Your question might already be answered
3. **No DMs for work** - Keep discussions in channels for visibility
4. **Avoid @channel/@here** - Use sparingly for urgent issues only
5. **Pin important info** - Pin project guides, runbooks, and critical links

## 🚨 Urgent Communication

**Critical Issues**
- Use @channel in #infrastructure for production incidents
- Post in #releases for emergency deployments
- Follow incident response procedures

**Response Times**
- High priority: 1 hour
- Medium priority: 4 hours
- Low priority: 1 business day

## 📎 Linking to Documentation

Always include links in Slack messages:
- GitHub PR/Issue links
- Documentation links
- Runbook links
- Related discussions

```
✅ "Deployed new auth service (PR #234). See runbook: [link]. Rollback plan: [link]"
❌ "Deployed new auth service"
```

## 🔗 Slack Integrations

**GitHub Integration**
- PR notifications
- Issue updates
- Deployment notifications
- CI/CD status

**Firebase Alerts**
- Performance alerts
- Quota warnings
- Error reporting
- Billing notifications

**Azure Alerts**
- Resource health
- Performance metrics
- Deployment status
- Cost alerts

## 🔒 Privacy & Security

- Don't share production data in Slack
- Treat #security and #infrastructure as confidential
- Use private channels for sensitive discussions
- Delete sensitive screenshots immediately
- Review before posting external communications

---

**Remember: Slack is for real-time communication. For permanent decisions and documentation, use GitHub.**
