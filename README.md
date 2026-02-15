# Package Creation Tutorial

A comprehensive guide to creating, testing, documenting, and publishing a Python package using modern tools and best practices.

## Project Overview

This project demonstrates the complete workflow of creating a Python package from scratch, including:
- Package structure and organization
- Dependency management with Poetry
- Unit testing
- Code quality with pre-commit hooks
- Documentation generation with Sphinx
- Continuous Integration with GitHub Actions
- Publishing to PyPI

**Published Package:** `ifatmagamha-package-creation`  
**PyPI URL:** https://pypi.org/project/ifatmagamha-package-creation/  
**Installation:** `pip install ifatmagamha-package-creation`

---

## Package Features

The package provides three string manipulation functions:

- **reverse_string**: Reverse any string
- **count_vowels**: Count vowels in a string
- **capitalize_words**: Capitalize the first letter of each word

### Usage Example

```python
from package_creation.string_ops import reverse_string, count_vowels, capitalize_words

# Reverse a string
result = reverse_string("hello world")
print(result)  # Output: dlrow olleh

# Count vowels
count = count_vowels("hello world")
print(count)  # Output: 3

# Capitalize words
capitalized = capitalize_words("hello world")
print(capitalized)  # Output: Hello World
```

---

## Project Structure

```
package_creation/
├── .pre-commit-config.yaml      # Pre-commit hooks configuration
├── pyproject.toml               # Poetry configuration and dependencies
├── README.md                    # This file
├── src/
│   └── package_creation/
│       ├── __init__.py
│       ├── main.py              # Main entry point
│       └── string_ops.py        # String operations module
├── tests/
│   └── test_string_ops.py       # Unit tests
├── docs/
│   ├── source/
│   │   ├── conf.py              # Sphinx configuration
│   │   ├── index.rst            # Documentation index
│   │   └── module.rst           # Module documentation
│   ├── build/html/              # Generated HTML documentation
│   └── make.bat                 # Windows build script
└── dist/                        # Built packages (.whl, .tar.gz)
```

---

## Development Setup

### Prerequisites

- Python 3.8 or higher
- Poetry (for dependency management)
- Git

### Installation Steps

```powershell
# Clone the repository
git clone https://github.com/ifatmagamha/py_project.git
cd py_project/package_creation

# Install Poetry (if not already installed)
pip install poetry

# Install dependencies
poetry install

# Install pre-commit hooks
poetry run pre-commit install
```

---

## Key Commands Reference

### Poetry Commands

```powershell
# Install dependencies
poetry install

# Add a new dependency
poetry add <package-name>

# Add a development dependency
poetry add --group dev <package-name>

# Run Python scripts
poetry run python -m package_creation.main

# Run tests
poetry run python -m unittest tests.test_string_ops -v

# Build the package
poetry build

# Publish to PyPI
poetry publish
```

### Testing

```powershell
# Run all tests
poetry run python -m unittest discover tests -v

# Run specific test file
poetry run python -m unittest tests.test_string_ops -v
```

### Pre-commit Hooks

```powershell
# Install hooks
poetry run pre-commit install

# Run on all files
poetry run pre-commit run --all-files

# Run on staged files only
poetry run pre-commit run
```

### Documentation

```powershell
# Navigate to docs directory
cd docs

# Build HTML documentation (Windows)
.\make.bat html

# Open documentation
start build\html\index.html

# Clean build files
.\make.bat clean
```

---

## Challenges and Solutions

### Challenge 1: Virtual Environment Path Mismatch

**Problem:** Error when running pip commands due to conflicting virtual environment paths.

**Error Message:**
```
Unable to create process using '"C:\Users\islem\Downloads\data viz\venv\Scripts\python.exe"  
"C:\Users\islem\Downloads\data viz\py_project\venv\Scripts\pip.exe"'
```

**Root Cause:** Virtual environment was moved or copied, causing path references to become invalid.

**Solution:**
1. Deactivate the current virtual environment
2. Delete the corrupted venv directory
3. Create a fresh virtual environment: `python -m venv venv`
4. Activate the new environment: `.\venv\Scripts\Activate.ps1`
5. Install dependencies: `pip install poetry`

**Key Takeaway:** Virtual environments contain absolute paths and should not be moved or copied. Always recreate them in the target location.

---

