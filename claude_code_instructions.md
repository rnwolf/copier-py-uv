# Python Package Development Instructions for Claude Code

## Project Structure

Use this file layout for all Python packages developed with TDD and uv:

```
my-package/
├── pyproject.toml
├── README.md
├── CHANGELOG.md
├── LICENSE.md
├── .env
├── .gitignore
├── .pre-commit-config.yaml
├── duties.py
├── src/
│   └── my_package/
│       ├── __init__.py
│       ├── __main__.py
│       ├── cli.py
│       ├── core.py
│       ├── utils.py
│       └── submodule/
│           ├── __init__.py
│           └── feature.py
├── tests/
│   ├── __init__.py
│   ├── conftest.py
│   ├── test_version.py
│   ├── test_cli.py
│   ├── test_core.py
│   ├── test_utils.py
│   └── test_submodule/
│       ├── __init__.py
│       └── test_feature.py
├── docs/
│   ├── index.md
│   ├── api.md
│   ├── mkdocs.yml
│   └── gen_ref_pages.py
├── scripts/
│   ├── bump-version.py
│   └── gen_ref_pages.py
└── .github/
    └── workflows/
        └── ci.yml
```

## Core Configuration Files

### pyproject.toml
```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "my-package"
version = "0.1.0"
description = "A brief description of the package"
readme = "README.md"
license = {file = "LICENSE.md"}
requires-python = ">=3.13"
authors = [
    {name = "Your Name", email = "your.email@example.com"},
]
keywords = ["keyword1", "keyword2"]
classifiers = [
    "Development Status :: 3 - Alpha",
    "Intended Audience :: Developers",
    "License :: OSI Approved :: MIT License",
    "Programming Language :: Python :: 3",
    "Programming Language :: Python :: 3.13",
]
dependencies = [
    "click>=8.0",  # for CLI interface
    "loguru>=0.7.0",  # for logging
]

[project.optional-dependencies]
dev = [
    "pytest>=7.0",
    "pytest-cov>=4.0",
    "pytest-xdist>=3.0",  # parallel testing
    "ruff>=0.1.0",
    "pyright>=1.1.350",
    "pre-commit>=3.0",
    "build>=0.10.0",
    "click>=8.0",  # for testing CLI
    "duty>=1.4.0",  # task runner
]
docs = [
    "mkdocs>=1.5",
    "mkdocs-material>=9.0",
    "mkdocstrings[python]>=0.20",
    "mkdocs-gen-files>=0.5",
    "mkdocs-literate-nav>=0.6",
]

[project.urls]
Homepage = "https://github.com/username/my-package"
Repository = "https://github.com/username/my-package"
Issues = "https://github.com/username/my-package/issues"

[project.scripts]
my-package = "my_package.cli:main"

[[tool.uv.index]]
name = "testpypi"
url = "https://test.pypi.org/simple/"
publish-url = "https://test.pypi.org/legacy/"
explicit = true

[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = "test_*.py"
python_classes = "Test*"
python_functions = "test_*"
addopts = [
    "--cov=src/my_package",
    "--cov-report=term-missing",
    "--cov-report=html",
    "--cov-fail-under=80",
    "--strict-markers",
    "--disable-warnings",
]
markers = [
    "slow: marks tests as slow (deselect with '-m \"not slow\"')",
    "integration: marks tests as integration tests",
    "unit: marks tests as unit tests",
]

[tool.ruff]
target-version = "py313"
line-length = 88
select = [
    "E",  # pycodestyle errors
    "W",  # pycodestyle warnings
    "F",  # pyflakes
    "I",  # isort
    "B",  # flake8-bugbear
    "C4", # flake8-comprehensions
    "UP", # pyupgrade
]
ignore = [
    "E501",  # line too long, handled by ruff format
    "B008",  # do not perform function calls in argument defaults
]

[tool.ruff.format]
quote-style = "double"
indent-style = "space"
skip-magic-trailing-comma = false
line-ending = "auto"

[tool.ruff.per-file-ignores]
"tests/*" = ["S101"]  # allow asserts in tests

[tool.pyright]
pythonVersion = "3.13"
include = ["src"]
exclude = [
    "**/__pycache__",
    "**/.pytest_cache",
    "**/node_modules",
    "**/.venv",
    "**/.*",
]
defineConstant = { DEBUG = true }
stubPath = "src/stubs"
venvPath = "."
venv = ".venv"

# Type checking settings
typeCheckingMode = "strict"
reportMissingImports = true
reportMissingTypeStubs = false
reportImportCycles = true
reportUnusedImport = true
reportUnusedClass = true
reportUnusedFunction = true
reportUnusedVariable = true
reportDuplicateImport = true
reportOptionalSubscript = true
reportOptionalMemberAccess = true
reportOptionalCall = true
reportOptionalIterable = true
reportOptionalContextManager = true
reportOptionalOperand = true
reportTypedDictNotRequiredAccess = false
reportPrivateImportUsage = true
reportConstantRedefinition = true
reportIncompatibleMethodOverride = true
reportIncompatibleVariableOverride = true
reportInconsistentConstructor = true
reportOverlappingOverloads = true
reportMissingSuperCall = false
reportUninitializedInstanceVariable = true
reportInvalidStringEscapeSequence = true
reportUnknownParameterType = true
reportUnknownArgumentType = true
reportUnknownLambdaType = true
reportUnknownVariableType = true
reportUnknownMemberType = false
reportMissingParameterType = true
reportMissingTypeArgument = true
reportInvalidTypeVarUse = true
reportCallInDefaultInitializer = true
reportUnnecessaryIsInstance = true
reportUnnecessaryCast = true
reportUnnecessaryComparison = true
reportAssertAlwaysTrue = true
reportSelfClsParameterName = true
reportImplicitStringConcatenation = false
reportUndefinedVariable = true
reportUnboundVariable = true
```

### .pre-commit-config.yaml
```yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-added-large-files
      - id: check-merge-conflict
      - id: debug-statements

  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.1.0
    hooks:
      - id: ruff
        args: [--fix]
      - id: ruff-format

  - repo: https://github.com/RobertCraigie/pyright-python
    rev: v1.1.350
    hooks:
      - id: pyright
```

### docs/mkdocs.yml
```yaml
site_name: My Package
site_description: A brief description of the package
site_url: https://username.github.io/my-package
repo_url: https://github.com/username/my-package
repo_name: username/my-package

nav:
  - Home: index.md
  - API Reference: api.md

theme:
  name: material
  palette:
    # Palette toggle for light mode
    - scheme: default
      primary: blue
      accent: blue
      toggle:
        icon: material/brightness-7
        name: Switch to dark mode
    # Palette toggle for dark mode
    - scheme: slate
      primary: blue
      accent: blue
      toggle:
        icon: material/brightness-4
        name: Switch to light mode
  features:
    - navigation.tabs
    - navigation.sections
    - navigation.expand
    - navigation.indexes
    - navigation.top
    - search.highlight
    - search.share
    - content.code.copy
    - content.code.annotate

plugins:
  - search
  - mkdocstrings:
      handlers:
        python:
          options:
            docstring_style: google
            show_source: true
            show_root_heading: true
            show_root_toc_entry: false
            heading_level: 2
  - gen-files:
      scripts:
        - docs/gen_ref_pages.py
  - literate-nav:
      nav_file: SUMMARY.md

markdown_extensions:
  - admonition
  - pymdownx.details
  - pymdownx.superfences:
      custom_fences:
        - name: mermaid
          class: mermaid
          format: !!python/name:pymdownx.superfences.fence_code_format
  - pymdownx.highlight:
      anchor_linenums: true
      line_spans: __span
      pygments_lang_class: true
  - pymdownx.inlinehilite
  - pymdownx.snippets
  - pymdownx.tabbed:
      alternate_style: true
  - attr_list
  - md_in_html
  - toc:
      permalink: true

extra:
  social:
    - icon: fontawesome/brands/github
      link: https://github.com/username/my-package
    - icon: fontawesome/brands/python
      link: https://pypi.org/project/my-package/
```

