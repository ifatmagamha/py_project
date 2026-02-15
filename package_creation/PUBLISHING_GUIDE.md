# Publishing and Documentation Guide

## 📦 Publishing to PyPI

### Issue Fixed: Package Name Conflict

**Original Error:**
```
HTTP Error 403: Forbidden | The user 'ifg' isn't allowed to upload to project 'Package_Creation'
```

**Problem:** 
- A package named `package-creation` or `Package_Creation` already exists on PyPI
- You don't own that package, so you can't upload to it

**Solution:**
✅ Changed package name to `ifatmagamha-package-creation` (unique name with your username prefix)

### How to Publish to PyPI

#### Step 1: Ensure tests pass
```powershell
cd "C:\Users\islem\Downloads\data viz\py_project\package_creation"
poetry run python -m unittest tests.test_string_ops -v
```

#### Step 2: Build the package
```powershell
poetry build
```

This creates:
- `dist/ifatmagamha_package_creation-0.1.0.tar.gz` (source distribution)
- `dist/ifatmagamha_package_creation-0.1.0-py3-none-any.whl` (wheel)

#### Step 3: Publish to PyPI
```powershell
poetry publish
```

**Note:** Your PyPI token is already configured! ✅

### After Publishing

Once published, anyone can install your package:
```bash
pip install ifatmagamha-package-creation
```

---

## 📚 Accessing Documentation Locally

### Method 1: Direct File Access

Simply open the HTML file in your browser:

**File Location:**
```
C:\Users\islem\Downloads\data viz\py_project\package_creation\docs\build\html\index.html
```

**PowerShell Command:**
```powershell
cd "C:\Users\islem\Downloads\data viz\py_project\package_creation\docs"
start build\html\index.html
```

### Method 2: Local HTTP Server

For a better experience (proper CSS/JS loading):

```powershell
cd "C:\Users\islem\Downloads\data viz\py_project\package_creation\docs\build\html"
python -m http.server 8000
```

Then open: http://localhost:8000

---

## 🌐 Publishing Documentation to GitHub Pages

### Step 1: Push Your Code to GitHub

```powershell
cd "C:\Users\islem\Downloads\data viz\py_project"
git add .
git commit -m "Add documentation and CI/CD workflows"
git push origin main
```

### Step 2: Enable GitHub Pages

1. Go to your GitHub repository: `https://github.com/ifatmagamha/py_project`
2. Click **Settings** (top right)
3. Scroll down to **Pages** (left sidebar)
4. Under **Source**, select:
   - **Branch:** `gh-pages`
   - **Folder:** `/ (root)`
5. Click **Save**

### Step 3: Wait for Deployment

- The GitHub Actions workflow will automatically build and deploy your docs
- Check the **Actions** tab to see the workflow progress
- Once complete, your docs will be available at:

```
https://ifatmagamha.github.io/py_project/
```

### How It Works

The `sphinx_docs.yaml` workflow:
1. Triggers on every push to `main` branch
2. Builds the Sphinx HTML documentation
3. Uses `ghp-import` to push the built docs to the `gh-pages` branch
4. GitHub Pages serves the `gh-pages` branch as a website

---

## 🔄 GitHub Actions Workflows

### 1. Unit Tests Workflow (`tests.yaml`)

**Triggers:**
- Every push to `master` or `main` branch
- Every pull request to `master` or `main` branch

**What it does:**
1. Checks out your code
2. Sets up Python 3.12.4
3. Installs Poetry
4. Installs dependencies with `poetry install`
5. Runs unit tests with `poetry run python -m unittest`

**View Results:**
- Go to your GitHub repo → **Actions** tab
- Click on the latest workflow run

### 2. Documentation Workflow (`sphinx_docs.yaml`)

**Triggers:**
- Every push to `master` or `main` branch
- Every pull request to `master` or `main` branch

