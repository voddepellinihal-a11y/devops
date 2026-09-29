# DevOps Assignment - Static HTML Project

This repository contains a simple 2-file static HTML registration form project with CI/CD configured via Jenkins.

## Project Structure

```
.
├── registartion.html    # Main registration form (complete HTML)
├── registartion         # HTML fragment (incomplete)
├── Jenkinsfile          # Jenkins pipeline for CI validation
├── .gitignore           # Git ignore rules
└── README.md            # This file
```

## Git Branching Workflow

### Branch Structure

- **main** - Protected branch, only updated via Pull Requests
- **feature/amogh** - Feature branch for Amogh
- **feature/varalamanichandrasai** - Feature branch for varalamanichandrasai
- **feature/yashwanth** - Feature branch for Yashwanth
- **feature/admin** - Feature branch for Admin (voddepellinihal-a11y)

### Workflow Rules

1. **Each member works on their own feature branch**
   - Never commit directly to `main`
   - Create feature branches from `main`

2. **Pull Request Process**
   - Members push changes to their feature branch
   - Create PR from feature branch → `main`
   - **Admin (voddepellinihal-a11y) reviews member PRs**
   - Admin must also use `feature/admin` branch and create PR
   - Another teammate reviews the admin PR

3. **Branch Protection** (configure manually in GitHub)
   - Require PR reviews before merging
   - Require status checks (Jenkins) to pass
   - Prevent force pushes to `main`

### Git Commands

```bash
# Create and switch to feature branch
git checkout -b feature/amogh main

# Make changes, commit
git add .
git commit -m "feat: description of changes"

# Push to remote
git push origin feature/amogh

# Create PR via GitHub UI: feature/amogh → main
```

## Jenkins CI Pipeline

The Jenkinsfile defines a pipeline with three stages:

1. **Checkout** - Clones the repository
2. **Validate HTML** - Performs structural validation on HTML files:
   - Checks for DOCTYPE declaration
   - Verifies required tags (<html>, <head>, <body>) exist
   - Validates matching open/close tags
   - Reports warnings for incomplete fragments
3. **Report** - Outputs success/failure status

### Pipeline Features

- **No external dependencies** - Uses only shell/Groovy built-ins
- **Validates HTML structure** - Checks for well-formed markup
- **Clear success/failure reporting** - Explicit pass/fail status
- **Handles HTML fragments** - Warns but doesn't fail on incomplete files

### Local Validation (optional)

To validate HTML locally before pushing:

```bash
# Basic structure check
grep -l "<!DOCTYPE html>" *.html
grep -L "</html>" *.html  # Should return nothing for complete files
```

## Required Manual Configuration

### GitHub Repository Settings

These **must be configured manually** in GitHub (not automated):

1. **Branch Protection Rules** (Settings → Branches → Add rule)
   - Branch pattern: `main`
   - ✅ Require a pull request before merging
   - ✅ Require approvals (1 minimum)
   - ✅ Require status checks to pass before merging
   - ✅ Include administrators

2. **Collaborators** (Settings → Collaborators)
   - Add: Amogh-07, varalamanichandrasai223-wb, Yashwanth7065
   - Set appropriate permissions (Write access)

3. **Webhooks** (Settings → Webhooks)
   - Add Jenkins webhook for PR events
   - Payload URL: `http://your-jenkins-url/github-webhook/`
   - Content type: `application/json`
   - Events: Pull requests, Push

### Jenkins Configuration

These **must be configured manually** in Jenkins:

1. **Create Pipeline Job**
   - New Item → Pipeline
   - Name: `devops-html-validation`
   - Pipeline script from SCM → Git
   - Repository URL: `https://github.com/voddepellinihal-a11y/devops.git`
   - Branch: `*/main` (or `*` for all branches)

2. **Jenkins Credentials**
   - Add GitHub credentials (username/token) if repo is private
   - Configure in Credentials → System → Global credentials

3. **Build Triggers**
   - ✅ GitHub hook trigger for GITScm polling
   - Or: Poll SCM with schedule `H/5 * * * *`

4. **Pipeline Script Path**
   - Script Path: `Jenkinsfile`

## Team Members

| Member | GitHub Username | Feature Branch |
|--------|-----------------|----------------|
| Amogh | Amogh-07 | feature/amogh |
| Varala Manichandrasai | varalamanichandrasai223-wb | feature/varalamanichandrasai |
| Yashwanth | Yashwanth7065 | feature/yashwanth |
| Admin | voddepellinihal-a11y | feature/admin |

## Notes

- The file `registartion` (no extension) is an incomplete HTML fragment - validation will warn but not fail
- No npm, Node.js, Python, or other runtime dependencies required
- Jenkins runs on any agent with basic shell/Groovy support
- All validation is static analysis - no browser testing

## Verification Checklist

- [ ] Repository cloned and files present
- [ ] `.gitignore` created
- [ ] `Jenkinsfile` created and valid
- [ ] `README.md` created with workflow docs
- [ ] Feature branches created locally
- [ ] Feature branches pushed to GitHub
- [ ] Branch protection configured in GitHub
- [ ] Collaborators added in GitHub
- [ ] Jenkins job created and connected
- [ ] Webhook configured in GitHub
- [ ] Test PR created and validated