### docs/index.md
```markdown
# My Package

A brief description of what your package does and why it's useful.

## Installation

Install from PyPI:

```bash
pip install my-package
```

Or with uv:

```bash
uv add my-package
```

## Quick Start

```python
from my_package import main_function

# Basic usage example
result = main_function("example input")
print(result)
```

## CLI Usage

The package also provides a command-line interface:

```bash
# Show version
my-package --version

# Basic command
my-package hello

# With options
my-package hello --verbose
```

## Features

- Feature 1: Description of what it does
- Feature 2: Another key capability
- Feature 3: Additional functionality

## Requirements

- Python 3.13+
- Dependencies are automatically handled during installation

## License

This project is licensed under the MIT License - see the [LICENSE.md](https://github.com/username/my-package/blob/main/LICENSE.md) file for details.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request
```

### docs/api.md
```markdown
# API Reference

::: my_package

## Core Functions

::: my_package.core

## Utilities

::: my_package.utils

## CLI

::: my_package.cli
```

### duties.py
```python
"""Development tasks for my-package."""

import os
import re
import sys
import tempfile
from datetime import datetime
from pathlib import Path

from duty import duty


@duty
def setup_dev(ctx):
    """Set up the development environment."""
    ctx.run(["uv", "sync", "--all-extras"], title="Installing all dependencies")
    ctx.run(["uv", "run", "pre-commit", "install"], title="Installing pre-commit hooks")
    print("✅ Development environment set up successfully!")
    print("💡 Don't forget to:")
    print("   - Add your PyPI tokens to .env file")
    print("   - Commit your initial code: git add . && git commit -m 'Initial commit'")


@duty(aliases=["test", "t"])
def run_tests(ctx, coverage: bool = True, parallel: bool = True, verbose: bool = False):
    """Run the test suite with optional coverage and parallel execution."""
    cmd = ["uv", "run", "pytest"]

    if coverage:
        cmd.extend(["--cov=src/my_package", "--cov-report=term-missing", "--cov-report=html"])

    if parallel:
        cmd.extend(["-n", "auto"])

    if verbose:
        cmd.append("-v")

    ctx.run(cmd, title="Running tests")


@duty(aliases=["lint"])
def check_quality(ctx, fix: bool = False):
    """Check code quality with ruff and pyright."""
    # Format code
    ctx.run(["uv", "run", "ruff", "format", "."], title="Formatting code with ruff")

    # Check linting
    check_cmd = ["uv", "run", "ruff", "check", "."]
    if fix:
        check_cmd.append("--fix")
    ctx.run(check_cmd, title="Checking code quality with ruff")

    # Type checking
    ctx.run(["uv", "run", "pyright"], title="Type checking with pyright")


@duty
def check_all(ctx):
    """Run all quality checks and tests."""
    check_quality.run(ctx)
    run_tests.run(ctx)


@duty(aliases=["clean"])
def clean_artifacts(ctx):
    """Clean up build artifacts and temporary files."""
    patterns = [
        "build/", "dist/", "*.egg-info/", ".coverage*", ".pytest_cache/",
        ".mypy_cache/", ".ruff_cache/", "htmlcov/", "__pycache__/", "*.pyc", "*.pyo"
    ]

    for pattern in patterns:
        if pattern.endswith("/"):
            # Directory patterns
            ctx.run(f"rm -rf {pattern}", title=f"Removing {pattern}", nofail=True, silent=True)
        else:
            # File patterns
            ctx.run(f"find . -name '{pattern}' -delete", title=f"Removing {pattern}", nofail=True, silent=True)


@duty(aliases=["docs"])
def serve_docs(ctx, port: int = 8000):
    """Serve documentation locally with live reload."""
    ctx.run(
        ["uv", "run", "--group", "docs", "mkdocs", "serve", "--dev-addr", f"localhost:{port}"],
        title=f"Serving docs on http://localhost:{port}",
        capture=False
    )


@duty
def build_docs(ctx):
    """Build documentation for production."""
    ctx.run(
        ["uv", "run", "--group", "docs", "mkdocs", "build"],
        title="Building documentation"
    )


@duty
def deploy_docs(ctx):
    """Deploy documentation to GitHub Pages."""
    ctx.run(
        ["uv", "run", "--group", "docs", "mkdocs", "gh-deploy"],
        title="Deploying documentation to GitHub Pages"
    )


@duty
def bump_version(ctx, version_type: str):
    """
    Bump version number.

    Args:
        version_type: Type of version bump (major, minor, patch)
    """
    if version_type not in ["major", "minor", "patch"]:
        print("❌ Error: version_type must be one of: major, minor, patch")
        sys.exit(1)

    ctx.run(
        ["python", "scripts/bump-version.py", version_type],
        title=f"Bumping {version_type} version"
    )


@duty(pre=["check_all"])
def build_package(ctx):
    """Build the package for distribution."""
    clean_artifacts.run(ctx)
    ctx.run(["uv", "run", "python", "-m", "build"], title="Building package")


def _get_current_version():
    """Get current version from pyproject.toml."""
    pyproject_path = Path("pyproject.toml")
    if not pyproject_path.exists():
        raise FileNotFoundError("pyproject.toml not found")

    content = pyproject_path.read_text()
    match = re.search(r'version = "([^"]+)"', content)
    if not match:
        raise ValueError("Version not found in pyproject.toml")

    return match.group(1)


def _update_changelog(version: str, summary: str):
    """Update CHANGELOG.md with new version entry."""
    if not Path("CHANGELOG.md").exists():
        raise FileNotFoundError("CHANGELOG.md not found")

    date = datetime.now().strftime("%Y-%m-%d")

    # Read current changelog
    changelog_content = Path("CHANGELOG.md").read_text()

    # Insert new version section after [Unreleased]
    new_section = f"""## [Unreleased]

### Added

### Changed

### Deprecated

### Removed

### Fixed

### Security

## [{version}] - {date}

### Added
- {summary}

"""

    # Replace the Unreleased section
    updated_content = re.sub(
        r"## \[Unreleased\].*?(?=## \[|\Z)",
        new_section,
        changelog_content,
        flags=re.DOTALL
    )

    Path("CHANGELOG.md").write_text(updated_content)
    print(f"✅ Updated CHANGELOG.md with version {version}")


@duty(pre=["build_package"])
def publish_test(ctx):
    """Publish package to Test PyPI."""
    current_version = _get_current_version()
    print(f"📦 Current version: {current_version}")

    # Prompt for changelog entry
    summary = input("📝 Enter a brief summary of changes: ").strip()
    if not summary:
        print("❌ Change summary is required")
        sys.exit(1)

    # Update changelog
    _update_changelog(current_version, summary)

    # Check if we're in a clean git state
    git_status = ctx.run(["git", "status", "--porcelain"], title="Checking git status")
    if git_status.strip():
        print("⚠️  You have uncommitted changes. Please commit them first.")
        sys.exit(1)

    # Build documentation
    try:
        build_docs.run(ctx)
    except Exception:
        print("⚠️  Documentation build failed, continuing anyway")

    # Publish to Test PyPI
    ctx.run(["uv", "publish", "--index", "testpypi"], title="Publishing to Test PyPI")

    # Commit changelog update
    ctx.run(["git", "add", "CHANGELOG.md"], title="Staging CHANGELOG.md")
    ctx.run(["git", "commit", "-m", f"Update CHANGELOG.md for v{current_version}"], title="Committing changelog")

    # Create and push git tag
    ctx.run(["git", "tag", f"v{current_version}"], title=f"Creating tag v{current_version}")
    ctx.run(["git", "push", "origin", f"v{current_version}"], title="Pushing tag")
    ctx.run(["git", "push"], title="Pushing commits")

    print("🎉 Successfully published to Test PyPI!")
    print(f"🧪 Test installation: pip install --index-url https://test.pypi.org/simple/ my-package")


@duty(pre=["build_package"])
def publish_prod(ctx):
    """Publish package to production PyPI."""
    current_version = _get_current_version()
    print(f"📦 Current version: {current_version}")

    # Check if we're on main/master branch
    current_branch = ctx.run(["git", "branch", "--show-current"]).strip()
    if current_branch not in ["main", "master"]:
        print(f"⚠️  Not on main/master branch. Current branch: {current_branch}")
        confirm = input("Continue anyway? (y/N): ").lower()
        if confirm != "y":
            sys.exit(1)

    # Prompt for changelog entry
    summary = input("📝 Enter a brief summary of changes: ").strip()
    if not summary:
        print("❌ Change summary is required")
        sys.exit(1)

    # Update changelog
    _update_changelog(current_version, summary)

    # Check if we're in a clean git state
    git_status = ctx.run(["git", "status", "--porcelain"], title="Checking git status")
    if git_status.strip():
        print("⚠️  You have uncommitted changes. Please commit them first.")
        sys.exit(1)

    # Check if tag already exists
    try:
        ctx.run(["git", "tag", "-l", f"v{current_version}"], title="Checking for existing tag")
        existing_tags = ctx.run(["git", "tag", "-l"], silent=True)
        if f"v{current_version}" in existing_tags:
            print(f"❌ Tag v{current_version} already exists")
            sys.exit(1)
    except Exception:
        pass

    # Build documentation
    try:
        build_docs.run(ctx)
    except Exception:
        print("⚠️  Documentation build failed, continuing anyway")

    # Final confirmation
    print(f"🚨 Ready to publish v{current_version} to production PyPI")
    confirm = input("Continue with publication? (y/N): ").lower()
    if confirm != "y":
        print("📦 Publication cancelled")
        sys.exit(0)

    # Publish to PyPI
    ctx.run(["uv", "publish"], title="Publishing to PyPI")

    # Commit changelog update
    ctx.run(["git", "add", "CHANGELOG.md"], title="Staging CHANGELOG.md")
    ctx.run(["git", "commit", "-m", f"Update CHANGELOG.md for v{current_version}"], title="Committing changelog")

    # Create and push git tag
    ctx.run(["git", "tag", f"v{current_version}"], title=f"Creating tag v{current_version}")
    ctx.run(["git", "push", "origin", f"v{current_version}"], title="Pushing tag")
    ctx.run(["git", "push"], title="Pushing commits")

    print("🎉 Successfully published to PyPI!")
    print(f"📦 Installation: pip install my-package")


@duty
def debug_fibonacci(ctx, n: int = 10):
    """Debug the fibonacci function with breakpoints."""
    debug_script = f"""
import pdb
from my_package.core import calculate_fibonacci
from my_package import configure_logging

# Enable debug logging
configure_logging(level="TRACE")

def debug_fibonacci():
    # Set breakpoint before function call
    pdb.set_trace()

    # Test with the provided value
    print(f"Calculating fibonacci({n})")
    result = calculate_fibonacci({n})
    print(f"fibonacci({n}) = {{result}}")

    # Another breakpoint to inspect results
    pdb.set_trace()

if __name__ == "__main__":
    debug_fibonacci()
"""

    # Write debug script to temporary file
    with tempfile.NamedTemporaryFile(mode='w', suffix='.py', delete=False) as f:
        f.write(debug_script)
        temp_file = f.name

    try:
        ctx.run(["uv", "run", "python", temp_file], title=f"Debugging fibonacci({n})")
    finally:
        os.unlink(temp_file)


@duty(aliases=["ci"])
def continuous_integration(ctx):
    """Run the full CI pipeline locally."""
    print("🚀 Running full CI pipeline...")
    check_quality.run(ctx)
    run_tests.run(ctx, coverage=True, parallel=True)
    build_docs.run(ctx)
    build_package.run(ctx)
    print("✅ CI pipeline completed successfully!")


@duty
def pre_commit_run(ctx, all_files: bool = False):
    """Run pre-commit hooks."""
    cmd = ["uv", "run", "pre-commit", "run"]
    if all_files:
        cmd.append("--all-files")
    ctx.run(cmd, title="Running pre-commit hooks")
```

