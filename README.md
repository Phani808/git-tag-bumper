# git-auto-version 🏷️

A lightweight **bash script** to automatically bump and tag your repository versions following [Semantic Versioning (SemVer)](https://semver.org).
Perfect for CI/CD pipelines (GitHub Actions, GitLab, Jenkins, etc.) or local version management.

---

## 🚀 Features

* 🧮 Auto-detects current version tag (`v1.2.3`)
* 🔼 Increments **major**, **minor**, or **patch**
* 🔒 Prevents duplicate tags
* 🌐 Works locally **and** in GitHub Actions
* ⚙️ Automatically pushes new tags to remote

---

## 🧩 Installation

Clone or copy the script into your project:

```bash
git clone https://github.com/YOUR_USERNAME/git-auto-version.git
cd git-auto-version
chmod +x bump-version.sh
```

Or add it to your own repo under `scripts/`:

```bash
mkdir -p scripts
cp bump-version.sh scripts/
chmod +x scripts/bump-version.sh
```

---

## ⚙️ Usage

### 1️⃣ Run Locally

```bash
./scripts/bump-version.sh -v patch
```

**Options:**

* `-v major` → `v1.2.3` → `v2.0.0`
* `-v minor` → `v1.2.3` → `v1.3.0`
* `-v patch` → `v1.2.3` → `v1.2.4`

**Output Example:**

```
Current Version: v1.2.3
(patch) updating v1.2.3 to v1.2.4
Tagging commit as v1.2.4
```

---

### 2️⃣ Use in GitHub Actions

Add this workflow to `.github/workflows/bump-version.yml`:

```yaml
name: Bump Version

on:
  workflow_dispatch:
    inputs:
      bump:
        description: 'Version type'
        required: true
        default: 'patch'
        type: choice
        options:
          - major
          - minor
          - patch

permissions:
  contents: write

jobs:
  bump:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Setup Git identity
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"

      - name: Run version bump
        run: ./scripts/bump-version.sh -v ${{ github.event.inputs.bump }}
```

Now you can manually trigger the workflow from GitHub UI and choose `major`, `minor`, or `patch`.

---

## 🔍 Example Output

```
Current Version: v0.2.1
(minor) updating v0.2.1 to v0.3.0
Tagging commit as v0.3.0
Pushing tags...
```

---

## 💡 Notes

* Works with tags that start with or without `v` (e.g., `v1.2.3` or `1.2.3`)
* Safe to re-run — won’t overwrite existing tags
* Uses `git tag --points-at HEAD` to check if commit already has a tag
* Compatible with Linux, macOS, and CI runners
