# Agent Browser (Vercel Labs)

## Summary

Agent Browser is a fast native Rust CLI built specifically for AI agent browser automation. It provides AI-friendly accessibility snapshots with stable element references (@e1, @e2), MCP server support, session management, cloud provider integrations, and security features like domain allowlists and content boundaries. Designed for headless or headed automation with minimal context overhead.

## Best For

- AI agent browser automation with LLM-friendly element references
- Headless browser testing and web scraping for agents
- Multi-session isolated browser environments
- Cloud browser deployments (Browserbase, Browserless, Browser Use, Kernel, AgentCore)
- iOS Simulator automation for mobile web testing
- Authentication state persistence across sessions
- MCP (Model Context Protocol) server integration
- Serverless browser automation (Vercel, AWS Lambda)

## Avoid When

- You need a visual browser automation IDE (use Playwright Inspector instead)
- You need cross-browser testing (focused on Chromium/Safari)
- You need advanced video recording (use Playwright for built-in video)
- You're building for non-technical end users (CLI-focused tool)
- You need browser extensions for Chrome (supported but not the primary focus)

## Setup

### Global Installation (Recommended)

```bash
npm install -g agent-browser
agent-browser install  # Download Chrome from Chrome for Testing
```

### Homebrew (macOS)

```bash
brew install agent-browser
agent-browser install
```

### Cargo (Rust)

```bash
cargo install agent-browser
agent-browser install
```

### Verify Installation

```bash
agent-browser --version
agent-browser doctor  # Diagnose installation and environment
```

## Core Workflow

### Basic Usage

```bash
# Navigate to a page
agent-browser open example.com

# Get AI-friendly snapshot with refs
agent-browser snapshot -i
# Output:
# - heading "Example Domain" [ref=e1] [level=1]
# - button "Submit" [ref=e2]
# - textbox "Email" [ref=e3]
# - link "Learn more" [ref=e4]

# Interact using refs
agent-browser click @e2                    # Click button
agent-browser fill @e3 "test@example.com"  # Fill input
agent-browser get text @e1                 # Get heading text
agent-browser screenshot page.png          # Take screenshot

# Close browser
agent-browser close
```

### Interactive Snapshot Options

```bash
agent-browser snapshot -i              # Interactive elements only
agent-browser snapshot -i --urls       # Include link URLs
agent-browser snapshot -c              # Compact (remove empty nodes)
agent-browser snapshot -d 3            # Limit depth to 3 levels
agent-browser snapshot -s "#main"      # Scope to CSS selector
agent-browser snapshot -i -c -d 5      # Combine options
```

## Session Management

```bash
# Different sessions = isolated browser instances
agent-browser --session agent1 open site-a.com
agent-browser --session agent2 open site-b.com

# Or via environment variable
export AGENT_BROWSER_SESSION=agent1
agent-browser click "#btn"

# List active sessions
agent-browser session list

# Generate stable session ID
agent-browser session id --scope worktree --prefix myapp
```

## Authentication & State Persistence

### Session Persistence (Automatic)

```bash
# Generate stable session ID
SESSION="$(agent-browser session id --scope worktree --prefix twitter)"

# Auto-save/restore state
agent-browser --session "$SESSION" --restore open twitter.com

# Login once, then state persists automatically
agent-browser --session "$SESSION" --restore open twitter.com/dashboard
```

### Chrome Profile Reuse

```bash
# List available Chrome profiles
agent-browser profiles

# Use existing Chrome profile
agent-browser --profile Default open gmail.com
agent-browser --profile "Work" open app.example.com
```

### State Files

```bash
# Save authenticated state
agent-browser --auto-connect state save ./my-auth.json

# Load state in future sessions
agent-browser --state ./my-auth.json open app.example.com

# State encryption (AES-256-GCM)
export AGENT_BROWSER_ENCRYPTION_KEY=<64-char-hex-key>
agent-browser --session secure --restore open example.com
```