### scripts/gen_ref_pages.py
```python
"""Generate the code reference pages and navigation."""

from pathlib import Path

import mkdocs_gen_files

nav = mkdocs_gen_files.Nav()

for path in sorted(Path("src").rglob("*.py")):
    module_path = path.relative_to("src").with_suffix("")
    doc_path = path.relative_to("src").with_suffix(".md")
    full_doc_path = Path("reference", doc_path)

    parts = tuple(module_path.parts)

    if parts[-1] == "__init__":
        parts = parts[:-1]
        doc_path = doc_path.with_name("index.md")
        full_doc_path = full_doc_path.with_name("index.md")
    elif parts[-1] == "__main__":
        continue

    nav[parts] = doc_path.as_posix()

    with mkdocs_gen_files.open(full_doc_path, "w") as fd:
        ident = ".".join(parts)
        fd.write(f"::: {ident}")

    mkdocs_gen_files.set_edit_path(full_doc_path, path)

with mkdocs_gen_files.open("reference/SUMMARY.md", "w") as nav_file:
    nav_file.writelines(nav.build_literate_nav())
```

### CHANGELOG.md
```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Initial project setup with TDD workflow
- CLI interface with version support
- Core functionality and utilities
- Comprehensive test suite
- Documentation with MkDocs
- Pre-commit hooks for code quality
- Automated publishing workflow

### Changed

### Deprecated

### Removed

### Fixed

### Security

## [0.1.0] - 2025-07-02

### Added
- Initial release
- Basic package structure
- Example CLI commands
- Development workflow setup
```

### LICENSE.md
```markdown
# MIT License

Copyright (c) 2025 Your Name

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

### .env
```bash
# PyPI Publishing Tokens
# Get tokens from:
# - PyPI: https://pypi.org/manage/account/#api-tokens
# - Test PyPI: https://test.pypi.org/manage/account/#api-tokens

UV_PUBLISH_TOKEN=pypi-your-api-token-here
UV_PUBLISH_TOKEN_TESTPYPI=pypi-your-test-api-token-here
```

### .gitignore
```
# Byte-compiled / optimized / DLL files
__pycache__/
*.py[cod]
*$py.class

# C extensions
*.so

# Distribution / packaging
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
*.egg-info/
.installed.cfg
*.egg
MANIFEST

# PyInstaller
*.manifest
*.spec

# Installer logs
pip-log.txt
pip-delete-this-directory.txt

# Unit test / coverage reports
htmlcov/
.tox/
.nox/
.coverage
.coverage.*
.cache
nosetests.xml
coverage.xml
*.cover
.hypothesis/
.pytest_cache/

# Environments
.env
.venv
env/
venv/
ENV/
env.bak/
venv.bak/

# mypy
.mypy_cache/
.dmypy.json
dmypy.json

# IDEs
.vscode/
.idea/
*.swp
*.swo
*~

