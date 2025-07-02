# Template Examples

This directory contains examples of projects generated with different configurations using the Python Utility Template.

## Example Configurations

### 1. CLI Application with Full Features
```bash
copier copy . example-cli-app
```

Configuration:
- include_cli: true
- include_docs: true
- include_github_actions: true
- include_pre_commit: true
- include_submodule: true
- use_ruff: true
- use_pyright: true
- use_pytest: true

**Use case**: Command-line tools, data processing utilities, automation scripts

### 2. Python Library (Minimal)
```bash
copier copy . example-library
```

Configuration:
- include_cli: false
- include_docs: true
- include_github_actions: true
- include_pre_commit: true
- include_submodule: false
- use_ruff: true
- use_pyright: true
- use_pytest: true

**Use case**: Reusable libraries, API clients, utility packages

### 3. Data Science Utility
```bash
copier copy . example-data-science
```

Configuration:
- include_cli: true
- include_docs: true
- include_github_actions: false
- include_pre_commit: true
- include_submodule: true
- use_ruff: true
- use_pyright: false  # Often too strict for data science
- use_pytest: true

**Use case**: Data analysis tools, ML utilities, research projects

### 4. Web Service/API
```bash
copier copy . example-web-service
```

Configuration:
- include_cli: false
- include_docs: true
- include_github_actions: true
- include_pre_commit: true
- include_submodule: true
- use_ruff: true
- use_pyright: true
- use_pytest: true

**Use case**: FastAPI services, Flask applications, microservices

### 5. Educational/Learning Project
```bash
copier copy . example-learning
```

Configuration:
- include_cli: true
- include_docs: false
- include_github_actions: false
- include_pre_commit: false
- include_submodule: false
- use_ruff: true
- use_pyright: false
- use_pytest: false  # Use unittest for simplicity

**Use case**: Learning projects, tutorials, simple scripts

## Testing Different Configurations

To test the template with different configurations:

```bash
# Test CLI application
copier copy . test-cli --data project_name="Test CLI App" --data include_cli=true

# Test library
copier copy . test-lib --data project_name="Test Library" --data include_cli=false

# Test with minimal features
copier copy . test-minimal --data include_docs=false --data include_github_actions=false
```

## Generated Project Examples

Each example shows:
- Different project structures based on configuration
- How features are conditionally included
- Best practices for different project types
- Common patterns and conventions

## Running Examples

After generating an example:

```bash
cd example-project
uv venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
uv pip install -e ".[dev]"
duty test
duty check
```

**Note**: The virtual environment activation command differs by operating system:
- **Linux/macOS**: `source .venv/bin/activate`
- **Windows (Command Prompt)**: `.venv\Scripts\activate.bat`
- **Windows (PowerShell)**: `.venv\Scripts\Activate.ps1`

## Contributing Examples

To add a new example configuration:

1. Create a new section in this README
2. Document the use case and configuration
3. Test the configuration thoroughly
4. Add any special setup instructions