### Auth Vault

```bash
# Store credentials locally (encrypted)
echo "password" | agent-browser auth save github \
  --url https://github.com/login \
  --username user \
  --password-stdin

# Login by name
agent-browser auth login github
```

## Advanced Features

### Annotated Screenshots

```bash
# Screenshot with numbered element labels
agent-browser screenshot --annotate ./page.png
# Output:
#    [1] @e1 button "Submit"
#    [2] @e2 link "Home"
#    [3] @e3 textbox "Email"

# Then interact with labeled elements
agent-browser click @e2  # Click [2]
```

### Batch Execution

```bash
# Multiple commands in one invocation
agent-browser batch \
  "open https://example.com" \
  "snapshot -i" \
  "screenshot"

# Stop on first error
agent-browser batch --bail "open example.com" "click @e1" "screenshot"

# Stdin mode with JSON
echo '[
  ["open", "https://example.com"],
  ["snapshot", "-i"],
  ["click", "@e1"]
]' | agent-browser batch --json
```

### Network Control

```bash
# Intercept and mock requests
agent-browser network route "*/api/*" --body '{"status":"ok"}'
agent-browser network route "*/ads/*" --abort

# Block scripts
agent-browser network route '*' --abort --resource-type script

# HAR recording
agent-browser network har start
agent-browser open example.com
agent-browser network har stop output.har

# View requests
agent-browser network requests
agent-browser network requests --filter api
agent-browser network requests --status 2xx
```

### Read Agent-Friendly Text

```bash
# Fetch URL without launching Chrome
agent-browser read https://example.com/article

# Read rendered DOM of active tab
agent-browser read

# Outline headings
agent-browser read https://example.com --outline

# Find docs via llms.txt
agent-browser read https://docs.example.com --llms index --filter auth
agent-browser read https://docs.example.com --llms full --filter auth

# Require markdown
agent-browser read example.com/article --require-md
```

### React DevTools Integration

```bash
# Launch with React DevTools hook
agent-browser open --enable react-devtools https://app.example.com

# Inspect React components
agent-browser react tree                    # Full component tree
agent-browser react inspect <fiberId>       # Props, hooks, state
agent-browser react renders start           # Record renders
agent-browser react renders stop --json     # Profile data
agent-browser react suspense --only-dynamic # Suspense boundaries

# Web Vitals (framework-agnostic)
agent-browser vitals https://example.com --json
```

### Accessibility Audits

```bash
# Run axe-core audit
agent-browser a11y                                # Current page
agent-browser a11y https://example.com            # Navigate then audit
agent-browser a11y --tags wcag2a,wcag2aa          # Specific rules
agent-browser a11y --selector "#main"             # Scope to subtree
agent-browser a11y example.com --json             # Full results
```

### Diff Commands

```bash
# Snapshot diff
agent-browser diff snapshot
agent-browser diff snapshot --baseline before.txt
agent-browser diff snapshot --selector "#main" --compact

# Visual screenshot diff
agent-browser diff screenshot --baseline before.png
agent-browser diff screenshot --baseline b.png -o diff.png
agent-browser diff screenshot --baseline b.png -t 0.2  # Threshold

# Compare two URLs
agent-browser diff url https://v1.com https://v2.com
agent-browser diff url https://v1.com https://v2.com --screenshot
```

## MCP Server

```bash
# Start MCP stdio server
agent-browser mcp

# With specific tool profiles
agent-browser mcp --tools all
agent-browser mcp --tools core,network,react
```

**MCP Client Config:**

```json
{
  "mcpServers": {
    "agent-browser": {
      "command": "agent-browser",
      "args": ["mcp"]
    }
  }
}
```

