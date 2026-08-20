# GitHub Actions

GitHub Actions is the default automation layer across projects: builds, tests, scheduled jobs, reports, notifications, and release checks.

## Used In

- `Trading-ML-LLM` - scheduled daily learning, repo hygiene, Hugging Face sync.
- `Workout-Timer` - Android build automation.
- `Shopping` - Dockerized Selenium/TestNG run with Telegram notification.
- `Naukri` - resume upload automation.
- `Swiggy-Instamart` - scheduled tracker and keep-alive jobs.

## Good Uses

- Run tests on push and pull request.
- Run scheduled jobs with `cron`.
- Publish artifacts such as Allure reports, logs, and generated summaries.
- Send status notifications to Telegram.
- Keep repos healthy with custom hygiene scripts.

## Starter Workflow

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
  workflow_dispatch:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run checks
        run: echo "replace with project checks"
```

## Patterns To Reuse

- Use `workflow_dispatch` on anything you may want to run manually.
- Add `concurrency` for scheduled jobs so stale runs cancel.
- Put secrets in GitHub repo secrets, never in workflow files.
- Upload reports and logs with `actions/upload-artifact`.
- Keep notification steps under `if: always()` when failures need alerts.

## Checklist

- Trigger is correct: push, PR, schedule, manual.
- Secrets are documented.
- Artifacts have retention days.
- Failure path is visible.
- Long jobs have timeouts.

