# Allure Reports

Allure turns test output into browsable reports, useful when CI failures need more context than terminal logs.

## Used In

- `Trading-ML-LLM` - pytest generates Allure results and GitHub Actions publishes reports.

## Python Setup

```bash
pip install allure-pytest
pytest --alluredir=allure-results
allure generate allure-results -o allure-report --clean
```

## GitHub Actions Setup

```yaml
- name: Set up Java
  uses: actions/setup-java@v4
  with:
    distribution: temurin
    java-version: "17"

- name: Install Allure CLI
  run: |
    curl -o allure.tgz -Ls https://github.com/allure-framework/allure2/releases/download/2.27.0/allure-2.27.0.tgz
    tar -zxvf allure.tgz
    sudo mv allure-2.27.0 /opt/allure
    sudo ln -s /opt/allure/bin/allure /usr/bin/allure

- name: Generate Allure Report
  if: always()
  run: allure generate allure-results -o allure-report --clean
```

## What To Save

- `allure-results/` - raw test result data.
- `allure-report/` - generated static report.
- CI artifacts - short retention backup.
- Published report branch or Pages folder when reports need public access.

## Checklist

- Report generation runs with `if: always()`.
- Failed test details are included.
- Report publishing does not expose secrets.
- Old report history is pruned.