# OS
.DS_Store
Thumbs.db
```

### .github/workflows/ci.yml
```yaml
name: CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ["3.13"]

    steps:
    - uses: actions/checkout@v4
    - name: Install uv
      uses: astral-sh/setup-uv@v2
    - name: Set up Python ${{ matrix.python-version }}
      run: uv python install ${{ matrix.python-version }}
    - name: Install dependencies
      run: uv sync --all-extras
    - name: Run tests
      run: uv run pytest
    - name: Run linting
      run: |
        uv run ruff check .
        uv run ruff format --check .
        uv run pyright
```

## Development Best Practices

### TDD Workflow
1. **Red**: Write a failing test first
2. **Green**: Write minimal code to make it pass
3. **Refactor**: Improve code while keeping tests green

### Test Organization
- Mirror source structure in tests
- Use descriptive test names: `test_should_return_empty_list_when_no_items_found`
- Group related tests in classes
- Use fixtures for common test data
- Mark tests appropriately (`@pytest.mark.slow`, `@pytest.mark.integration`)

### Code Quality Standards
- **Type hints**: Always include type hints for function signatures
- **Docstrings**: Use Google or NumPy style docstrings
- **Error handling**: Include proper exception handling with specific exception types
- **Logging**: Use structured logging with appropriate levels
- **Configuration**: Use environment variables or config files, not hardcoded values

### Example Test Structure (tests/conftest.py)
```python
import pytest
from unittest.mock import Mock
from my_package.core import SomeClass

@pytest.fixture
def sample_data():
    """Provide sample data for tests."""
    return {"key": "value", "number": 42}

@pytest.fixture
def mock_external_service():
    """Mock external service dependencies."""
    return Mock()

@pytest.fixture
def configured_instance(sample_data):
    """Provide a configured instance for testing."""
    return SomeClass(config=sample_data)
```

### Example Source Structure (src/my_package/__init__.py)
```python
"""My Package - A brief description."""

import importlib.metadata

try:
    __version__ = importlib.metadata.version("my-package")
except importlib.metadata.PackageNotFoundError:
    # Package is not installed, fallback to development version
    __version__ = "0.0.0-dev"

__author__ = "Your Name"
__email__ = "your.email@example.com"

from .core import main_function
from .utils import helper_function

__all__ = ["main_function", "helper_function", "__version__"]
```

### Example CLI Module (src/my_package/cli.py)
```python
"""Command line interface for my-package."""

import click
from . import __version__


@click.group()
@click.version_option(version=__version__, prog_name="my-package")
@click.pass_context
def cli(ctx: click.Context) -> None:
    """My Package CLI - A brief description of what it does."""
    ctx.ensure_object(dict)


@cli.command()
@click.option("--verbose", "-v", is_flag=True, help="Enable verbose output")
def hello(verbose: bool) -> None:
    """Say hello."""
    if verbose:
        click.echo(f"Hello from my-package v{__version__}!")
    else:
        click.echo("Hello!")


@cli.command()
def info() -> None:
    """Show package information."""
    click.echo(f"my-package version: {__version__}")


def main() -> None:
    """Entry point for the CLI."""
    cli()


if __name__ == "__main__":
    main()
```

### Example Main Module (src/my_package/__main__.py)
```python
"""Allow package to be run as python -m my_package."""

from .cli import main

if __name__ == "__main__":
    main()
```

### Version Testing (tests/test_version.py)
```python
"""Test version consistency across the package."""

import importlib.metadata
import re
import tomllib
from pathlib import Path

import pytest

from my_package import __version__


def test_version_is_valid_semver():
    """Test that version follows semantic versioning."""
    # SemVer regex pattern
    semver_pattern = r"^(\d+)\.(\d+)\.(\d+)(?:-([0-9A-Za-z-]+(?:\.[0-9A-Za-z-]+)*))?(?:\+([0-9A-Za-z-]+(?:\.[0-9A-Za-z-]+)*))?$"

    # Allow development versions
    dev_pattern = r"^\d+\.\d+\.\d+-dev$"

    assert re.match(semver_pattern, __version__) or re.match(dev_pattern, __version__), \
        f"Version '{__version__}' does not follow semantic versioning"


def test_version_matches_pyproject_toml():
    """Test that __version__ matches version in pyproject.toml."""
    # Skip this test in development mode
    if __version__.endswith("-dev"):
        pytest.skip("Skipping version check in development mode")

    # Find pyproject.toml (go up from tests directory)
    current_dir = Path(__file__).parent
    pyproject_path = None

    # Search up the directory tree for pyproject.toml
    for parent in [current_dir] + list(current_dir.parents):
        potential_path = parent / "pyproject.toml"
        if potential_path.exists():
            pyproject_path = potential_path
            break

    assert pyproject_path is not None, "Could not find pyproject.toml"

    # Read version from pyproject.toml
    with open(pyproject_path, "rb") as f:
        pyproject_data = tomllib.load(f)

    pyproject_version = pyproject_data["project"]["version"]

    assert __version__ == pyproject_version, \
        f"Version mismatch: __version__='{__version__}' vs pyproject.toml='{pyproject_version}'"


def test_version_accessible_via_importlib():
    """Test that version can be accessed via importlib.metadata."""
    try:
        installed_version = importlib.metadata.version("my-package")
        # Only check if package is actually installed (not in development mode)
        if not __version__.endswith("-dev"):
            assert __version__ == installed_version, \
                f"Version mismatch: __version__='{__version__}' vs installed='{installed_version}'"
    except importlib.metadata.PackageNotFoundError:
        # Package not installed, which is fine in development
        assert __version__.endswith("-dev"), \
            "Package not installed but version doesn't end with '-dev'"


def test_version_is_string():
    """Test that version is a string."""
    assert isinstance(__version__, str)
    assert len(__version__) > 0


def test_version_components_are_numeric():
    """Test that version components are numeric (except for pre-release/build)."""
    if __version__.endswith("-dev"):
        version_base = __version__.replace("-dev", "")
    else:
        # Remove pre-release and build metadata for testing
        version_base = __version__.split("-")[0].split("+")[0]

    parts = version_base.split(".")
    assert len(parts) == 3, f"Version should have 3 parts, got {len(parts)}: {parts}"

    for i, part in enumerate(parts):
        assert part.isdigit(), f"Version part {i} should be numeric: '{part}'"
        assert int(part) >= 0, f"Version part {i} should be non-negative: {part}"
```

### CLI Testing (tests/test_cli.py)
```python
"""Test the command line interface."""

import re

import pytest
from click.testing import CliRunner

from my_package import __version__
from my_package.cli import cli


@pytest.fixture
def runner():
    """Provide a CLI runner for testing."""
    return CliRunner()


def test_cli_version_option(runner):
    """Test that --version flag works and shows correct version."""
    result = runner.invoke(cli, ["--version"])
    assert result.exit_code == 0

    # Check that version is displayed
    version_pattern = rf"my-package, version {re.escape(__version__)}"
    assert re.search(version_pattern, result.output), \
        f"Expected version pattern '{version_pattern}' in output: {result.output}"


def test_cli_help(runner):
    """Test that help works."""
    result = runner.invoke(cli, ["--help"])
    assert result.exit_code == 0
    assert "My Package CLI" in result.output


def test_hello_command(runner):
    """Test the hello command."""
    result = runner.invoke(cli, ["hello"])
    assert result.exit_code == 0
    assert "Hello!" in result.output


def test_hello_command_verbose(runner):
    """Test the hello command with verbose flag."""
    result = runner.invoke(cli, ["hello", "--verbose"])
    assert result.exit_code == 0
    assert f"Hello from my-package v{__version__}!" in result.output


