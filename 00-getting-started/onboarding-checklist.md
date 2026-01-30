# Onboarding Checklist 🚀

Welcome to Better.sg Engineering! This checklist will help you get set up and ready to contribute.

## Before You Start

- [ ] **Read the [Welcome Guide](welcome.md)** — Understand our culture and values
- [ ] **Review the [Engineering Playbook](../README.md)** — Get familiar with our standards

## Account Setup

### Required Accounts

- [ ] **GitHub Account**
  - Create account at [github.com](https://github.com)
  - Enable 2FA (Two-Factor Authentication)
  - Add your Singapore-based email if applicable

- [ ] **Slack Workspace**
  - Join Better.sg Slack workspace
  - Introduce yourself in `#introductions`
  - Join `#engineering` channel
  - Join relevant project channels

### Optional But Recommended

- [ ] **AI Coding Tools** (if you use them)
  - GitHub Copilot
  - Claude / ChatGPT
  - Review our [AI-Assisted Coding Guidelines](../02-development-practices/ai-assisted-coding.md)

## Development Environment

### Essential Tools

- [ ] **Git**
  - Install Git: [git-scm.com](https://git-scm.com/)
  - Configure your name: `git config --global user.name "Your Name"`
  - Configure your email: `git config --global user.email "your.email@example.com"`

- [ ] **Code Editor / IDE**
  - VS Code (recommended), IntelliJ, or your preferred editor
  - Install relevant extensions for your language

- [ ] **Terminal / Command Line**
  - Bash, Zsh, or PowerShell
  - Basic familiarity with command-line operations

### Project-Specific Setup

- [ ] **Clone the repository you'll be working on**
  ```bash
  git clone https://github.com/better-sg/[project-name].git
  ```

- [ ] **Install project dependencies**
  - Follow the project's README.md instructions
  - Check for language-specific requirements (Node.js, Python, etc.)

- [ ] **Run the project locally**
  - Ensure you can build and run the project
  - Ask for help if you encounter issues

## Access Requests

- [ ] **Complete [Access Requests](access-requests.md)** for:
  - GitHub repository access
  - Cloud services (Firebase, Azure, etc.)
  - Project management tools
  - Monitoring and logging systems

## Security Setup

- [ ] **Enable 2FA on all accounts** (GitHub, Google, etc.)
- [ ] **Set up SSH keys for GitHub**
  - Generate SSH key: `ssh-keygen -t ed25519 -C "your.email@example.com"`
  - Add to GitHub: Settings → SSH and GPG keys
  - Test connection: `ssh -T git@github.com`

- [ ] **Never commit secrets**
  - Review [Secrets Management Guide](../03-security-and-compliance/secrets-management.md)
  - Use environment variables for sensitive data
  - Install `git-secrets` or similar tools

## Communication & Collaboration

- [ ] **Slack Etiquette**
  - Use threads for discussions
  - Set your timezone in profile
  - Use status to indicate availability
  - Review [Communication Guidelines](../06-project-management/communication-guidelines.md)

- [ ] **Meeting & Standup (if applicable)**
  - Add Better.sg calendar to your schedule
  - Join weekly engineering sync (if you're available)
  - Async standups in Slack are preferred

## First Tasks

- [ ] **Explore the codebase**
  - Read through project documentation
  - Understand folder structure
  - Look for `CONTRIBUTING.md` in the project

- [ ] **Pick a "good first issue"**
  - Look for issues labeled `good first issue` or `help wanted`
  - Ask in `#engineering` if you need help finding one

- [ ] **Follow [Your First Contribution Guide](your-first-contribution.md)**
  - Learn our Git workflow
  - Submit your first pull request
  - Experience our code review process

## Learning Resources

- [ ] **Review Key Playbook Sections**
  - [Principles & Values](../01-principles-and-values/)
  - [Development Practices](../02-development-practices/)
  - [Security Guidelines](../03-security-and-compliance/)
  - [Version Control](../04-version-control/)

- [ ] **Familiarize with Tools We Use**
  - Git & GitHub
  - Project-specific frameworks
  - Testing tools
  - CI/CD pipelines

## Questions & Support

### Getting Help

- **Technical Issues**: Ask in `#engineering` on Slack
- **Access Problems**: Contact project lead or CTO
- **General Questions**: Post in `#help` or `#general`
- **Security Concerns**: See [Vulnerability Response](../03-security-and-compliance/vulnerability-response.md)

### Who to Contact

- **CTO / Tech Lead**: Overall technical direction
- **Project Maintainer**: Project-specific questions
- **Peers**: Day-to-day collaboration

## Checklist Complete? 🎉

Once you've completed this checklist:

1. **Join a project**: Ask in `#engineering` for current projects
2. **Start contributing**: Pick your first issue
3. **Stay connected**: Engage with the community

**Next Step**: → [Your First Contribution](your-first-contribution.md)

---

**Questions?** Open an issue in this playbook or ask in `#engineering` on Slack.

**Welcome to the team!** 🙌