### Challenge 2: Poetry Shell Command Not Available

**Problem:** Poetry 2.0+ removed the `poetry shell` command.

**Error Message:**
```
Looks like you're trying to use a Poetry command that is not available.
Since Poetry (2.0.0), the shell command is not installed by default.
```

**Solution:**
Use `poetry run <command>` instead of activating a shell:
```powershell
# Instead of: poetry shell
# Use: poetry run python script.py
poetry run python -m package_creation.main
```

**Alternative:** Use `poetry env activate` or install the shell plugin.

**Key Takeaway:** Poetry 2.0 changed how environment activation works. The `poetry run` prefix is now the recommended approach.

---

### Challenge 3: Missing pyproject.toml

**Problem:** Poetry commands failed because pyproject.toml was in a subdirectory.

**Error Message:**
```
Poetry could not find a pyproject.toml file in C:\Users\islem\Downloads\data viz\py_project or its parents
```

**Root Cause:** Running Poetry commands from the wrong directory.

**Solution:**
Always run Poetry commands from the directory containing pyproject.toml:
```powershell
cd "C:\Users\islem\Downloads\data viz\py_project\package_creation"
poetry build
```

**Key Takeaway:** Poetry requires pyproject.toml in the current directory. Always verify your working directory before running Poetry commands.

---

### Challenge 4: Import Errors in Code

**Problem:** Code imported from wrong package name.

**Error:** Import statements referenced `package_creation_tutorial` instead of `package_creation`.

**Solution:**
Fixed import statements in main.py and test files:
```python
# Wrong
from package_creation_tutorial.string_ops import reverse_string

# Correct
from package_creation.string_ops import reverse_string
```

**Key Takeaway:** Package names in imports must match the package name defined in pyproject.toml.

---

### Challenge 5: Missing self Parameter in Test Methods

**Problem:** Unit test methods were missing the `self` parameter.

**Error:** Tests failed because unittest.TestCase methods require `self` as the first parameter.

**Solution:**
```python
# Wrong
def test_reverse_string():
    self.assertEqual(reverse_string("hello"), "olleh")

# Correct
def test_reverse_string(self):
    self.assertEqual(reverse_string("hello"), "olleh")
```

**Key Takeaway:** All methods in a unittest.TestCase class must have `self` as the first parameter.

---

### Challenge 6: Pre-commit Config in Wrong Location

**Problem:** Pre-commit couldn't find configuration file.

**Error Message:**
```
.pre-commit-config.yaml is not a file
```

**Root Cause:** Configuration file was in `tests/` directory instead of project root.

**Solution:**
Move `.pre-commit-config.yaml` to the project root (where `.git` directory is located).

**Key Takeaway:** Pre-commit looks for its configuration in the repository root, not subdirectories.

---

### Challenge 7: Make Command Not Found on Windows

**Problem:** `make` command doesn't exist on Windows by default.

**Error Message:**
```
make : Le terme «make» n'est pas reconnu...
```

**Solution:**
Use the Windows batch file instead:
```powershell
# Instead of: make html
# Use:
.\make.bat html
```

**Key Takeaway:** Sphinx provides `make.bat` for Windows users. Always use the `.bat` version on Windows.

---

### Challenge 8: Wrong Package Names in Documentation

**Problem:** Sphinx documentation referenced incorrect package name.

**Root Cause:** Documentation files used `package_creation_tutorial` instead of `package_creation`.

**Solution:**
Updated all references in:
- `docs/source/index.rst`
- `docs/source/module.rst`
- `docs/source/conf.py`

Also added required configuration to conf.py:
```python
import os
import sys
sys.path.insert(0, os.path.abspath('../../src'))

extensions = [
    'sphinx.ext.autodoc',
    'sphinx.ext.napoleon',
    'sphinx.ext.viewcode',
]

html_theme = 'sphinx_rtd_theme'
```

**Key Takeaway:** Sphinx needs the correct Python path and extensions configured to generate API documentation.

---

### Challenge 9: PyPI Package Name Conflict

**Problem:** Cannot publish to PyPI due to existing package with same name.

**Error Message:**
```
HTTP Error 403: Forbidden | The user 'ifg' isn't allowed to upload to project 'Package_Creation'
```

**Root Cause:** A package named `package-creation` already exists on PyPI.