def test_info_command(runner):
    """Test the info command shows version."""
    result = runner.invoke(cli, ["info"])
    assert result.exit_code == 0
    assert f"my-package version: {__version__}" in result.output


def test_cli_can_be_imported():
    """Test that CLI module can be imported without errors."""
    from my_package.cli import main
    assert callable(main)


def test_main_module_exists():
    """Test that __main__.py exists and can be imported."""
    import my_package.__main__  # noqa: F401


def test_package_can_be_run_as_module(runner):
    """Test that package can be run with python -m my_package --version."""
    import subprocess
    import sys

    # Run the package as a module
    result = subprocess.run(
        [sys.executable, "-m", "my_package", "--version"],
        capture_output=True,
        text=True,
        timeout=10
    )

    assert result.returncode == 0, f"Command failed with stderr: {result.stderr}"

    # Check that version is displayed
    version_pattern = rf"my-package, version {re.escape(__version__)}"
    assert re.search(version_pattern, result.stdout), \
        f"Expected version pattern '{version_pattern}' in output: {result.stdout}"
```

## Setup Commands

### Initial Setup
```bash
# Navigate to your existing project directory
cd my-package

# Initialize git repository (if not already done)
git init

# Install development dependencies
uv add --dev pytest pytest-cov pytest-xdist ruff pyright pre-commit build click duty

# Install documentation dependencies
uv add --group docs mkdocs mkdocs-material mkdocstrings[python] mkdocs-gen-files mkdocs-literate-nav

# Install main dependencies
uv add click loguru

# Set up development environment using Duty
uv run duty setup-dev

# Create initial commit
git add .
git commit -m "Initial commit with development setup"
```

### Development Workflow
```bash
# TDD: Write a failing test first (Red)
uv run pytest tests/test_core.py::test_calculate_fibonacci -v

# Create the function to make test pass (Green)
# Example: Add to src/my_package/core.py
echo 'def calculate_fibonacci(n: int) -> int:
    """Calculate the nth Fibonacci number."""
    if n <= 1:
        return n
    return calculate_fibonacci(n - 1) + calculate_fibonacci(n - 2)' >> src/my_package/core.py

# Run the specific test to see it pass
uv run pytest tests/test_core.py::test_calculate_fibonacci -v

# Debug a specific function during development
uv run python -c "
import pdb
from my_package.core import calculate_fibonacci
pdb.set_trace()
result = calculate_fibonacci(10)
print(f'Result: {result}')
"

# Alternative: Create a debug script
cat > debug_fibonacci.py << 'EOF'
#!/usr/bin/env python3
"""Debug script for fibonacci function."""
import pdb
from my_package.core import calculate_fibonacci

def debug_fibonacci():
    """Debug the fibonacci function with breakpoints."""
    # Set breakpoint before function call
    pdb.set_trace()

    # Test with different values
    test_values = [5, 10, 15]

    for n in test_values:
        print(f"Calculating fibonacci({n})")
        result = calculate_fibonacci(n)
        print(f"fibonacci({n}) = {result}")
        # Another breakpoint to inspect results
        pdb.set_trace()

if __name__ == "__main__":
    debug_fibonacci()
EOF

# Run debug script
uv run python debug_fibonacci.py

# Run tests with different options
uv run pytest tests/test_core.py::test_calculate_fibonacci  # Single test
uv run pytest -x  # Stop on first failure
uv run pytest --lf  # Run last failed tests
uv run pytest -n auto  # Parallel execution
uv run pytest --pdb  # Drop into debugger on failures

# Test with coverage and see specific lines
uv run pytest tests/test_core.py --cov=src/my_package/core --cov-report=term-missing

# Build and serve documentation locally
uv run --group docs mkdocs serve

# Build documentation for production
uv run --group docs mkdocs build

# Interactive development and testing
uv run python -i -c "
from my_package.core import calculate_fibonacci
# Now you're in interactive mode - try:
# calculate_fibonacci(5)
# help(calculate_fibonacci)
"

# Code quality checks
uv run ruff format .
uv run ruff check . --fix
uv run pyright

# Run all checks
uv run pre-commit run --all-files

# Coverage report
uv run pytest --cov-report=html
open htmlcov/index.html

# Test CLI functionality
uv run my-package --version
uv run python -m my_package --version

# Version management using Duty
uv run duty bump-version patch          # or minor, major
python scripts/bump-version.py --dry-run minor  # preview changes (still using script)

# Publishing using Duty
uv run duty publish-test                # Test PyPI first
uv run duty publish-prod                # Production PyPI

# Documentation using Duty
uv run duty docs                        # Local development server
uv run duty build-docs                  # Build static site
uv run duty deploy-docs                 # Deploy to GitHub Pages

# Development workflow using Duty
uv run duty test                        # Run tests with coverage
uv run duty check-quality               # Format and lint code
uv run duty ci                          # Full CI pipeline locally
uv run duty clean                       # Clean artifacts
```

## Version Management and Publishing

### Version Numbering Strategy
Follow Semantic Versioning (SemVer) format: `MAJOR.MINOR.PATCH`
- **MAJOR**: Breaking changes that are not backward compatible
- **MINOR**: New features that are backward compatible
- **PATCH**: Bug fixes that are backward compatible

### Version Sources
The package version is defined in `pyproject.toml` and should be the single source of truth:
```toml
[project]
version = "0.1.0"
```

### Helper Scripts

#### scripts/bump-version.py
```python
#!/usr/bin/env python3
"""Version bumping utility for the package."""
import argparse
import re
import sys
from pathlib import Path


def get_current_version():
    """Get current version from pyproject.toml."""
    pyproject_path = Path("pyproject.toml")
    if not pyproject_path.exists():
        raise FileNotFoundError("pyproject.toml not found")

    content = pyproject_path.read_text()
    match = re.search(r'version = "([^"]+)"', content)
    if not match:
        raise ValueError("Version not found in pyproject.toml")

    return match.group(1)


def parse_version(version_str):
    """Parse version string into components."""
    parts = version_str.split(".")
    if len(parts) != 3:
        raise ValueError(f"Invalid version format: {version_str}")

    try:
        return tuple(int(part) for part in parts)
    except ValueError:
        raise ValueError(f"Version parts must be integers: {version_str}")


def bump_version(current_version, bump_type):
    """Bump version based on type."""
    major, minor, patch = parse_version(current_version)

    if bump_type == "major":
        return f"{major + 1}.0.0"
    elif bump_type == "minor":
        return f"{major}.{minor + 1}.0"
    elif bump_type == "patch":
        return f"{major}.{minor}.{patch + 1}"
    else:
        raise ValueError(f"Invalid bump type: {bump_type}")


def update_version_in_file(new_version):
    """Update version in pyproject.toml."""
    pyproject_path = Path("pyproject.toml")
    content = pyproject_path.read_text()

    # Update version in pyproject.toml
    new_content = re.sub(
        r'version = "[^"]+"',
        f'version = "{new_version}"',
        content
    )
    pyproject_path.write_text(new_content)

    # Update __init__.py if it exists and has a hardcoded version
    init_files = list(Path("src").rglob("__init__.py"))
    for init_file in init_files:
        init_content = init_file.read_text()
        if '__version__ = "' in init_content:
            new_init_content = re.sub(
                r'__version__ = "[^"]+"',
                f'__version__ = "{new_version}"',
                init_content
            )
            init_file.write_text(new_init_content)
            break


