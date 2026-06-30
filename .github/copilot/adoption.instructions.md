---
description: Instructions for adopting ni-python-styleguide in an existing Python project
keywords:
  - adoption
  - add
  - use
  - linting
  - formatting
  - ni-python-styleguide
---

# Adopting ni-python-styleguide

When a user asks you to help adopt ni-python-styleguide to their Python project, follow these steps:

## For New Projects

This section to come once there is an NI template.

## For Existing Projects

Follow these steps in order:

### Step 1: Remove conflicting linters from dependencies
Remove the following packages from your project's dependencies (search `pyproject.toml`, `requirements*.txt`, or `requirements*.in` files):
- `flake8`
- `flake8-*` plugins (any variants like `flake8-docstrings`, `flake8-bugbear`, etc.)
- `autopep8`
- `pep8-naming`

### Step 2: Add ni-python-styleguide dependency
Add ni-python-styleguide as a **dev dependency** (clients of your library should not need to install it):
```toml
# In pyproject.toml
[tool.poetry.group.dev.dependencies]
ni-python-styleguide = ">=0.5.0"  # Use latest released version
```

### Step 3: Remove flake8 configuration
Remove all flake8-related configuration from the project:
- Delete `.flake8` file if it exists
- Remove `[tool.flake8]` sections from `pyproject.toml` or `setup.cfg`
- Search for any other references to "flake8" in configuration files

### Step 4: Format all existing code
Run:
```bash
poetry run nps format
```

This applies all formatting rules to the existing codebase.

### Step 5: Acknowledge existing violations
Acknowledge all pre-existing style violations so enforcement applies only to new changes:
```bash
poetry run nps fix --aggressive
```

### Step 6: (Optional) Fix easy violations
Review code comments that were added during step 5 and fix any minor violations that are easy to address. This helps improve code quality beyond the minimum requirements.

### Step 7: Check in and enforce
Commit all changes. From this point forward, ni-python-styleguide will enforce style rules on all new code changes (e.g., via CI pipelines).

## Common Issues & Tips

- **Why remove flake8?** ni-python-styleguide is a flake8 aggregator. It provides the curated list of plugins and rules, so having flake8 or other plugins installed can cause conflicts.
- **Why dev dependency?** Consumers of your package don't need to install linting tools unless they're developing.
- **The `--aggressive` flag** on acknowledge-existing-violations ensures all existing violations are marked, and formatted with the acknowledgment, so future runs will only check for new violations.
