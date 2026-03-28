# DevSecOps for Git

## Contents

1. .gitignore  
2. Native Git Pre-Commit Hooks (Custom Scripts)  
3. Block commits with Gitleaks  
4. Gitleaks -> Repository & History Scanning  
5. Gitleaks in GitHub Actions  
6. Branch Protection Rules  
7. RBAC  
8. Mandatory Reviews  
9. CODEOWNERS  
10. Dependabot  

---

## .gitignore — First Line of Defense

### Purpose
Prevent sensitive files from ever being tracked by Git.

### Common Security Files to Ignore
```gitignore
.env
.env.*
*.pem
*.key
id_rsa
terraform.tfstate
.terraform/
node_modules/
dist/
```

### Demo
```bash
echo "AWS_SECRET_ACCESS_KEY=123" > .env
git status
```

Add `.gitignore`:
```bash
echo ".env" >> .gitignore
git status
```

✅ File is no longer tracked.

⚠️ `.gitignore` does NOT protect secrets already committed.

---

## Native Git Pre-Commit Hooks (Custom Script)

### What This Is
A **pre-commit hook** is a script located at:

```text
.git/hooks/pre-commit
```

Git executes it **automatically before every commit**.

### Exit Codes
| Code | Result |
|------|--------|
| 0    | Commit allowed |
| ≠0   | Commit blocked |

---

### Demo — Minimal Native Secret Detector

```bash
cat > .git/hooks/pre-commit << 'EOF'
#!/bin/bash

echo "🔍 Running native pre-commit hook..."

if git diff --cached | grep -i "secret"; then
  echo "❌ Secret detected. Commit blocked."
  exit 1
fi

echo "✅ Commit passed security checks."
exit 0
EOF
```

Make executable:
```bash
chmod +x .git/hooks/pre-commit
```

Test:
```bash
echo "my_secret=123" > test.txt
git add test.txt
git commit -m "test commit"
```

❌ Commit blocked.

---

## Gitleaks — Blocking Commits (Native Hook)

### Replace Pre-Commit Hook with Gitleaks

1. Install pre-commit from https://pre-commit.com/#install
2. Create a `.pre-commit-config.yaml` file at the root of your repository with the following content:

   ```
   repos:
     - repo: https://github.com/gitleaks/gitleaks
       rev: v8.24.2
       hooks:
         - id: gitleaks
   ```
   
3. Auto-update the config to the latest repos' versions by executing `pre-commit autoupdate`
4. Install with `pre-commit install`
5. Now you're all set!

### Demo — Block a Secret Commit

```bash
echo "AWS_SECRET_ACCESS_KEY=AKIA123456789" > secrets.env
git add secrets.env
git commit -m "adding secrets"
```

❌ Commit blocked.

---

## Gitleaks — Repository & History Scanning

1. Create a custom rules file - `custom-rules.toml`

```
[[rules]]
id = "generic-password"
description = "Detect any PASSWORD assignment"
regex = '''(?i)password\s*=\s*["'][^"']+["']'''
tags = ["password", "custom"]
```

2. Run the gitleaks command

`gitleaks detect --config custom-rules.toml`

---

## Gitleaks in GitHub Actions

### GitHub Action

Check out the official [Gitleaks GitHub Action](https://github.com/gitleaks/gitleaks-action)

```
name: gitleaks
on: [pull_request, push, workflow_dispatch]
jobs:
  scan:
    name: gitleaks
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
      - uses: gitleaks/gitleaks-action@v2
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          GITLEAKS_LICENSE: ${{ secrets.GITLEAKS_LICENSE}} # Only required for Organizations, not personal accounts.
```

---

## Branch Protection Rules

Enforce:
- No direct pushes to `main`
- Required pull requests
- Required status checks
- No force pushes

---

## RBAC — Least Privilege

| Role | Permissions |
|-----|-------------|
| Admin | Repo settings |
| Maintainer | Merge PRs |
| Developer | PR only |
| Auditor | Read-only |

---

## Mandatory Reviews

Best practices:
- Minimum 1–2 reviewers
- Code owners for sensitive paths
- Security review for auth, infra, CI

---

## CODEOWNERS

```text
/.github/ @security-team
/terraform/ @cloud-team
```

---

## Dependabot

```yaml
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
```

---