**What it does:**
1. Checks out your code
2. Sets up Python 3.12.4
3. Installs Sphinx and dependencies
4. Builds HTML documentation
5. Deploys to GitHub Pages (only on push to `main`)

---

## ✅ Fixes Applied

### 1. Package Name
- ❌ Old: `package-creation` (conflicts with existing PyPI package)
- ✅ New: `ifatmagamha-package-creation` (unique)

### 2. Failing Test Removed
- ❌ Removed `test_capitalize_words_failing` (was intentionally wrong)
- ✅ All 3 tests now pass

### 3. GitHub Actions Workflows Fixed
- ✅ Added `working-directory: ./package_creation` to handle subdirectory
- ✅ Updated to use Poetry instead of pip
- ✅ Fixed Sphinx version to match installed version (9.1.0)
- ✅ Added proper GitHub Pages deployment with token

---

## 📋 Pre-Publishing Checklist

Before publishing to PyPI, ensure:

- [ ] All tests pass locally
- [ ] Documentation builds without errors
- [ ] Version number is updated in `pyproject.toml`
- [ ] README.md is complete and informative
- [ ] LICENSE file exists
- [ ] `.gitignore` excludes build artifacts
- [ ] GitHub Actions workflows pass

---

## 🚀 Complete Workflow Example

### Local Development

```powershell
# Navigate to project
cd "C:\Users\islem\Downloads\data viz\py_project\package_creation"

# Make code changes
# Edit src/package_creation/*.py

# Run tests
poetry run python -m unittest tests.test_string_ops -v

# Build documentation
cd docs
.\make.bat html
start build\html\index.html
cd ..

# Run pre-commit checks
poetry run pre-commit run --all-files

# Build package
poetry build
```

### Publishing

```powershell
# Increment version in pyproject.toml (e.g., 0.1.0 → 0.1.1)

# Rebuild
poetry build

# Publish to PyPI
poetry publish

# Commit and push to GitHub
git add .
git commit -m "Release v0.1.1"
git tag v0.1.1
git push origin main --tags
```

---

## 🔍 Troubleshooting

### PyPI Publishing Issues

**Error: Package name already exists**
- Solution: Use a unique name with your username prefix

**Error: Invalid credentials**
- Solution: Regenerate PyPI token and run:
  ```powershell
  poetry config pypi-token.pypi YOUR_NEW_TOKEN
  ```

### GitHub Pages Not Showing

**Check:**
1. Is the `gh-pages` branch created? (Check branches on GitHub)
2. Is GitHub Pages enabled in Settings → Pages?
3. Did the workflow succeed? (Check Actions tab)
4. Wait 2-5 minutes after first deployment

**Manual Deploy:**
```powershell
cd docs
.\make.bat html
poetry run ghp-import -n -p -f build/html
```

### GitHub Actions Failing

**Common Issues:**
1. **Working directory wrong** - Fixed with `working-directory: ./package_creation`
2. **Python version mismatch** - Using 3.12.4 consistently
3. **Missing dependencies** - All installed via Poetry

**View Logs:**
- GitHub repo → Actions tab → Click on failed workflow → View logs

---

## 📖 Additional Resources

- **PyPI Package Page:** https://pypi.org/project/ifatmagamha-package-creation/
- **GitHub Pages Docs:** https://docs.github.com/en/pages
- **Poetry Publishing:** https://python-poetry.org/docs/cli/#publish
- **Sphinx Tutorial:** https://www.sphinx-doc.org/en/master/tutorial/

---

## 🎯 Next Steps

1. ✅ **Publish to PyPI** - Run `poetry publish`
2. ✅ **Push to GitHub** - Trigger CI/CD workflows
3. ✅ **Enable GitHub Pages** - Make docs publicly accessible
4. 📝 **Write better README** - Add usage examples
5. 🏷️ **Add badges** - Show build status and coverage
6. 📦 **Add more features** - Expand your package

---

**Your package is ready to publish!** 🎉