**Tool Profiles:**
- `core` (default) - Navigation, snapshots, interaction, waits, screenshots
- `network` - Network routes, HAR, headers, credentials
- `state` - Cookies, storage, auth, sessions
- `debug` - Console, tracing, profiling, a11y, clipboard
- `tabs` - Back/forward, tabs, windows, frames
- `react` - React tree/inspect/renders, vitals
- `mobile` - Viewport, device, geolocation, touch
- `all` - Every MCP tool

## Cloud Providers

### Browserbase

```bash
export BROWSERBASE_API_KEY="your-api-key"
agent-browser -p browserbase open https://example.com
```

### Browserless

```bash
export BROWSERLESS_API_KEY="your-api-token"
agent-browser -p browserless open https://example.com

# Optional config
export BROWSERLESS_API_URL="https://production-sfo.browserless.io"
export BROWSERLESS_BROWSER_TYPE="chromium"
export BROWSERLESS_STEALTH="true"
```

### Browser Use

```bash
export BROWSER_USE_API_KEY="your-api-key"
agent-browser -p browseruse open https://example.com
```

### Kernel

```bash
export KERNEL_API_KEY="your-api-key"
agent-browser -p kernel open https://example.com

# Optional config
export KERNEL_HEADLESS="true"
export KERNEL_STEALTH="false"
export KERNEL_PROFILE_NAME="my-profile"  # Persistent cookies/logins
```

### AgentCore (AWS Bedrock)

```bash
export AGENT_BROWSER_PROVIDER=agentcore
agent-browser open https://example.com

# Optional config
export AGENTCORE_REGION="us-east-1"
export AGENTCORE_PROFILE_ID="my-profile"
```

## iOS Simulator Support

```bash
# Install Appium and XCUITest driver
npm install -g appium
appium driver install xcuitest

# List devices
agent-browser device list

# Launch Safari on iOS Simulator
agent-browser -p ios --device "iPhone 16 Pro" open https://example.com

# Same commands as desktop
agent-browser -p ios snapshot -i
agent-browser -p ios tap @e1
agent-browser -p ios swipe up

# Or via environment
export AGENT_BROWSER_PROVIDER=ios
export AGENT_BROWSER_IOS_DEVICE="iPhone 16 Pro"
agent-browser open https://example.com
```

## Security Features

### Domain Allowlist

```bash
# Restrict navigation to trusted domains
agent-browser --allowed-domains "example.com,*.example.com" open example.com

# Also disables WebRTC to prevent bypass
```

### Content Boundaries

```bash
# Wrap page output in delimiters for LLM safety
agent-browser --content-boundaries snapshot
```

### Action Confirmation

```bash
# Require approval for sensitive actions
agent-browser --confirm-actions eval,download --confirm-interactive eval "alert(1)"
```

### Output Limits

```bash
# Prevent context flooding
agent-browser --max-output 50000 snapshot
```

### Plugin System

```bash
# Add plugins
agent-browser plugin add agent-browser-plugin-captcha
agent-browser plugin add @company/vault --name vault

# List plugins
agent-browser plugin list

# Use credential provider
agent-browser auth login my-app --credential-provider vault --item "My App"
```

## Serverless Deployment

### Vercel Sandbox

```typescript
import { runAgentBrowserCommand, withAgentBrowserSandbox } from "@agent-browser/sandbox/vercel";

const result = await withAgentBrowserSandbox(async (sandbox) => {
  await runAgentBrowserCommand(sandbox, ["open", "https://example.com"]);
  return runAgentBrowserCommand(sandbox, ["screenshot"]);
});
```

### eve Extension

```typescript
// agent/extensions/browser.ts
import browser from "@agent-browser/eve";

export default browser({});
```

### AWS Lambda

