# Telegram Automation

Telegram notifications are useful for scheduled jobs, trackers, CI status, and long-running automation that should report back without opening GitHub.

## Used In

- `Trading-ML-LLM` - daily learning reports and failure notifications.
- `Shopping` - test result and product detail notifications.

## Required Secrets

```text
TELEGRAM_TOKEN
TELEGRAM_CHAT_ID
```

## Shell Example

```bash
curl -s -X POST "https://api.telegram.org/bot${TELEGRAM_TOKEN}/sendMessage" \
  -d chat_id="${TELEGRAM_CHAT_ID}" \
  --data-urlencode text="Job finished"
```

## Python Example

```python
import os
import requests

def send_telegram(message: str) -> None:
    token = os.environ["TELEGRAM_TOKEN"]
    chat_id = os.environ["TELEGRAM_CHAT_ID"]
    requests.post(
        f"https://api.telegram.org/bot{token}/sendMessage",
        data={"chat_id": chat_id, "text": message},
        timeout=20,
    )
```

## Good Uses

- Scheduled workflow completed.
- Test suite failed.
- Product or price tracker found a change.
- Report URL is ready.
- Long-running automation needs human attention.

## Checklist

- Keep messages short.
- Include a run link when triggered from GitHub Actions.
- Send failure notifications with `if: failure()` or `if: always()`.
- Never print tokens.

