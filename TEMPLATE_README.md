# Python Utility Template - Usage Guide

This guide explains how to use the Python Utility Template to create well-structured Python projects optimized for AI-assisted development.

## Quick Start

1. **Install Copier**:
   ```bash
   pip install copier
   # or
   uv tool install copier
   ```

2. **Generate a new project**:
   ```bash
   copier copy https://github.com/rnwolf/copier-py-uv my-new-project
   ```

3. **Follow the prompts** to configure your project

4. **Set up development environment**:
   ```bash
   cd my-new-project
   uv venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   uv pip install -e ".[dev]"
   pre-commit install  # if enabled
   ```

## Template Configuration Options

### Project Metadata
- **project_name**: Human-readable project name
- **project_slug**: Directory/repository name (kebab-case)
- **package_name**: Python package name (snake_case)
- **project_description**: Brief description of the project
- **author_name**: Your full name
- **author_email**: Your email address
- **github_username**: Your GitHub username

### Technical Configuration
- **python_version**: Minimum Python version (3.9-3.13)
- **license**: Project license (MIT, Apache-2.0, GPL-3.0, BSD-3-Clause, Unlicense)

### Feature Toggles
- **include_cli**: Add Click-based command-line interface
- **include_docs**: Add MkDocs documentation setup
- **include_github_actions**: Add GitHub Actions CI/CD workflow
- **include_pre_commit**: Add pre-commit hooks configuration
- **include_submodule**: Include example submodule structure

### Development Tools
- **use_ruff**: Use Ruff for linting and formatting
- **use_pyright**: Use Pyright for type checking
- **use_pytest**: Use pytest instead of unittest

### AI Development Features
- **include_agent_md**: Add .agent.md files for AI development context

## Generated Project Structure

```
my-project/
├── .agent.md                  # AI development context
├── .env                       # Environment variables template
├── .gitignore                 # Comprehensive gitignore
├── .pre-commit-config.yaml    # Pre-commit hooks (optional)
├── CHANGELOG.md              # Version history
├── LICENSE.md                # License file
├── README.md                 # Project documentation
├── duties.py                 # Task automation
├── pyproject.toml            # Project configuration
├── src/
│   └── my_package/
│       ├── __init__.py       # Package initialization
│       ├── __main__.py       # Module execution entry
│       ├── cli.py            # CLI interface (optional)
│       ├── core.py           # Core functionality
│       ├── utils.py          # Utility functions
│       └── submodule/        # Example submodule (optional)
├── tests/                    # Comprehensive test suite
├── docs/                     # Documentation (optional)
├── scripts/                  # Utility scripts
└── .github/workflows/        # CI/CD workflows (optional)
```

## Development Workflow

### Initial Setup
```bash
# Set up development environment
duty setup

# Or manually:
uv venv
source .venv/bin/activate
uv pip install -e ".[dev,docs]"
pre-commit install
```

### Daily Development
```bash
# Run tests
duty test

# Run all quality checks
duty check

# Format and lint code
duty lint --fix
duty format_code

# Type checking
duty typecheck

# Build documentation
duty docs

# Build package
duty build
```

### Release Process
```bash
# Create a new release (bumps version, runs CI, creates git tag)
duty release patch  # or minor, major

# Publish to PyPI (uses uv publish)
duty publish
```

## AI Development Features

### .agent.md Files
The template includes `.agent.md` files that provide context for AI coding assistants:

- Project structure and conventions
- Development practices and patterns
- Code style guidelines
- Testing strategies
- Common tasks and workflows

### Optimized Structure
- Clear, consistent file organization
- Comprehensive type hints
- Well-documented APIs
- Test-driven development setup
- Automated quality checks

## Customization

### Adding New Features
1. Add source files to `src/package_name/`
2. Add corresponding tests to `tests/`
3. Update documentation
4. Add any new dependencies to `pyproject.toml`

### Modifying the Template
1. Fork this repository
2. Modify the template files in `{{project_slug}}/`
3. Update `copier.yml` if adding new configuration options
4. Test with `copier copy . test-project`

### Template Updates
To update an existing project when the template changes:

```bash
copier update
```

This will apply template updates while preserving your customizations.

## Best Practices

### Code Organization
- Use the `src/` layout for better import handling
- Keep modules focused and cohesive
- Separate business logic from CLI/UI code
- Use type hints throughout

### Testing
- Write tests before implementation (TDD)
- Maintain high test coverage (>80%)
- Use descriptive test names
- Test both success and failure cases

### Documentation
- Write comprehensive docstrings
- Keep README up-to-date
- Document configuration options
- Provide usage examples

### Version Management
- Follow semantic versioning
- Update CHANGELOG.md for each release
- Use git tags for releases
- Test releases on Test PyPI first

## Troubleshooting

### Common Issues

**Import errors during testing:**
- Ensure you're using the `src/` layout correctly
- Install package in development mode: `pip install -e .`

**Pre-commit hooks failing:**
- Run `pre-commit install` after setup
- Fix issues with `duty lint --fix`

**Type checking errors:**
- Add missing type hints
- Update pyright configuration in `pyproject.toml`

**Documentation build failures:**
- Check MkDocs configuration in `docs/mkdocs.yml`
- Ensure all referenced files exist

### Getting Help

1. Check the generated project's README.md
2. Review the .agent.md files for AI development context
3. Look at the example code in the template
4. Check the GitHub Issues for common problems

## Contributing to the Template

We welcome contributions to improve the template:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test with multiple project configurations
5. Submit a pull request

### Template Development

When working on the template:

- Test with different configuration combinations
- Ensure all conditional logic works correctly
- Update documentation for new features
- Maintain backward compatibility when possible

## License

This template is licensed under the MIT License. Generated projects will use the license you select during generation.