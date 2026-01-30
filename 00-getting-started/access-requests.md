# Access Requests 🔑

This guide helps you request access to the tools, systems, and repositories you need to contribute at Better.sg.

## Overview

Better.sg uses multiple platforms and tools. Access is granted on a **need-to-know and need-to-use basis** to protect our systems and user data.

## General Access Request Process

### How to Request Access

1. **Identify what you need** — Check the sections below
2. **Submit your request** — Use the appropriate channel
3. **Wait for approval** — Typically 1-3 business days
4. **Confirm access** — Test that you can log in

### Who to Contact

| Type of Access | Contact | Channel |
|----------------|---------|--------|
| **GitHub Repositories** | Project Lead or CTO | `#engineering` on Slack |
| **Cloud Services** | CTO or Infrastructure Lead | `#engineering` on Slack |
| **Project Management Tools** | Project Lead | Project-specific Slack channel |
| **Monitoring & Logging** | CTO or DevOps Lead | `#engineering` on Slack |
| **Slack Channels** | Channel Owner | Request directly in Slack |

## GitHub Access

### What You'll Need

- [ ] GitHub account (with 2FA enabled)
- [ ] Your GitHub username
- [ ] The specific repository you need access to
- [ ] Your role (developer, reviewer, etc.)

### How to Request

**In `#engineering` Slack channel:**

```
Hi! I’d like access to [repository-name].

GitHub username: @your-username
Role: Developer
Reason: Working on [feature/issue]
```

### Access Levels

- **Read**: View code, issues, discussions
- **Triage**: Manage issues and pull requests
- **Write**: Push changes, create branches
- **Maintain**: Manage settings (limited)
- **Admin**: Full control (for maintainers only)

Most contributors receive **Write** access.

## Cloud Services Access

### Firebase

**Required for:** Deploying web apps, accessing Firestore, managing authentication

**Request Process:**
1. Share your Google account email
2. Specify project name (e.g., `bettersg-prod`)
3. Request appropriate role:
   - **Viewer**: Read-only access
   - **Editor**: Deploy and manage
   - **Owner**: Full control (rarely granted)

**Submit request in:** `#engineering` on Slack

### Azure

**Required for:** Cloud infrastructure, VMs, databases

**Request Process:**
1. Share your Microsoft account email
2. Specify resource group or subscription
3. Request role:
   - **Reader**: View resources
   - **Contributor**: Manage resources
   - **Owner**: Full control (rarely granted)

**Submit request in:** `#engineering` on Slack

### MongoDB Atlas

**Required for:** Database access for projects using MongoDB

**Request Process:**
1. Share your email address
2. Specify project/cluster name
3. Request role:
   - **Read-only**: View data
   - **Read-write**: Modify data
   - **Admin**: Manage cluster (rarely granted)

**Submit request in:** Project-specific Slack channel or `#engineering`

## Project Management Tools

### Notion

**Required for:** Project documentation, roadmaps, meeting notes

**Request Process:**
1. Share your email
2. Specify workspace or page
3. Request will be approved by project lead

**Submit request in:** Project-specific Slack channel

### Jira / Linear / Other PM Tools

**Request Process:**
1. Check with project lead which PM tool is used
2. Provide your email
3. Project lead will send invite

**Submit request in:** Project-specific Slack channel

## Communication Channels

### Slack

**Public Channels**: You can join these yourself
- `#engineering`
- `#general`
- `#random`
- `#introductions`

**Private Channels**: Request access from channel owner or admin
- Project-specific channels
- Security channels
- Leadership channels

### Email Lists (if applicable)

Some projects use Google Groups or mailing lists.

**Request Process:**
1. Ask project lead for the email list
2. Provide your email address
3. You'll receive an invitation

## Monitoring & Logging Access

### Sentry (Error Monitoring)

**Required for:** Viewing application errors and performance issues

**Request Process:**
1. Share your email
2. Specify project
3. Request role:
   - **Member**: View issues
   - **Admin**: Manage settings (rarely granted)

**Submit request in:** `#engineering` on Slack

### DataDog / Other Monitoring Tools

**Request Process:**
1. Confirm which tool the project uses
2. Provide your email
3. CTO or DevOps lead will grant access

**Submit request in:** `#engineering` on Slack

## Secrets & Credentials

🚨 **Important**: 
- **Never share secrets in Slack, email, or other channels**
- Use **1Password, Bitwarden, or project-specific secret manager**
- If you need credentials, request secure sharing method

### How to Request Credentials

1. Ask in `#engineering` for secure method
2. CTO or project lead will share via:
   - 1Password shared vault
   - Encrypted file transfer
   - In-person handoff (if local)

**Never:**
- Share secrets in plain text
- Store credentials in code
- Commit secrets to Git

Review: [Secrets Management Guide](../03-security-and-compliance/secrets-management.md)

## Access Review & Revocation

### When Access is Removed

- When you leave a project
- After prolonged inactivity (90+ days)
- Upon offboarding from Better.sg
- Security incidents or policy violations

### Periodic Access Reviews

Better.sg conducts **quarterly access reviews** to ensure:
- Only active contributors have access
- Access levels are appropriate
- Unused accounts are deactivated

## Troubleshooting Access Issues

### I requested access but haven't received it

- Wait 1-3 business days
- Check your spam folder for invitation emails
- Ping the person you requested from in Slack
- Escalate to CTO if no response after 3 days

### My access was revoked unexpectedly

- Contact the project lead or CTO
- May be due to:
  - Inactivity
  - Quarterly access review
  - Project completion

### I can't access a tool after receiving approval

- Ensure you're using the correct account (email)
- Check if 2FA is required
- Try logging out and back in
- Contact IT support or project lead

## Security Best Practices

✅ **Do:**
- Enable 2FA on all accounts
- Use strong, unique passwords
- Log out of shared devices
- Report suspicious access attempts
- Request only the access you need

❌ **Don't:**
- Share your credentials with anyone
- Use personal accounts for Better.sg work (if avoidable)
- Leave accounts logged in on public computers
- Grant access to others without approval

## FAQ

**Q: How long does access approval take?**  
A: Typically 1-3 business days, depending on the system and availability of approvers.

**Q: Can I request access for someone else?**  
A: No, individuals must request their own access. Project leads can nominate team members.

**Q: What if I need urgent access?**  
A: Contact CTO or project lead directly in Slack with “urgent” in your message. Explain why it's time-sensitive.

**Q: Do I need separate access for staging and production?**  
A: Yes, production access is more restricted. Start with staging/development access.

**Q: What happens to my access if I take a break from contributing?**  
A: Access may be revoked after 90 days of inactivity. You can request reinstatement when you return.

## Summary Checklist

Before requesting access:

- [ ] I have a GitHub account with 2FA enabled
- [ ] I know which specific resources I need
- [ ] I understand my role and responsibilities
- [ ] I’ve reviewed security best practices
- [ ] I know who to contact for approval

**Next Step**: → [Your First Contribution](your-first-contribution.md)

---

**Questions?** Ask in `#engineering` on Slack or open an issue in this playbook.
