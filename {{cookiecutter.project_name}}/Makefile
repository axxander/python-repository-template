VENV := ".venv"
PYTHON := $(VENV)/bin/python
PIP := $(VENV)/bin/pip
UV := $(VENV)/bin/uv

.PHONY: setup sync update 

setup:
	python -m venv $(VENV)
	$(PIP) install -U pip uv
	make sync

sync:
	$(UV) pip sync requirements.txt requirements.dev.txt

update:
	$(UV) pip compile pyproject.toml -o requirements.txt
	$(UV) pip compile pyproject.toml --extra dev -o requirements.dev.txt --constraint requirements.txt

.PHONY: test lint typecheck format

test:
	$(PYTHON) -m pytest

lint:
	$(PYTHON) -m ruff check .

typecheck:
	$(PYTHON) -m mypy src tests

format:
	$(PYTHON) -m ruff format .
