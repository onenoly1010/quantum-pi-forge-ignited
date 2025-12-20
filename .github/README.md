# .github Directory

This directory contains GitHub-specific configuration files for the Quantum Pi Forge project, including issue templates, pull request templates, and automated workflows.

## 📁 Directory Structure

```
.github/
├── ISSUE_TEMPLATE/
│   ├── bug_report.yml           # Bug report template
│   ├── feature_request.yml      # Feature request template
│   ├── security_vulnerability.yml # Security report template
│   ├── general_issue.yml        # General issue template
│   └── config.yml               # Issue template configuration
├── workflows/
│   ├── ci-cd.yml                # CI/CD pipeline
│   ├── agent-pr-review.yml      # Automated PR review
│   └── auto-triage-issues.yml   # Issue triage automation
├── PULL_REQUEST_TEMPLATE.md     # PR template
├── WORKFLOW_SUGGESTIONS.md      # Additional workflow ideas
└── README.md                    # This file
```

## 🎯 Purpose

This directory enables the **agent-based development system** that:

- 🤖 **Automates** code reviews and issue triage
- 🏷️ **Labels** issues and PRs automatically
- 🔀 **Routes** work to appropriate team members
- 🔒 **Scans** for security vulnerabilities
- ✅ **Validates** code quality and tests
- 📊 **Analyzes** changes and provides feedback

## 📝 Issue Templates

### Bug Report (`bug_report.yml`)
For reporting bugs and issues. Auto-labeled with `bug`, `needs-triage`, and `agent-review`.

**Key Features:**
- Structured bug information collection
- Severity assessment
- Component identification
- Automatic routing to relevant team

### Feature Request (`feature_request.yml`)
For proposing new features or enhancements. Auto-labeled with `enhancement` and `agent-review`.

**Key Features:**
- Problem statement capture
- Solution proposal
- Priority assessment
- Implementation complexity estimation

### Security Vulnerability (`security_vulnerability.yml`)
For reporting security issues privately. Auto-labeled with `security`, `critical`, `confidential`.

**Key Features:**
- Private security disclosure
- Vulnerability type classification
- Impact assessment
- Proof of concept section

### General Issue (`general_issue.yml`)
For questions, discussions, and other issues. Auto-labeled with `question` and `needs-triage`.

**Key Features:**
- Flexible issue category
- Context gathering
- Documentation linkage

## 🔄 Pull Request Template

The PR template (`PULL_REQUEST_TEMPLATE.md`) enforces a comprehensive handoff process between developers, agents, and reviewers.

**Sections:**
- Description and motivation
- Type of change
- Testing details
- Agent handoff checklist
- Security review
- Documentation updates
- Deployment notes

## ⚙️ Workflows

### CI/CD Pipeline (`ci-cd.yml`)

**Runs on:** Push to main/develop, PRs to main/develop

**Jobs:**
- Code quality analysis (linting, formatting)
- Security vulnerability scanning
- Backend API testing
- Frontend validation
- Agent code review
- Deployment readiness check

**Benefits:**
- Catches issues early
- Ensures code quality
- Validates security
- Confirms deployment readiness

### Agent-Assisted PR Review (`agent-pr-review.yml`)

**Runs on:** PR opened, synchronized, reopened

**Jobs:**
- Auto-labeling based on changes
- PR change analysis
- Breaking change detection
- Security pattern checking
- Template compliance verification
- Automatic reviewer assignment

**Benefits:**
- Instant feedback
- Proper routing
- Consistent reviews
- Security awareness

### Auto-triage Issues (`auto-triage-issues.yml`)

**Runs on:** Issue opened, labeled, daily schedule

**Jobs:**
- Issue content analysis
- Automatic labeling
- Security prioritization
- First-time contributor welcome
- Expert routing
- Stale issue checking

**Benefits:**
- Fast triage
- Proper categorization
- Priority handling
- Community engagement

## 🔧 Configuration

### Required Secrets

Configure in `Settings → Secrets and variables → Actions`:

- `GITHUB_TOKEN` - Auto-provided by GitHub
- `NETLIFY_AUTH_TOKEN` - For Netlify deployments *(optional)*
- `NETLIFY_QPF_SITE_ID` - Netlify site ID *(optional)*

### Recommended Labels

Create these labels for optimal agent routing:

**Type Labels:**
```
bug, enhancement, question, documentation, security
```

**Component Labels:**
```
backend, frontend, pi-network, ci/cd
```

**Priority Labels:**
```
priority:low, priority:medium, priority:high, priority:critical
```

**Size Labels:**
```
size/XS, size/S, size/M, size/L, size/XL
```

**Status Labels:**
```
needs-triage, in-progress, blocked, ready-for-review
```

**Special Labels:**
```
good first issue, help wanted, automated, agent-review
```

### Workflow Permissions

Enable in `Settings → Actions → General`:

- ✅ Read and write permissions
- ✅ Allow GitHub Actions to create and approve pull requests

## 📚 Additional Resources

- **[WORKFLOW_SUGGESTIONS.md](WORKFLOW_SUGGESTIONS.md)** - Ideas for additional workflows
- **[../CONTRIBUTING.md](../CONTRIBUTING.md)** - Full contribution guidelines
- **[GitHub Actions Docs](https://docs.github.com/en/actions)** - Official documentation

## 🚀 Getting Started

1. **Review the templates** - Understand what information is required
2. **Check workflows** - See what automation is running
3. **Configure secrets** - Set up required authentication
4. **Create labels** - Enable proper routing
5. **Test the system** - Create a test issue or PR

## 🛠️ Customization

### Modifying Templates

Edit YAML files in `ISSUE_TEMPLATE/` to:
- Add/remove fields
- Change validation rules
- Adjust labels
- Modify routing logic

### Extending Workflows

1. Copy a suggested workflow from `WORKFLOW_SUGGESTIONS.md`
2. Customize for your needs
3. Save to `workflows/` directory
4. Test with `workflow_dispatch`

### Adjusting Agent Behavior

Modify workflow files to:
- Change labeling logic
- Adjust routing rules
- Add custom checks
- Integrate external tools

## 📊 Monitoring

### View Workflow Runs
```
Repository → Actions tab
```

### Check Issue/PR Activity
```
Repository → Issues/Pull Requests tab
```

### Review Agent Comments
Look for comments from `github-actions[bot]` on issues and PRs

## 🤝 Contributing

To improve the agent system:

1. Test changes locally with `act` (GitHub Actions local runner)
2. Submit PRs with workflow improvements
3. Document new features in this README
4. Share feedback in discussions

## ❓ Troubleshooting

**Workflows not triggering?**
- Check branch names in trigger conditions
- Verify file paths are correct
- Review permissions settings

**Labels not being applied?**
- Ensure labels exist in repository
- Check workflow logs for errors
- Verify workflow permissions

**Templates not showing up?**
- Wait a few minutes after push
- Clear browser cache
- Check YAML syntax is valid

**Need help?**
- Open a general issue
- Check workflow logs
- Review GitHub Actions documentation

---

**Maintained by:** Quantum Pi Forge Team  
**Last Updated:** 2025-12-20  
**Version:** 1.0.0