```typescript
import chromium from '@sparticuz/chromium';
import { execSync } from 'child_process';

export async function handler() {
  const executablePath = await chromium.executablePath();
  const result = execSync(
    `AGENT_BROWSER_EXECUTABLE_PATH=${executablePath} agent-browser open https://example.com && agent-browser snapshot -i --json`,
    { encoding: 'utf-8' }
  );
  return JSON.parse(result);
}
```

## Configuration

Create `agent-browser.json` for persistent defaults:

```json
{
  "headed": true,
  "proxy": "http://localhost:8080",
  "profile": "./browser-data",
  "userAgent": "my-agent/1.0",
  "hideScrollbars": false,
  "ignoreHttpsErrors": true,
  "caCert": "/etc/ssl/certs/proxy-ca.crt",
  "plugins": [
    {
      "name": "vault",
      "command": "agent-browser-plugin-vault",
      "capabilities": ["credential.read"]
    }
  ]
}
```

**Locations (lowest to highest priority):**
1. `~/.agent-browser/config.json` (user-level)
2. `./agent-browser.json` (project-level)
3. `AGENT_BROWSER_*` environment variables
4. CLI flags

## Common Commands

```bash
# Navigation
agent-browser open <url>
agent-browser back
agent-browser forward
agent-browser reload
agent-browser close

# Interaction
agent-browser click <selector>
agent-browser fill <selector> <text>
agent-browser type <selector> <text>
agent-browser press <key>
agent-browser hover <selector>
agent-browser check <selector>
agent-browser scroll <direction>

# Information
agent-browser get text <selector>
agent-browser get html <selector>
agent-browser get url
agent-browser get title
agent-browser is visible <selector>

# Waiting
agent-browser wait <selector>
agent-browser wait <ms>
agent-browser wait --text "Welcome"
agent-browser wait --url "**/dashboard"
agent-browser wait --load networkidle

# Find elements (semantic)
agent-browser find role button click --name "Submit"
agent-browser find label "Email" fill "test@test.com"
agent-browser find text "Sign In" click

# Tabs
agent-browser tab                     # List tabs
agent-browser tab new [url]           # New tab
agent-browser tab new --label docs    # New tab with label
agent-browser tab <t1|label>          # Switch tab
agent-browser tab close [t1|label]    # Close tab

# Debug
agent-browser console                 # View console messages
agent-browser errors                  # View page errors
agent-browser inspect                 # Open DevTools
agent-browser highlight <selector>    # Highlight element

# Streaming
agent-browser stream status           # Show streaming state
agent-browser stream enable --port 9223
agent-browser stream disable

# Dashboard
agent-browser dashboard start         # Start on port 4848
agent-browser dashboard start --port 8080
agent-browser dashboard stop

# AI Chat
agent-browser chat "open google.com and search for cats"
agent-browser chat                    # Interactive REPL
agent-browser -q chat "summarize"     # Quiet mode
agent-browser -v chat "login"         # Verbose mode
```

## Environment Variables

Key environment variables:

```bash
# Session & State
AGENT_BROWSER_SESSION=<name>
AGENT_BROWSER_RESTORE=<name>
AGENT_BROWSER_PROFILE=<path>
AGENT_BROWSER_STATE=<path>
AGENT_BROWSER_ENCRYPTION_KEY=<64-char-hex>

# Browser
AGENT_BROWSER_EXECUTABLE_PATH=<path>
AGENT_BROWSER_HEADED=true
AGENT_BROWSER_WEBGPU=true
AGENT_BROWSER_PROVIDER=<chrome|ios|browserbase|kernel|etc>

# Security
AGENT_BROWSER_ALLOWED_DOMAINS=<comma-list>
AGENT_BROWSER_CONTENT_BOUNDARIES=true
AGENT_BROWSER_MAX_OUTPUT=<chars>

# Network
AGENT_BROWSER_PROXY=<url>
AGENT_BROWSER_CA_CERT=<path>

# Streaming
AGENT_BROWSER_STREAM_PORT=<port>
AGENT_BROWSER_STREAM_QUALITY=<0-100>