**Solution:**
Changed package name to include username prefix:
```toml
# pyproject.toml
name = "ifatmagamha-package-creation"
```

**Key Takeaway:** PyPI package names must be globally unique. Use a prefix (username, organization) to ensure uniqueness.

---

### Challenge 10: PyPI README Rendering Error

**Problem:** PyPI rejected package upload due to invalid README.

**Error Message:**
```
HTTP Error 400: Bad Request | The description failed to render for 'text/markdown'
```

**Root Cause:** README.md was empty.

**Solution:**
Created a comprehensive README.md with:
- Package description
- Installation instructions
- Usage examples
- Development guide

Also fixed pyproject.toml:
```toml
description = "A simple Python package for string manipulation operations"
requires-python = ">=3.8"  # Changed from >=3.14
```

**Key Takeaway:** PyPI requires a valid README and realistic Python version requirements. Always test your README renders correctly.

---

### Challenge 11: GitHub Actions Workflow Path Issues

**Problem:** GitHub Actions workflows failed because they couldn't find the package.

**Root Cause:** Workflows were running from repository root, but package is in `package_creation/` subdirectory.

**Solution:**
Added `working-directory` to workflow configuration:
```yaml
jobs:
    test:
        runs-on: ubuntu-latest
        defaults:
            run:
                working-directory: ./package_creation
```

Also updated to use Poetry:
```yaml
- name: Install Poetry
  run: |
    python -m pip install --upgrade pip
    pip install poetry
- name: Install dependencies
  run: |
    poetry install
- name: Run tests
  run: |
    poetry run python -m unittest discover tests -v
```

**Key Takeaway:** GitHub Actions workflows need explicit working directory configuration for monorepo or nested project structures.

---

## Configuration Files Explained

### pyproject.toml

The central configuration file for Poetry-based projects:

```toml
[project]
name = "ifatmagamha-package-creation"
version = "0.1.0"
description = "A simple Python package for string manipulation operations"
authors = [{name = "ifatmagamha", email = "gamhaif@gmail.com"}]
readme = "README.md"
requires-python = ">=3.8"
dependencies = [
    "pytest (>=9.0.2,<10.0.0)",
    "sphinx (>=9.1.0,<10.0.0)",
    "sphinx-rtd-theme (>=3.1.0,<4.0.0)"
]

[tool.poetry]
packages = [{include = "package_creation", from = "src"}]

[build-system]
requires = ["poetry-core>=2.0.0,<3.0.0"]
build-backend = "poetry.core.masonry.api"
```

**Key Fields:**
- `name`: Package name on PyPI (must be unique)
- `version`: Semantic version number (MAJOR.MINOR.PATCH)
- `requires-python`: Minimum Python version
- `packages`: Where to find the package source code
- `build-backend`: Build system (Poetry uses poetry-core)

---

### .pre-commit-config.yaml

Configuration for pre-commit hooks:

```yaml
repos:
- repo: https://github.com/astral-sh/ruff-pre-commit
  rev: v0.8.4
  hooks:
  - id: ruff
    args: [--fix, --exit-non-zero-on-fix]
```

**What it does:**
- Runs Ruff linter before each commit
- Automatically fixes code style issues
- Prevents commits if issues can't be auto-fixed

**Usage:**
- Install: `poetry run pre-commit install`
- Run manually: `poetry run pre-commit run --all-files`

---

### GitHub Actions Workflows

#### tests.yaml - Unit Tests

```yaml
name: unit-tests
on:
    push:   
        branches: [ master, main ]
    pull_request:
        branches: [ master, main ]
jobs:
    test:
        runs-on: ubuntu-latest
        defaults:
            run:
                working-directory: ./package_creation
        steps:
        - name: Checkout code
          uses: actions/checkout@v4
        - name: Set up Python
          uses: actions/setup-python@v5
          with:
            python-version: '3.12.4'
        - name: Install Poetry
          run: |
            python -m pip install --upgrade pip
            pip install poetry
        - name: Install dependencies
          run: |
            poetry install
        - name: Run tests
          run: |
            poetry run python -m unittest discover tests -v
```

**Triggers:** Every push or pull request to master/main branches  
**Purpose:** Ensures all tests pass before merging code

---

#### sphinx_docs.yaml - Documentation