def main():
    """Main function."""
    parser = argparse.ArgumentParser(description="Bump package version")
    parser.add_argument(
        "bump_type",
        choices=["major", "minor", "patch"],
        help="Type of version bump"
    )
    parser.add_argument(
        "--dry-run",
        action="store_true",
        help="Show what would be done without making changes"
    )

    args = parser.parse_args()

    try:
        current_version = get_current_version()
        new_version = bump_version(current_version, args.bump_type)

        print(f"Current version: {current_version}")
        print(f"New version: {new_version}")

        if args.dry_run:
            print("Dry run - no changes made")
            return

        update_version_in_file(new_version)
        print(f"Version updated to {new_version}")
        print("Don't forget to:")
        print("1. Update CHANGELOG.md with your changes")
        print("2. Commit the version change: git add . && git commit -m 'Bump version to {new_version}'")
        print("3. Publish using Duty: uv run duty publish-test (then uv run duty publish-prod)")
        print("4. Or create a git tag manually: git tag v{new_version} && git push --tags"))

    except Exception as e:
        print(f"Error: {e}", file=sys.stderr)
        sys.exit(1)


if __name__ == "__main__":
    main()
```

#### scripts/publish.sh
```bash
#!/bin/bash
# Publishing script for PyPI
set -e

# Colors for output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m' # No Color

echo_info() {
    echo -e "${GREEN}[INFO]${NC} $1"
}

echo_warn() {
    echo -e "${YELLOW}[WARN]${NC} $1"
}

echo_error() {
    echo -e "${RED}[ERROR]${NC} $1"
}

# Check if we're in a git repository
if ! git rev-parse --git-dir > /dev/null 2>&1; then
    echo_error "Not in a git repository"
    exit 1
fi

# Check for uncommitted changes
if ! git diff-index --quiet HEAD --; then
    echo_error "You have uncommitted changes. Please commit or stash them first."
    exit 1
fi

# Check if we're on main/master branch
CURRENT_BRANCH=$(git branch --show-current)
if [[ "$CURRENT_BRANCH" != "main" && "$CURRENT_BRANCH" != "master" ]]; then
    echo_warn "Not on main/master branch. Current branch: $CURRENT_BRANCH"
    read -p "Continue anyway? (y/N): " -n 1 -r
    echo
    if [[ ! $REPLY =~ ^[Yy]$ ]]; then
        exit 1
    fi
fi

# Get current version
CURRENT_VERSION=$(python -c "import tomllib; print(tomllib.load(open('pyproject.toml', 'rb'))['project']['version'])")
echo_info "Current version: $CURRENT_VERSION"

# Check if CHANGELOG.md exists
if [[ ! -f "CHANGELOG.md" ]]; then
    echo_error "CHANGELOG.md not found. Please create one before publishing."
    exit 1
fi

# Prompt for changelog entry
echo_warn "Please provide a brief summary of changes for v$CURRENT_VERSION:"
read -p "Change summary: " -r CHANGE_SUMMARY
if [[ -z "$CHANGE_SUMMARY" ]]; then
    echo_error "Change summary is required"
    exit 1
fi

# Update CHANGELOG.md
update_changelog() {
    local version="$1"
    local summary="$2"
    local date=$(date +%Y-%m-%d)

    # Create temporary file
    local temp_file=$(mktemp)

    # Read the changelog and insert new entry
    awk -v version="$version" -v date="$date" -v summary="$summary" '
    /^## \[Unreleased\]/ {
        print $0
        print ""
        print "### Added"
        print ""
        print "### Changed"
        print ""
        print "### Deprecated"
        print ""
        print "### Removed"
        print ""
        print "### Fixed"
        print ""
        print "### Security"
        print ""
        print "## [" version "] - " date
        print ""
        print "### Added"
        print "- " summary
        next
    }
    { print }
    ' CHANGELOG.md > "$temp_file"

    # Replace original file
    mv "$temp_file" CHANGELOG.md

    echo_info "Updated CHANGELOG.md with version $version"
}

# Update changelog
update_changelog "$CURRENT_VERSION" "$CHANGE_SUMMARY"

# Check if tag already exists
if git tag -l | grep -q "^v$CURRENT_VERSION$"; then
    echo_error "Tag v$CURRENT_VERSION already exists"
    exit 1
fi

# Determine target repository
TARGET_REPO="pypi"
if [[ "$1" == "--test" ]]; then
    TARGET_REPO="testpypi"
    echo_info "Publishing to Test PyPI"
else
    echo_info "Publishing to PyPI"
fi

# Run tests
echo_info "Running tests..."
if ! uv run pytest; then
    echo_error "Tests failed"
    exit 1
fi

# Run code quality checks
echo_info "Running code quality checks..."
if ! uv run ruff check .; then
    echo_error "Ruff check failed"
    exit 1
fi

if ! uv run ruff format --check .; then
    echo_error "Code formatting check failed"
    exit 1
fi

if ! uv run pyright; then
    echo_error "Type checking failed"
    exit 1
fi

# Clean previous builds
echo_info "Cleaning previous builds..."
rm -rf dist/ build/ *.egg-info/

# Build package
echo_info "Building package..."
if ! uv run python -m build; then
    echo_error "Build failed"
    exit 1
fi

# Build documentation
echo_info "Building documentation..."
if ! uv run --group docs mkdocs build; then
    echo_warn "Documentation build failed, continuing anyway"
fi

# Check package
echo_info "Checking package..."
if ! uv run python -c "import sys; sys.exit(0 if len(sys.argv) == 1 else 1)"; then
    echo_error "Python validation failed"
    exit 1
fi

# Show what will be uploaded
echo_info "Package contents:"
ls -la dist/

# Confirm upload
echo_warn "Ready to upload to $TARGET_REPO"
read -p "Continue with upload? (y/N): " -n 1 -r
echo
if [[ ! $REPLY =~ ^[Yy]$ ]]; then
    echo_info "Upload cancelled"
    exit 0
fi

# Upload to PyPI
echo_info "Uploading to $TARGET_REPO..."
if [[ "$TARGET_REPO" == "testpypi" ]]; then
    uv publish --index testpypi
else
    uv publish
fi

if [[ $? -eq 0 ]]; then
    echo_info "Upload successful!"

    # Commit changelog update
    echo_info "Committing CHANGELOG.md update..."
    git add CHANGELOG.md
    git commit -m "Update CHANGELOG.md for v$CURRENT_VERSION"

    # Create and push git tag
    echo_info "Creating git tag v$CURRENT_VERSION..."
    git tag "v$CURRENT_VERSION"
    git push origin "v$CURRENT_VERSION"
    git push origin "$(git branch --show-current)"

    echo_info "Package published successfully!"
    if [[ "$TARGET_REPO" == "testpypi" ]]; then
        echo_info "Test installation: pip install --index-url https://test.pypi.org/simple/ your-package-name"
    else
        echo_info "Installation: pip install your-package-name"
    fi
else
    echo_error "Upload failed"
    # Revert changelog changes if upload failed
    echo_info "Reverting CHANGELOG.md changes..."
    git checkout -- CHANGELOG.md
    exit 1
fi
```

### PyPI Configuration

### PyPI Configuration

Create a `.env` file in your project root with your PyPI tokens:

```bash
# .env
UV_PUBLISH_TOKEN=pypi-your-api-token-here
UV_PUBLISH_TOKEN_TESTPYPI=pypi-your-test-api-token-here
```

**Security Note**: Use API tokens instead of passwords. Generate tokens at:
- PyPI: https://pypi.org/manage/account/#api-tokens
- Test PyPI: https://test.pypi.org/manage/account/#api-tokens

The Test PyPI index is already configured in the example `pyproject.toml` above.

### Publishing Workflow

#### 1. Development Release Cycle
```bash
# Make changes and commit
git add .
git commit -m "Add new feature"

