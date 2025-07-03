# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Fixed
- Fixed import sorting in all template files to prevent Ruff I001 errors
- Properly grouped imports by standard library, third-party, and local modules
- Added blank lines between import groups for better readability

## [0.2.0] - 2024-07-03

### Added
- Modern Python typing support using built-in types (`list`, `dict`, `tuple`) instead of deprecated `typing` imports
- Support for Python 3.10+ union syntax (`X | Y`) instead of `Union[X, Y]` and `Optional[X]`
- Modern `isinstance()` calls using pipe syntax instead of tuple syntax
- Proper conditional file generation for submodules based on `include_submodule` setting
- Robust handling of empty author information in `pyproject.toml` generation

### Fixed
- Fixed `.copier-answers.yml` generation to properly record all project metadata using `{{_copier_answers|to_nice_yaml}}`
- Fixed deprecated typing imports that caused linting errors:
  - `typing.List` → `list`
  - `typing.Dict` → `dict` 
  - `typing.Tuple` → `tuple`
  - `Optional[X]` → `X | None`
  - `Union[X, Y]` → `X | Y`
- Fixed `isinstance()` calls to use modern syntax: `isinstance(x, int | float)` instead of `isinstance(x, (int, float))`
- Fixed author details not being populated in `pyproject.toml` when empty values are provided
- Fixed duplicate `_tasks` sections in `copier.yml`
- Fixed submodule files being created even when `include_submodule=false`

### Changed
- Updated all template files to use modern Python 3.9+ typing annotations
- Improved template generation process to properly handle conditional content
- Enhanced error handling for missing or invalid author information

### Technical Improvements
- All generated code now passes Ruff linting checks without typing-related errors
- Template follows modern Python best practices for type annotations
- Improved maintainability by using Copier's built-in mechanisms for metadata recording

## [0.1.0] - 2024-07-02

### Added
- Initial release of copier-py-uv template
- Python project template optimized for AI-assisted development
- Support for modern Python packaging with uv
- CLI interface using Click
- Documentation setup with MkDocs
- GitHub Actions CI/CD workflow
- Pre-commit hooks configuration
- Comprehensive testing setup with pytest
- Code quality tools (Ruff, Pyright)
- Example submodule structure
- AI development context with .agent.md files

[Unreleased]: https://github.com/yourusername/copier-py-uv/compare/v0.2.0...HEAD
[0.2.0]: https://github.com/yourusername/copier-py-uv/compare/v0.1.0...v0.2.0
[0.1.0]: https://github.com/yourusername/copier-py-uv/releases/tag/v0.1.0