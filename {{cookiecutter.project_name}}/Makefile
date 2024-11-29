VENV := ".venv"
PYTHON := $(VENV)/bin/python
PIP := $(VENV)/bin/pip
UV := $(VENV)/bin/uv

.PHONY: init setup sync update

#: Initialise new project, get latest versions for requirements defined in pyproject.toml
init:
	python -m venv $(VENV)
	$(PIP) install -U pip uv
	make update
	make sync
	@echo "\nRemember to activate your virtual environment:"
	@echo "  source $(VENV)/bin/activate"

#: Setup development environment using existing requirements and their versions
setup:
	python -m venv $(VENV)
	$(PIP) install -U pip uv
	make sync
	@echo "\nRemember to activate your virtual environment:"
	@echo "  source $(VENV)/bin/activate"

#: Synchronise virtual environment with requirements files
sync:
	$(UV) pip sync requirements.txt requirements.dev.txt --allow-empty-requirements

#: Update requirements files from pyproject.toml using uv (pip tools)
update:
	$(UV) pip compile pyproject.toml -o requirements.txt
	$(UV) pip compile pyproject.toml --extra dev -o requirements.dev.txt --constraint requirements.txt

.PHONY: test lint typecheck format

#: Execute all tests using pytest
test:
	$(PYTHON) -m pytest

#: Run ruff linter to check code quality
lint:
	$(PYTHON) -m ruff check .

#: Run mypy for static type checking
typecheck:
	$(PYTHON) -m mypy src tests

#: Format code using ruff formatter
format:
	$(PYTHON) -m ruff format .

.PHONY: help

#: Show this help message
help:
	@echo 'Usage:'
	@echo '  make <target>'
	@echo ''
	@echo 'Targets:'
	@awk '/^[a-zA-Z\-\_0-9]+:/ { \
		helpMessage = match(lastLine, /^#: (.*)/); \
		if (helpMessage) { \
			helpCommand = substr($$1, 0, index($$1, ":")-1); \
			helpMessage = substr(lastLine, RSTART + 3, RLENGTH); \
			printf "  %-20s %s\n", helpCommand, helpMessage; \
		} \
	} \
	{ lastLine = $$0 }' $(MAKEFILE_LIST)