```yaml
name: documentation
on:
    push:
        branches: [ master, main ]
    pull_request:
        branches: [ master, main ]
jobs:
    build:
        runs-on: ubuntu-latest
        defaults:
            run:
                working-directory: ./package_creation
        steps:
            - uses: actions/checkout@v4
            - name: Set up Python 3.12.4
              uses: actions/setup-python@v5
              with:
                python-version: "3.12.4"
            - name: Install dependencies
              run: |
                python -m pip install --upgrade pip
                pip install ghp-import sphinx==9.1.0 sphinx_rtd_theme==3.1.0
            - name: Build HTML
              run: |
                cd docs/
                make html
            - name: Deploy to GitHub Pages
              if: github.event_name == 'push' && github.ref == 'refs/heads/main'
              run: |
                ghp-import -n -p -f docs/build/html
              env:
                GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

**Triggers:** Every push or pull request to master/main branches  
**Purpose:** Builds documentation and deploys to GitHub Pages

---

## Publishing Workflow

### Step 1: Prepare Package

```powershell
# Ensure all tests pass
poetry run python -m unittest discover tests -v

# Run pre-commit checks
poetry run pre-commit run --all-files

# Update version in pyproject.toml if needed
# version = "0.1.1"
```

### Step 2: Build Package

```powershell
# Clean previous builds
Remove-Item -Recurse -Force dist

# Build new distribution
poetry build
```

This creates:
- `dist/ifatmagamha_package_creation-0.1.0.tar.gz` (source distribution)
- `dist/ifatmagamha_package_creation-0.1.0-py3-none-any.whl` (wheel)

### Step 3: Configure PyPI Token

```powershell
# One-time setup
poetry config pypi-token.pypi YOUR_PYPI_TOKEN
```

Get your token from: https://pypi.org/manage/account/token/

### Step 4: Publish to PyPI

```powershell
poetry publish
```

### Step 5: Verify Publication

```powershell
# Install from PyPI
pip install ifatmagamha-package-creation

# Test it
python -c "from package_creation.string_ops import reverse_string; print(reverse_string('Success!'))"
```

### Step 6: Push to GitHub

```powershell
git add .
git commit -m "Published to PyPI v0.1.0"
git tag v0.1.0
git push origin main --tags
```

---

## GitHub Pages Setup

### Enable GitHub Pages

1. Push your code to GitHub
2. Go to repository Settings
3. Navigate to Pages section (left sidebar)
4. Under "Branch", select: `gh-pages` → `/ (root)` → Save
5. Wait 2-5 minutes for deployment

### Access Documentation

Your documentation will be available at:
```
https://ifatmagamha.github.io/py_project/
```

The GitHub Actions workflow automatically builds and deploys documentation on every push to main.

---

## Semantic Versioning

Version format: MAJOR.MINOR.PATCH

- **PATCH (0.1.0 → 0.1.1)**: Bug fixes, no API changes
- **MINOR (0.1.0 → 0.2.0)**: New features, backward compatible
- **MAJOR (0.1.0 → 1.0.0)**: Breaking changes, not backward compatible

### Updating Version

1. Edit `pyproject.toml`:
   ```toml
   version = "0.1.1"
   ```

2. Rebuild and publish:
   ```powershell
   poetry build
   poetry publish
   ```

3. Tag the release:
   ```powershell
   git tag v0.1.1
   git push origin main --tags
   ```

---

## Testing Strategy

### Unit Tests

Located in `tests/test_string_ops.py`:

```python
import unittest
from package_creation.string_ops import reverse_string, count_vowels, capitalize_words

class TestStringOps(unittest.TestCase):

    def test_reverse_string(self):
        """Test the reverse_string function."""
        self.assertEqual(reverse_string("hello"), "olleh")
        self.assertEqual(reverse_string("python"), "nohtyp")

    def test_count_vowels(self):
        """Test the count_vowels function."""
        self.assertEqual(count_vowels("hello"), 2)
        self.assertEqual(count_vowels("AEIOU"), 5)

    def test_capitalize_words(self):
        """Test the capitalize_words function."""
        self.assertEqual(capitalize_words("hello world"), "Hello World")
        self.assertEqual(capitalize_words("python programming"), "Python Programming")
```

### Running Tests

```powershell
# Run all tests with verbose output
poetry run python -m unittest discover tests -v

