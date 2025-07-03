# Python Utility Template

A comprehensive [Copier](https://copier.readthedocs.io/) template for creating well-structured Python utility projects optimized for AI-assisted development with tools like Claude Code.

## Features

🚀 **Modern Python Development**
- Uses `uv` for fast dependency management and publishing
- `src/` layout for better import handling
- Comprehensive `pyproject.toml` configuration
- Type checking with Pyright
- Linting and formatting with Ruff

🧪 **Testing & Quality**
- pytest with coverage reporting
- Pre-commit hooks for code quality
- GitHub Actions CI/CD pipeline
- Comprehensive test structure

📚 **Documentation**
- MkDocs with Material theme
- Auto-generated API documentation
- Markdown-based documentation

🤖 **AI Development Optimized**
- `.agent.md` files for AI context
- Clear project structure for AI tools
- Comprehensive development instructions
- TDD-friendly setup

🛠️ **Development Tools**
- Task automation with `duty`
- Version management scripts
- Development environment setup
- CLI interface with Click

## Quick Start

1. **Install Copier and extensions** (if you haven't already):
   ```bash
   pipx install copier copier_templates_extensions
   # or
   uv tool install --with copier_templates_extensions copier
   ```

2. **Generate a new project**:
   ```bash
   copier copy https://github.com/rnwolf/copier-py-uv my-new-project
   ```

3. **Set up the development environment**:
   ```bash
   cd my-new-project
   uv venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   uv pip install -e ".[dev]"
   ```

4. **Initialize development tools**:
   ```bash
   pre-commit install
   duty setup
   ```

## Template Structure

```
copier-py-uv/
├── copier.yml                    # Main configuration
├── README.md                     # Template documentation
├── TEMPLATE_README.md            # Instructions for template users
├── template/                     # Template root
│   ├── .copier-answers.yml.jinja # For template updates
│   ├── pyproject.toml.jinja      # Project configuration
│   ├── README.md.jinja           # Generated project README
│   ├── .gitignore.jinja          # Git ignore patterns
│   ├── src/                      # Source code templates
│   ├── tests/                    # Test templates
│   ├── docs/                     # Documentation templates
│   └── scripts/                  # Utility script templates
└── examples/                     # Example configurations
```

## Template Options

The template will prompt you for various configuration options:

- **Project Information**: Name, description, author details
- **Python Version**: Minimum Python version (3.12-3.13)
- **License**: MIT, Apache-2.0, GPL-3.0, BSD-3-Clause, or Unlicense
- **Features**: CLI interface, documentation, GitHub Actions, etc.
- **Development Tools**: Ruff, Pyright, pytest configuration
- **AI Optimization**: Include .agent.md files for AI development context

## Generated Project Structure

```
my-project/
├── pyproject.toml              # Project configuration and dependencies
├── README.md                   # Project documentation with usage examples
├── CHANGELOG.md               # Version history and release notes
├── LICENSE.md                 # License file (MIT, Apache-2.0, GPL-3.0, BSD-3-Clause, or Unlicense)
├── .env                       # Environment variables template
├── .gitignore                 # Comprehensive Git ignore patterns
├── .pre-commit-config.yaml    # Pre-commit hooks for code quality (optional)
├── duties.py                  # Task automation with 20+ development tasks
├── .agent.md                  # AI development context and guidelines (optional)
├── src/
│   └── my_project/
│       ├── __init__.py        # Package initialization with version and exports
│       ├── __main__.py        # Module execution entry point (python -m my_project)
│       ├── cli.py             # Click-based command-line interface (optional)
│       ├── core.py            # Core business logic and main functions
│       ├── utils.py           # Utility functions (logging, file ops, env handling)
│       └── submodule/         # Example submodule structure (optional)
│           ├── __init__.py    # Submodule initialization
│           └── feature.py     # Feature processing with classes and functions
├── tests/
│   ├── __init__.py            # Test package initialization
│   ├── conftest.py            # Pytest fixtures and test configuration
│   ├── test_version.py        # Version validation tests
│   ├── test_cli.py            # Command-line interface tests (optional)
│   ├── test_core.py           # Core functionality tests
│   ├── test_utils.py          # Utility function tests
│   └── test_submodule/        # Submodule tests (optional)
│       ├── __init__.py        # Test submodule initialization
│       └── test_feature.py    # Feature processing tests
├── docs/                      # Documentation (optional)
│   ├── index.md               # Main documentation page
│   ├── api.md                 # Auto-generated API reference
│   ├── mkdocs.yml             # MkDocs configuration
│   └── gen_ref_pages.py       # API documentation generator script
├── scripts/
│   ├── bump-version.py        # Semantic version bumping script
│   └── gen_ref_pages.py       # Manual API documentation generator
└── .github/workflows/         # CI/CD workflows (optional)
    └── ci.yml                 # GitHub Actions workflow for testing, building, and publishing
```

### File Purposes Explained

#### Core Configuration
- **`pyproject.toml`**: Complete project configuration including dependencies, tool settings (ruff, pyright, pytest), build system, and metadata
- **`duties.py`**: Task automation with commands for testing, linting, building, publishing, and development workflow
- **`.env`**: Template for environment variables with examples for development settings
- **`.gitignore`**: Comprehensive ignore patterns for Python, IDEs, OS files, and build artifacts

#### Source Code Structure
- **`src/my_project/__init__.py`**: Package entry point with version, author info, and public API exports
- **`src/my_project/__main__.py`**: Enables running package as module (`python -m my_project`)
- **`src/my_project/core.py`**: Main business logic with example functions, statistics, and data processing
- **`src/my_project/cli.py`**: Full-featured CLI with multiple commands, options, and error handling (if enabled)
- **`src/my_project/utils.py`**: Utility functions for logging, file operations, environment handling, and data transformation
- **`src/my_project/submodule/`**: Example of modular code organization with classes and processing functions (if enabled)

#### Test Suite
- **`tests/conftest.py`**: Pytest fixtures for sample data, temporary files, CLI testing, and mock objects
- **`tests/test_*.py`**: Comprehensive tests with parametrized tests, edge cases, and integration tests
- **Test structure mirrors source**: Each source file has corresponding test file with same name pattern

#### Documentation (Optional)
- **`docs/index.md`**: Main documentation with features, usage examples, and development guide
- **`docs/api.md`**: Auto-generated API reference using mkdocstrings
- **`docs/mkdocs.yml`**: MkDocs configuration with Material theme, plugins, and navigation
- **`docs/gen_ref_pages.py`**: Script for generating API documentation from docstrings

#### Development Tools
- **`scripts/bump-version.py`**: Intelligent version bumping with semantic versioning and changelog updates
- **`scripts/gen_ref_pages.py`**: Manual API documentation generation by parsing source code
- **`.pre-commit-config.yaml`**: Code quality hooks for formatting, linting, type checking, and security (if enabled)

#### CI/CD (Optional)
- **`.github/workflows/ci.yml`**: Complete GitHub Actions workflow with:
  - Multi-OS and multi-Python version testing
  - Code quality checks (linting, formatting, type checking)
  - Security scanning
  - Package building and publishing
  - Documentation deployment
  - Automated releases

#### AI Development Features (Optional)
- **`.agent.md`**: Comprehensive context for AI coding assistants including:
  - Project structure and conventions
  - Development practices and patterns
  - Code style guidelines and examples
  - Testing strategies and workflows
  - Common tasks and troubleshooting

## AI Development Features

This template is specifically optimized for AI-assisted development:

### .agent.md Files
- Provide context and instructions for AI coding assistants
- Include development patterns and best practices
- Guide AI tools on project structure and conventions

### Clear Structure
- Consistent file organization
- Descriptive naming conventions
- Comprehensive type hints
- Well-documented code patterns

### TDD-Ready
- Pre-configured pytest setup
- Test structure mirrors source structure
- Coverage reporting and quality gates
- Parallel test execution support

## Development Workflow

The generated projects follow these best practices:

1. **Test-Driven Development**: Write tests first, then implement
2. **Type Safety**: Use type hints and Pyright for type checking
3. **Code Quality**: Automated linting, formatting, and pre-commit hooks
4. **Documentation**: Keep docs up-to-date with code changes
5. **Version Management**: Semantic versioning with automated tools

## Available Tasks

Generated projects include these `duty` tasks:

- `duty test` - Run the test suite
- `duty lint` - Run linting and formatting
- `duty docs` - Build documentation
- `duty build` - Build the package
- `duty publish` - Publish to PyPI
- `duty setup` - Set up development environment

## Requirements

- Python 3.13+
- [uv](https://github.com/astral-sh/uv) for dependency management
- [Copier](https://copier.readthedocs.io/) for template generation

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This template is licensed under the MIT License. Generated projects will use the license you select during generation.

## Related

- [Copier Documentation](https://copier.readthedocs.io/)
- [uv Documentation](https://github.com/astral-sh/uv)
- [Claude Code](https://claude.ai/chat) - AI coding assistant