# AI Chat
AI_GATEWAY_API_KEY=<key>
AI_GATEWAY_MODEL=<model>
```

## Performance Characteristics

**Daemon Architecture:**
- Rust CLI + Rust daemon (no Node.js runtime required)
- Daemon starts automatically and persists between commands
- Fast subsequent operations (no browser startup overhead)
- Auto-shutdown after 1 hour idle (configurable)

**Context Efficiency:**
- Reduces context usage by up to 93% vs full DOM
- AI-friendly refs eliminate verbose CSS selectors
- Compact snapshot format for LLMs

**Speeds:**
- First command: ~1-2s (daemon + browser startup)
- Subsequent commands: 50-200ms (daemon already running)

## Observability Dashboard

```bash
# Start dashboard (runs on port 4848)
agent-browser dashboard start

# Open http://localhost:4848
# Features:
# - Live viewport (real-time JPEG frames)
# - Activity feed (command/result stream)
# - Console output
# - Session creation UI
# - AI Chat panel (requires AI_GATEWAY_API_KEY)

# Stop dashboard
agent-browser dashboard stop
```

## Skills Integration

```bash
# Add to AI coding assistants
npx skills add vercel-labs/agent-browser

# Get skill content
agent-browser skills list
agent-browser skills get core
agent-browser skills get core --full
```

## Troubleshooting

### Installation Issues

```bash
# Diagnose environment
agent-browser doctor
agent-browser doctor --fix

# Install system dependencies (Linux)
agent-browser install --with-deps

# Upgrade to latest
agent-browser upgrade
```

### Ref Not Working After Page Change

```bash
# Always take fresh snapshot after navigation or interaction that changes DOM
agent-browser click @e1
agent-browser snapshot -i  # Get new refs
agent-browser click @e2    # Use new refs
```

### Click Blocked by Overlay

```bash
# Error will show covering element
# Example: "covered by <div#consent-banner>"

# Solution: Click the blocking element first
agent-browser click "#consent-banner button"
agent-browser snapshot -i  # Get fresh refs
agent-browser click @e2    # Retry original click
```

### Idle Timeout Too Short

```bash
# Increase timeout
agent-browser --idle-timeout 5m open example.com

# Or disable
agent-browser --idle-timeout 0 open example.com

# Or via environment
export AGENT_BROWSER_IDLE_TIMEOUT_MS=300000  # 5 minutes
```

## Related Tools

- **Playwright** - Full-featured browser automation for testing (more verbose for agents)
- **Puppeteer** - Chrome DevTools Protocol automation (more boilerplate)
- **Selenium** - Cross-browser WebDriver automation (slower startup)
- **Browserbase/Browserless** - Cloud browser services (integrates with agent-browser)

## Reference

- [GitHub Repository](https://github.com/vercel-labs/agent-browser)
- [Official Documentation](https://agent-browser.dev/)
- [Skills.sh Page](https://skills.sh/vercel-labs/agent-browser)
- [WebGPU Guide](https://agent-browser.dev/webgpu)
- [Security Documentation](https://agent-browser.dev/security)
- [Authentication Guide](https://github.com/vercel-labs/agent-browser/blob/main/docs/src/app/sessions/page.mdx)

## Lessons Learned

- Always take fresh snapshots after DOM-changing interactions
- Use refs (@e1, @e2) instead of CSS selectors for AI workflows
- Session persistence with `--restore` is essential for auth workflows
- Batch execution reduces overhead for multi-step workflows
- Domain allowlists prevent agent misbehavior and data exfiltration
- Cloud providers (Browserbase, Kernel) solve serverless deployment challenges
- MCP integration makes agent-browser a first-class citizen in MCP clients
- Annotated screenshots bridge visual and text-based automation
- iOS provider enables true mobile Safari testing in CI
- Dashboard provides invaluable debugging visibility for agent development

## Status

Status: production-ready
Version: v0.34.0 (active development)
Stars: 41.2k on GitHub
License: Apache-2.0
