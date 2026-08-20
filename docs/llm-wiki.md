# LLM Wiki

An LLM wiki is a generated or curated knowledge base that helps humans and AI assistants navigate a project without repeatedly rediscovering the same context.

## Purpose

- Give fast orientation to a project.
- Preserve architecture explanations.
- Make common workflows easy to find.
- Capture decisions, tradeoffs, and setup steps.
- Reduce repeated context gathering during AI-assisted work.

## Useful Sections

- `Overview` - what the project does and who uses it.
- `Architecture` - major modules and how they connect.
- `Workflows` - build, test, release, debug, deploy.
- `Domain Concepts` - product-specific terms and rules.
- `Important Files` - entry points, config files, shared modules.
- `Common Tasks` - repeatable implementation and maintenance steps.
- `Known Risks` - fragile areas, tricky integrations, migration notes.

## Good LLM Wiki Rules

- Prefer short pages with clear links over one giant document.
- Include commands exactly as they should be run.
- Separate stable architecture from temporary investigation notes.
- Keep generated content reviewable and editable.
- Update the wiki after meaningful architecture or workflow changes.

## Example Layout

```text
wiki/
  index.md
  architecture.md
  workflows.md
  testing.md
  release.md
  domain-model.md
  integrations.md
  troubleshooting.md
```

## Evaluation Questions

- Can a new contributor understand the project faster with this wiki?
- Can an AI assistant answer repo questions with fewer raw file reads?
- Are commands current and copy-pasteable?
- Does the wiki distinguish facts from guesses?
- Is there a simple update workflow?
