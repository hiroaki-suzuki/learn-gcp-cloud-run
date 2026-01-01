# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a learning project for Google Cloud Run (GCP). It contains a simple Python application designed to be containerized and deployed to Cloud Run.

## Project Structure

- `app/` - Main Python application directory
  - `app/main.py` - Entry point with a simple main() function
  - `pyproject.toml` - Project configuration using hatchling build system
  - `Dockerfile` - Multi-stage Docker build using uv for dependency management
  - `uv.lock` - Locked dependencies managed by uv
  - `ruff.toml` - Ruff linter configuration
  - `ty.toml` - Type checker configuration
- `.devcontainer/` - Development container configuration for VS Code

## Development Environment

The project uses:
- **Python**: 3.14
- **Package Manager**: uv (version 0.9.18)
- **Build System**: hatchling
- **Linter**: Ruff (configured for Python 3.14, line length 88)
- **Type Checker**: ty

## Common Commands

### Package Management
```bash
cd app
uv sync                    # Install all dependencies including dev
uv sync --no-dev          # Install only production dependencies
uv add <package>          # Add a new dependency
uv add --dev <package>    # Add a dev dependency
```

### Running the Application
```bash
cd app
uv run app                # Run via the entry point defined in pyproject.toml
uv run python -m app.main # Alternative way to run
```

### Code Quality
```bash
cd app
ruff check .              # Run linter
ruff format .             # Format code
ty check .                # Run type checker
```

### Building the Container
```bash
cd app
docker build -t app:latest .
```

The Dockerfile uses a multi-stage build:
1. **uv stage**: Provides the uv binary
2. **builder stage**: Installs dependencies using uv with build caching
3. **runner stage**: Final minimal image with non-root user (app:10001)

### Running the Container
```bash
docker run app:latest
```

## Docker Build Details

The Dockerfile uses:
- Pinned Python 3.14.2-slim-trixie image via SHA256
- Pinned uv:0.9.18 via SHA256
- Build caching for uv dependencies
- Non-root user execution (app:10001)
- Multi-stage build to minimize final image size

## Code Style Configuration

**Ruff** (ruff.toml):
- Target: Python 3.14
- Line length: 88
- Enabled rule sets: E (pycodestyle), F (Pyflakes), UP (pyupgrade), B (flake8-bugbear), SIM (flake8-simplify), I (isort)
- Docstring code formatting enabled
- F401 (unused imports) ignored in __init__.py files

**ty** (ty.toml):
- Python version: 3.14
