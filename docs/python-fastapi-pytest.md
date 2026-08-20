# Python FastAPI And Pytest

This stack is useful for Python services, ML workflows, local APIs, and testable automation.

## Used In

- `Trading-ML-LLM` - FastAPI, uvicorn, pandas, numpy, scikit-learn, yfinance, pytest, coverage, Allure.

## Common Dependencies

```text
fastapi
uvicorn
pandas
numpy
scikit-learn
joblib
yfinance
python-dotenv
pytest
pytest-cov
pytest-html
allure-pytest
```

## Commands

```bash
python -m venv .venv
pip install -r requirements.txt
uvicorn app.main:app --reload
pytest
pytest --cov=app --cov-report=term-missing
pytest --alluredir=allure-results
```

## Pytest Defaults

```ini
[pytest]
testpaths = tests
python_files = test_*.py
python_classes = Test*
python_functions = test_*
addopts = -v --strict-markers --tb=short
markers =
    critical: Critical tests that must pass
    integration: Integration tests
    unit: Unit tests
```

## Good Practices

- Keep app imports testable without requiring live secrets.
- Load secrets through `.env` only for local runs.
- Mark slow integration tests separately.
- Run a small import smoke test in CI for FastAPI apps.
- Keep generated reports out of source unless intentionally published.