# Bump version (patch for bug fixes, minor for features, major for breaking changes)
uv run duty bump-version patch

# Commit version bump
git add .
git commit -m "Bump version to $(python -c "import tomllib; print(tomllib.load(open('pyproject.toml', 'rb'))['project']['version'])")"

# Test publish to Test PyPI first (includes tests, quality checks, and documentation build)
uv run duty publish-test

# If everything looks good, publish to production PyPI
uv run duty publish-prod
```

#### 2. Manual Version Management
```bash
# Check current version
python -c "import tomllib; print(tomllib.load(open('pyproject.toml', 'rb'))['project']['version'])"

# Dry run version bump
python scripts/bump-version.py --dry-run patch

# Actually bump version using Duty
uv run duty bump-version minor

# List all available Duty tasks
uv run duty --list

# Get help for specific tasks
uv run duty --help publish-test
```

#### 3. Quality Assurance Workflow
```bash
# Run full CI pipeline locally before pushing
uv run duty ci

# Run individual checks
uv run duty test                    # Tests with coverage
uv run duty check-quality          # Code formatting and linting
uv run duty build-docs             # Documentation build
uv run duty clean                  # Clean up artifacts

# Debug specific functionality
uv run duty debug-fibonacci n=15   # Interactive debugging
```

### Pre-publication Checklist

Before publishing, ensure:
- [ ] All tests pass: `uv run duty test`
- [ ] Code quality checks pass: `uv run duty check-quality`
- [ ] Version number is appropriate for changes
- [ ] CHANGELOG.md is updated (if you maintain one)
- [ ] README.md is up to date
- [ ] Documentation is built and reviewed: `uv run duty build-docs`
- [ ] All changes are committed
- [ ] LICENSE.md is present and up to date
- [ ] You're on the correct branch (main/master)

### Automated Publishing (Optional)

For automated publishing via GitHub Actions, add this to `.github/workflows/publish.yml`:

```yaml
name: Publish to PyPI

on:
  push:
    tags:
      - 'v*'

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: Install uv
      uses: astral-sh/setup-uv@v2
    - name: Set up Python
      run: uv python install 3.13
    - name: Install dependencies
      run: uv sync --all-extras
    - name: Run tests
      run: uv run pytest
    - name: Build package
      run: uv run python -m build
    - name: Publish to PyPI
      uses: pypa/gh-action-pypi-publish@release/v1
      with:
        password: ${{ secrets.PYPI_API_TOKEN }}
        # Alternative: use UV_PUBLISH_TOKEN environment variable
        # env:
        #   UV_PUBLISH_TOKEN: ${{ secrets.PYPI_API_TOKEN }}
        # run: uv publish
```

### Logging Guidelines with Loguru

Since packages can be used both as libraries and applications, follow these logging best practices:

#### Package Structure for Logging

**src/my_package/__init__.py**
```python
"""My Package - A brief description."""

import importlib.metadata
from loguru import logger

# Disable logging by default when used as a library
logger.disable("my_package")

try:
    __version__ = importlib.metadata.version("my-package")
except importlib.metadata.PackageNotFoundError:
    # Package is not installed, fallback to development version
    __version__ = "0.0.0-dev"

__author__ = "Your Name"
__email__ = "your.email@example.com"

from .core import main_function
from .utils import helper_function

__all__ = ["main_function", "helper_function", "__version__", "configure_logging"]


def configure_logging(level="INFO", format_string=None, file_path=None):
    """
    Configure logging for users of this package.

    This function allows users to enable and customize logging when using
    this package as a library.

    Args:
        level (str): Logging level (DEBUG, INFO, WARNING, ERROR, CRITICAL)
        format_string (str): Custom format string for log messages
        file_path (str): Optional file path to write logs to

    Example:
        >>> import my_package
        >>> my_package.configure_logging(level="DEBUG")
        >>> my_package.main_function()  # Will now show debug logs
    """
    # Enable logging for this package
    logger.enable("my_package")

    # Default format if none provided
    if format_string is None:
        format_string = (
            "<green>{time:YYYY-MM-DD HH:mm:ss}</green> | "
            "<level>{level: <8}</level> | "
            "<cyan>{name}</cyan>:<cyan>{function}</cyan>:<cyan>{line}</cyan> | "
            "<level>{message}</level>"
        )

    # Add console handler
    logger.add(
        sink=lambda msg: print(msg, end=""),
        format=format_string,
        level=level,
        filter=lambda record: record["name"].startswith("my_package")
    )

    # Add file handler if requested
    if file_path:
        logger.add(
            sink=file_path,
            format=format_string,
            level=level,
            rotation="10 MB",
            retention="1 week",
            filter=lambda record: record["name"].startswith("my_package")
        )
```

**src/my_package/core.py**
```python
"""Core functionality for my_package."""

from loguru import logger


def main_function(data: str) -> str:
    """
    Main function that demonstrates proper logging usage.

    Args:
        data: Input data to process

    Returns:
        Processed data
    """
    logger.info("Starting main_function with data: {}", data)

    try:
        # Simulate some processing
        if not data:
            logger.warning("Empty data provided, using default")
            data = "default_value"

        result = data.upper()
        logger.debug("Processed data: {}", result)

        logger.success("Successfully processed data")
        return result

    except Exception as e:
        logger.error("Error processing data: {}", e)
        logger.exception("Full traceback:")
        raise


def calculate_fibonacci(n: int) -> int:
    """Calculate the nth Fibonacci number with logging."""
    logger.debug("Calculating fibonacci for n={}", n)

    if n < 0:
        logger.error("Negative input not supported: {}", n)
        raise ValueError("Fibonacci sequence not defined for negative numbers")

    if n <= 1:
        logger.trace("Base case: fibonacci({}) = {}", n, n)
        return n

    # Log for larger computations
    if n > 10:
        logger.info("Computing fibonacci({}) - this may take a moment", n)

    result = calculate_fibonacci(n - 1) + calculate_fibonacci(n - 2)
    logger.debug("fibonacci({}) = {}", n, result)

    return result
```

**src/my_package/cli.py**
```python
"""Command line interface for my-package."""

import sys
from pathlib import Path

import click
from loguru import logger

from . import __version__, configure_logging


@click.group()
@click.version_option(version=__version__, prog_name="my-package")
@click.option(
    "--verbose", "-v",
    count=True,
    help="Increase verbosity (use -v, -vv, or -vvv)"
)
@click.option(
    "--log-file",
    type=click.Path(path_type=Path),
    help="Write logs to file"
)
@click.option(
    "--quiet", "-q",
    is_flag=True,
    help="Suppress all output except errors"
)
@click.pass_context
def cli(ctx: click.Context, verbose: int, log_file: Path, quiet: bool) -> None:
    """My Package CLI - A brief description of what it does."""
    ctx.ensure_object(dict)

    # Configure logging based on verbosity
    if quiet:
        level = "ERROR"
    elif verbose == 0:
        level = "INFO"
    elif verbose == 1:
        level = "DEBUG"
    else:  # verbose >= 2
        level = "TRACE"

    # When used as CLI application, enable logging
    logger.enable("my_package")

    # Remove default handler to avoid duplicates
    logger.remove()

    # Add console handler with appropriate level
    if not quiet:
        logger.add(
            sys.stderr,
            format="<green>{time:HH:mm:ss}</green> | <level>{level: <8}</level> | <level>{message}</level>",
            level=level,
            colorize=True
        )

    # Add file handler if specified
    if log_file:
        logger.add(
            log_file,
            format="{time:YYYY-MM-DD HH:mm:ss} | {level: <8} | {name}:{function}:{line} | {message}",
            level="DEBUG",  # Always debug level for file
            rotation="10 MB",
            retention="1 week"
        )
        logger.info("Logging to file: {}", log_file)

    # Store configuration for subcommands
    ctx.obj["verbose"] = verbose
    ctx.obj["quiet"] = quiet


