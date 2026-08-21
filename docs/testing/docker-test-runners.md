# Docker Test Runners

Dockerized test runners make CI behavior more repeatable, especially for Java browser automation and Python services.

## Used In

- `Shopping` - Docker image runs Maven Selenium/TestNG tests.
- `Trading-ML-LLM` - Dockerfile for Python service/runtime.

## Good Uses

- CI test environments.
- Browser automation dependencies.
- Reproducible local runs.
- Isolating system packages from the host.

## Example Commands

```bash
docker build -t project-tests .
docker run --rm project-tests
docker run --rm -v "$PWD:/workspace" project-tests
```

## CI Pattern

```yaml
- name: Build Docker image
  run: docker build -t project-tests -f Dockerfile .

- name: Run tests
  run: docker run --rm project-tests
```

## Checklist

- Keep images small enough for CI.
- Avoid baking secrets into images.
- Mount output folders when reports must be saved.
- Print test output to stdout.
- Pin base image versions when stability matters.