# Run specific test file
poetry run python -m unittest tests.test_string_ops -v

# Run specific test class
poetry run python -m unittest tests.test_string_ops.TestStringOps

# Run specific test method
poetry run python -m unittest tests.test_string_ops.TestStringOps.test_reverse_string
```

---

## Documentation Generation

### Sphinx Configuration

The documentation uses:
- **Sphinx**: Documentation generator
- **ReadTheDocs Theme**: Professional theme
- **Autodoc**: Automatic API documentation from docstrings
- **Napoleon**: Support for Google/NumPy style docstrings

### Building Documentation

```powershell
cd docs
.\make.bat html
start build\html\index.html
```

### Documentation Structure

```
docs/
├── source/
│   ├── conf.py          # Sphinx configuration
│   ├── index.rst        # Main page
│   └── module.rst       # API reference
└── build/
    └── html/            # Generated HTML
        └── index.html   # Entry point
```

---

## Code Quality Tools

### Ruff

Fast Python linter and formatter configured in `.pre-commit-config.yaml`.

**Features:**
- Checks code style
- Finds common errors
- Auto-fixes issues when possible

**Manual run:**
```powershell
poetry run ruff check .
poetry run ruff check --fix .
```

### Pre-commit Hooks

Automatically runs Ruff before each commit.

**Setup:**
```powershell
poetry run pre-commit install
```

**Manual run:**
```powershell
poetry run pre-commit run --all-files
```

---

## Continuous Integration

### GitHub Actions

Two workflows run automatically:

1. **Unit Tests** (`tests.yaml`)
   - Runs on every push and pull request
   - Tests on Ubuntu with Python 3.12.4
   - Fails if any test fails

2. **Documentation** (`sphinx_docs.yaml`)
   - Builds documentation on every push
   - Deploys to GitHub Pages on push to main
   - Fails if documentation build fails

### Viewing CI Results

1. Go to your GitHub repository
2. Click the "Actions" tab
3. Select a workflow run to view logs

---

## Key Learnings

### Package Management
- Poetry provides unified dependency management, virtual environments, and packaging
- `pyproject.toml` is the single source of truth for project configuration
- Lock files ensure reproducible builds across environments

### Testing
- Unit tests should cover all public functions
- Test methods in unittest.TestCase must have `self` parameter
- Run tests before every commit and publish

### Documentation
- Sphinx generates professional documentation from docstrings
- ReadTheDocs theme provides clean, searchable interface
- GitHub Pages offers free hosting for project documentation

### Code Quality
- Pre-commit hooks catch issues before they enter version control
- Ruff provides fast linting and auto-fixing
- Consistent code style improves maintainability

### Publishing
- PyPI package names must be globally unique
- README.md must be valid and render correctly
- Semantic versioning communicates change impact to users

### CI/CD
- GitHub Actions automates testing and deployment
- Working directory configuration is critical for nested projects
- Automated documentation deployment keeps docs in sync with code

### Version Control
- Git tags mark release points
- Commit messages should be descriptive
- Keep build artifacts out of version control

---

## Common Pitfalls to Avoid

1. **Moving virtual environments** - Always recreate them in the target location
2. **Running Poetry from wrong directory** - Ensure you're in the directory with pyproject.toml
3. **Forgetting self parameter** - All unittest methods need self
4. **Empty README** - PyPI requires valid README for package description
5. **Unrealistic Python versions** - Use versions that actually exist (>=3.8, not >=3.14)
6. **Wrong package names** - Imports must match package name in pyproject.toml
7. **Pre-commit config location** - Must be in repository root, not subdirectory
8. **Using make on Windows** - Use make.bat instead
9. **Package name conflicts** - Check PyPI before choosing a name
10. **Missing working directory in CI** - Specify working-directory for nested projects

---

## Resources

- **Poetry Documentation**: https://python-poetry.org/docs/
- **PyPI**: https://pypi.org/
- **Sphinx Documentation**: https://www.sphinx-doc.org/
- **GitHub Actions**: https://docs.github.com/en/actions
- **Pre-commit**: https://pre-commit.com/
- **Ruff**: https://github.com/astral-sh/ruff
- **Semantic Versioning**: https://semver.org/

---

## License

MIT License

## Author

ifatmagamha (gamhaif@gmail.com)