@cli.command()
@click.option("--verbose", "-v", is_flag=True, help="Enable verbose output")
def hello(verbose: bool) -> None:
    """Say hello with optional verbose logging."""
    if verbose:
        logger.info("Hello command started")
        logger.debug("Verbose mode enabled")
        click.echo(f"Hello from my-package v{__version__}!")
        logger.success("Hello command completed")
    else:
        click.echo("Hello!")


@cli.command()
@click.argument("number", type=int)
def fibonacci(number: int) -> None:
    """Calculate Fibonacci number with detailed logging."""
    from .core import calculate_fibonacci

    logger.info("Starting fibonacci calculation for n={}", number)

    try:
        result = calculate_fibonacci(number)
        click.echo(f"Fibonacci({number}) = {result}")
        logger.success("Fibonacci calculation completed successfully")

    except ValueError as e:
        logger.error("Invalid input: {}", e)
        click.echo(f"Error: {e}", err=True)
        sys.exit(1)
    except Exception as e:
        logger.exception("Unexpected error during calculation")
        click.echo(f"Unexpected error: {e}", err=True)
        sys.exit(1)


def main() -> None:
    """Entry point for the CLI."""
    cli()


if __name__ == "__main__":
    main()
```

#### Library vs Application Usage

**When used as a Library:**
```python
import my_package

# Logging is disabled by default - no output
result = my_package.main_function("test")

# Users can enable logging if they want
my_package.configure_logging(level="DEBUG", file_path="app.log")
result = my_package.main_function("test")  # Now shows logs
```

**When used as an Application:**
```bash
# Basic usage
my-package hello

# With verbose logging
my-package -v fibonacci 10

# With file logging
my-package --log-file app.log fibonacci 10

# Very verbose with trace level
my-package -vv fibonacci 5
```

#### Testing with Logging

**tests/test_logging.py**
```python
"""Test logging functionality."""

import pytest
from loguru import logger

from my_package import configure_logging
from my_package.core import main_function, calculate_fibonacci


def test_logging_disabled_by_default(capfd):
    """Test that logging is disabled by default for library usage."""
    # Should not produce any log output
    main_function("test")
    captured = capfd.readouterr()
    assert captured.out == ""
    assert captured.err == ""


def test_logging_can_be_enabled(capfd):
    """Test that logging can be enabled for library usage."""
    # Enable logging to stderr
    configure_logging(level="DEBUG")

    try:
        main_function("test")
        captured = capfd.readouterr()

        # Should now see log output
        assert "Starting main_function" in captured.out
        assert "Successfully processed" in captured.out

    finally:
        # Clean up - disable logging again
        logger.disable("my_package")


def test_logging_with_invalid_input():
    """Test logging behavior with invalid input."""
    configure_logging(level="DEBUG")

    try:
        with pytest.raises(ValueError):
            calculate_fibonacci(-1)
    finally:
        logger.disable("my_package")


@pytest.fixture
def temp_log_file(tmp_path):
    """Provide a temporary log file for testing."""
    log_file = tmp_path / "test.log"
    return log_file


def test_file_logging(temp_log_file):
    """Test logging to file."""
    configure_logging(level="INFO", file_path=str(temp_log_file))

    try:
        main_function("test")

        # Check that log file was created and contains expected content
        assert temp_log_file.exists()
        log_content = temp_log_file.read_text()
        assert "Starting main_function" in log_content

    finally:
        logger.disable("my_package")
```

#### Development Workflow with Logging

```bash
# Debug specific function with logging
uv run python -c "
import my_package
my_package.configure_logging(level='TRACE')
result = my_package.calculate_fibonacci(10)
print(f'Result: {result}')
"

# Test CLI with different logging levels
uv run my-package -vv fibonacci 8

# Run tests with logging enabled
uv run pytest tests/test_logging.py -v -s
```

## Development with Duty Task Runner

### Available Duty Tasks

**Setup and Environment:**
- `uv run duty setup-dev` - Set up development environment
- `uv run duty clean` - Clean build artifacts and cache files

**Testing and Quality:**
- `uv run duty test` - Run tests with coverage (aliases: `t`)
- `uv run duty check-quality` - Format and lint code (aliases: `lint`)
- `uv run duty check-all` - Run all quality checks and tests
- `uv run duty ci` - Full CI pipeline locally

**Documentation:**
- `uv run duty docs` - Serve documentation locally (default: port 8000)
- `uv run duty build-docs` - Build documentation for production
- `uv run duty deploy-docs` - Deploy documentation to GitHub Pages

**Version and Publishing:**
- `uv run duty bump-version [major|minor|patch]` - Bump version number
- `uv run duty build-package` - Build package for distribution
- `uv run duty publish-test` - Publish to Test PyPI
- `uv run duty publish-prod` - Publish to production PyPI

**Development Tools:**
- `uv run duty debug-fibonacci [n=10]` - Interactive debugging session
- `uv run duty pre-commit-run` - Run pre-commit hooks

### Cross-Platform Benefits

Using Duty instead of shell scripts provides several advantages:

**✅ Platform Independence:**
- Works identically on Windows (PowerShell), macOS (zsh/bash), and Linux (bash)
- No need for separate `.bat` files or shell-specific syntax

**✅ Python Integration:**
- Direct access to Python APIs and tools
- Consistent error handling and logging
- Type hints and IDE support for task parameters

**✅ Rich Features:**
- Built-in progress indicators and colored output
- Parameter validation and help text
- Task dependencies and pre/post hooks
- Flexible output formatting

**✅ Development Experience:**
- `uv run duty --list` shows all available tasks
- `uv run duty --help task-name` provides detailed help
- Consistent interface across all development operations

### Task Examples with Parameters

```bash
# Test with different configurations
uv run duty test coverage=false           # Skip coverage
uv run duty test parallel=false verbose=true  # Sequential, verbose

# Documentation with custom port
uv run duty docs port=8080

# Quality checks with auto-fix
uv run duty check-quality fix=true

# Debug with specific input
uv run duty debug-fibonacci n=20

# Pre-commit on all files
uv run duty pre-commit-run all-files=true
```

## Important Guidelines

1. **Always use src layout** - Prevents import issues during testing
2. **Write tests first** - Follow TDD religiously
3. **Use type hints everywhere** - Improves code quality and IDE support (Pyright provides excellent IntelliSense)
4. **Proper logging strategy** - Disable logging by default for libraries, enable for CLI applications
5. **Mock external dependencies** - Keep tests fast and reliable
6. **Test edge cases** - Empty inputs, None values, boundary conditions
7. **Use meaningful assertions** - Prefer specific assertions over generic ones
8. **Keep functions small** - Single responsibility principle
9. **Use dependency injection** - Makes code testable
10. **Version consistency** - Ensure version numbers match across package files and CLI
11. **Version pin development dependencies** - Ensures consistent development environment
12. **Document public APIs** - Include examples in docstrings

## Performance Considerations

- Use `pytest-xdist` for parallel test execution
- Mark slow tests with `@pytest.mark.slow`
- Use `pytest.mark.parametrize` for multiple test cases
- Consider using `pytest-benchmark` for performance regression testing
- Profile code with `cProfile` or `py-spy` when needed

Remember: Quality is not an accident - it's the result of systematic practices and tooling.