# Firecrawl

## Summary

Firecrawl is an API-first web context platform designed specifically for AI agents and LLMs. It transforms any web page into clean, LLM-ready markdown or structured JSON, handling JavaScript-heavy pages, rate limiting, and anti-bot measures automatically. With features like Search, Scrape, Crawl, Map, Batch Scrape, and Interact modes, Firecrawl provides a comprehensive solution for feeding web data into AI applications. Built for speed and simplicity with P95 latency of 3.4 seconds and coverage of 96% of the web.

## Best For

- AI agent web data retrieval with LLM-ready markdown output
- RAG (Retrieval-Augmented Generation) pipelines needing web context
- Autonomous research agents requiring search + scrape capabilities
- Web scraping with JavaScript rendering and anti-bot bypass
- Documentation crawling and knowledge base extraction
- Batch web page processing for AI training data
- Screenshot capture with structured data extraction
- Browser automation via interact mode for dynamic content
- MCP (Model Context Protocol) integration for AI assistants
- Skills.sh integration for reusable agent capabilities

## Avoid When

- You need pre-built scrapers for specific platforms (use Apify instead)
- You need long-running scheduled jobs or complex workflows (use Apify instead)
- You want a visual scraping IDE or no-code tool
- You need advanced proxy rotation control (Firecrawl handles this automatically)
- You're building a scraping marketplace or monetization platform
- You need persistent browser sessions across multiple operations
- You need to scrape behind authentication walls (limited support)

## Setup

### API-First Approach (Recommended)

```bash
# Get API key from https://firecrawl.dev
export FIRECRAWL_API_KEY='fc-your-api-key'

# Or use cloud service (no setup needed)
# Sign up at https://firecrawl.dev
```

### Self-Hosted Installation

```bash
# Clone repository
git clone https://github.com/mendableai/firecrawl
cd firecrawl

# Install with Docker Compose
docker-compose up -d

# Or install with Node.js
npm install -g firecrawl-cli
```

### SDK Installation

**Python:**

```bash
pip install firecrawl-py
```

**Node.js/TypeScript:**

```bash
npm install @mendable/firecrawl-js
```

**Go:**

```bash
go get github.com/mendable/firecrawl-go
```

**Additional SDKs:**
- Java: `com.mendable:firecrawl-java`
- Rust: `firecrawl = "0.1"`
- Ruby: `gem install firecrawl`
- .NET: `dotnet add package Firecrawl`
- PHP: `composer require mendable/firecrawl-php`
- Elixir: `{:firecrawl, "~> 0.1"}`

## Core Workflow

### Basic Scrape (Single Page)

**API (cURL):**

```bash
curl -X POST https://api.firecrawl.dev/v1/scrape \
  -H "Authorization: Bearer $FIRECRAWL_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com",
    "formats": ["markdown", "html"]
  }'
```

**Python:**

```python
from firecrawl import FirecrawlApp

app = FirecrawlApp(api_key='fc-your-api-key')

# Simple scrape
result = app.scrape_url('https://example.com')
print(result['markdown'])

# With options
result = app.scrape_url(
    'https://docs.example.com',
    params={
        'formats': ['markdown', 'html', 'screenshot'],
        'onlyMainContent': True,
        'includeTags': ['article', 'main'],
        'excludeTags': ['nav', 'footer'],
        'waitFor': 2000  # Wait 2 seconds for JS
    }
)
```

**Node.js:**

```typescript
import FirecrawlApp from '@mendable/firecrawl-js';

const app = new FirecrawlApp({ apiKey: 'fc-your-api-key' });

// Simple scrape
const result = await app.scrapeUrl('https://example.com');
console.log(result.markdown);

// With screenshots
const result = await app.scrapeUrl('https://example.com', {
  formats: ['markdown', 'screenshot'],
  screenshot: {
    fullPage: true
  }
});
```

### Crawl (Multi-Page Recursive)

**Python:**

```python
# Async crawl (returns immediately with job ID)
crawl_result = app.crawl_url(
    'https://docs.example.com',
    params={
        'limit': 100,  # Max pages
        'scrapeOptions': {
            'formats': ['markdown'],
            'onlyMainContent': True
        },
        'excludePaths': ['*/api-reference/*'],
        'includePaths': ['*/guides/*', '*/tutorials/*'],
        'maxDepth': 3,  # Depth limit
        'allowBackwardLinks': False,
        'allowExternalLinks': False
    },
    poll_interval=5  # Check status every 5 seconds
)

# Get results
print(f"Status: {crawl_result['status']}")
for page in crawl_result['data']:
    print(f"URL: {page['url']}")
    print(f"Content: {page['markdown'][:200]}...")

# Check specific job status
status = app.check_crawl_status(crawl_result['id'])
```

**API:**

```bash
# Start crawl
curl -X POST https://api.firecrawl.dev/v1/crawl \
  -H "Authorization: Bearer $FIRECRAWL_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://docs.example.com",
    "limit": 100,
    "scrapeOptions": {
      "formats": ["markdown"]
    }
  }'

# Response: {"id": "crawl-job-123", "status": "scraping"}

# Check status
curl https://api.firecrawl.dev/v1/crawl/crawl-job-123 \
  -H "Authorization: Bearer $FIRECRAWL_API_KEY"
```

### Map (Get Sitemap Structure)

**Python:**

```python
# Get all URLs from a site without scraping content
map_result = app.map_url(
    'https://example.com',
    params={
        'search': 'documentation',  # Filter URLs
        'limit': 500,
        'ignoreSitemap': False  # Use sitemap if available
    }
)

print(f"Found {len(map_result['links'])} URLs")
for link in map_result['links']:
    print(link)
```

**Use Case:** Fast site structure discovery before targeted crawling.

### Search (Google Search + Scrape)

**Python:**

```python
# Search the web and scrape results
search_result = app.search(
    'latest AI agent frameworks 2026',
    params={
        'limit': 5,  # Number of results
        'scrapeOptions': {
            'formats': ['markdown']
        },
        'lang': 'en',
        'country': 'us'
    }
)

for result in search_result['data']:
    print(f"Title: {result['title']}")
    print(f"URL: {result['url']}")
    print(f"Content: {result['markdown'][:300]}...")
```

**Node.js:**

```typescript
const searchResults = await app.search('AI web scraping tools', {
  limit: 10,
  scrapeOptions: {
    formats: ['markdown'],
    onlyMainContent: true
  }
});
```

### Batch Scrape (Parallel Processing)

**Python:**

```python
# Scrape multiple URLs in parallel
urls = [
    'https://example.com/page1',
    'https://example.com/page2',
    'https://example.com/page3'
]

batch_result = app.batch_scrape_urls(
    urls,
    params={
        'formats': ['markdown', 'html'],
        'onlyMainContent': True
    },
    poll_interval=5
)

for page in batch_result['data']:
    print(f"URL: {page['url']}")
    print(f"Status: {page['statusCode']}")
    print(f"Content length: {len(page['markdown'])}")
```

**API:**

```bash
curl -X POST https://api.firecrawl.dev/v1/batch/scrape \
  -H "Authorization: Bearer $FIRECRAWL_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "urls": [
      "https://example.com/page1",
      "https://example.com/page2"
    ],
    "formats": ["markdown"]
  }'
```

### Interact (Browser Automation)

**Python:**

```python
# Control browser with actions
result = app.scrape_url(
    'https://example.com/search',
    params={
        'formats': ['markdown'],
        'actions': [
            {'type': 'wait', 'milliseconds': 2000},
            {'type': 'click', 'selector': '#search-button'},
            {'type': 'wait', 'milliseconds': 1000},
            {'type': 'write', 'text': 'AI agents', 'selector': '#search-input'},
            {'type': 'press', 'key': 'Enter'},
            {'type': 'wait', 'milliseconds': 3000},
            {'type': 'scroll', 'direction': 'down'},
            {'type': 'screenshot'}
        ]
    }
)
```

**Available Actions:**
- `wait` - Wait for milliseconds or selector
- `click` - Click element
- `write` - Type text into input
- `press` - Press keyboard key
- `scroll` - Scroll direction
- `screenshot` - Capture screenshot

### LLM Extract (Structured Data)

**Python:**

```python
# Extract structured data using LLM
from pydantic import BaseModel

class Product(BaseModel):
    name: str
    price: float
    in_stock: bool
    description: str

result = app.scrape_url(
    'https://store.example.com/product',
    params={
        'formats': ['extract'],
        'extract': {
            'schema': Product.schema(),
            'systemPrompt': 'Extract product information',
            'prompt': 'Get the product name, price, availability and description'
        }
    }
)

product = Product(**result['extract'])
print(f"{product.name}: ${product.price}")
```

**Node.js:**

```typescript
interface Article {
  title: string;
  author: string;
  publishDate: string;
  summary: string;
  tags: string[];
}

const result = await app.scrapeUrl('https://blog.example.com/post', {
  formats: ['extract'],
  extract: {
    schema: {
      type: 'object',
      properties: {
        title: { type: 'string' },
        author: { type: 'string' },
        publishDate: { type: 'string' },
        summary: { type: 'string' },
        tags: { type: 'array', items: { type: 'string' } }
      },
      required: ['title', 'author']
    }
  }
});

const article: Article = result.extract;
```

## Advanced Features

### Screenshot Capture

```python
# Full page screenshot
result = app.scrape_url(
    'https://example.com',
    params={
        'formats': ['screenshot', 'markdown'],
        'screenshot': {
            'fullPage': True
        }
    }
)

# Save screenshot (base64 encoded)
import base64
screenshot_data = base64.b64decode(result['screenshot'])
with open('page.png', 'wb') as f:
    f.write(screenshot_data)
```

### Content Filtering

```python
# Only scrape main content, exclude navigation/footer
result = app.scrape_url(
    'https://blog.example.com/article',
    params={
        'formats': ['markdown'],
        'onlyMainContent': True,  # Smart main content detection
        'includeTags': ['article', 'main', 'section'],
        'excludeTags': ['nav', 'footer', 'aside', 'header'],
        'removeBase64Images': True  # Exclude images from markdown
    }
)
```

### Custom Headers and Cookies

```python
# Authentication or custom headers
result = app.scrape_url(
    'https://api.example.com/data',
    params={
        'formats': ['markdown'],
        'headers': {
            'Authorization': 'Bearer token123',
            'User-Agent': 'MyAgent/1.0'
        }
    }
)
```

### Webhook Notifications

```python
# Get notified when crawl completes
crawl_result = app.crawl_url(
    'https://docs.example.com',
    params={
        'limit': 100,
        'webhook': 'https://your-server.com/webhook'
    }
)

# Your webhook receives:
# {
#   "id": "crawl-123",
#   "status": "completed",
#   "data": [...],
#   "completed_at": "2026-08-23T10:30:00Z"
# }
```

### Rate Limiting

```python
# Automatic rate limiting and retry
crawl_result = app.crawl_url(
    'https://example.com',
    params={
        'limit': 1000,
        'scrapeOptions': {
            'formats': ['markdown']
        }
    }
)

# Firecrawl handles:
# - Automatic backoff on rate limits
# - Retry on transient errors
# - Distributed crawling for speed
```

## MCP Server Integration

Firecrawl provides an MCP server for AI assistants like Claude Desktop.

### Setup

**Install:**

```bash
npx @mendable/firecrawl-mcp init
```

**Configure (Claude Desktop):**

Edit `~/Library/Application Support/Claude/claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "firecrawl": {
      "command": "npx",
      "args": ["-y", "@mendable/firecrawl-mcp"],
      "env": {
        "FIRECRAWL_API_KEY": "fc-your-api-key"
      }
    }
  }
}
```

**Available MCP Tools:**
- `firecrawl_scrape` - Scrape single URL
- `firecrawl_crawl` - Recursive crawl
- `firecrawl_map` - Get site structure
- `firecrawl_search` - Search and scrape

**Usage in AI Chat:**

```
User: "Scrape the latest blog posts from example.com and summarize them"
Assistant: [uses firecrawl_crawl to get posts, then summarizes]

User: "Search for information about AI agents and extract key features"
Assistant: [uses firecrawl_search to find and scrape relevant pages]
```

## Skills.sh Integration

```bash
# Add Firecrawl skill to AI coding assistant
npx skills add mendable/firecrawl

# Available skills:
# - firecrawl-scrape
# - firecrawl-crawl
# - firecrawl-search
# - firecrawl-extract

# List skills
npx skills list | grep firecrawl

# Get skill documentation
npx skills get mendable/firecrawl-scrape --full
```

## LLMs.txt Support

Firecrawl can discover and parse `llms.txt` and `llms-full.txt` files for AI-optimized documentation.

```python
# Check if site has llms.txt
result = app.scrape_url(
    'https://docs.example.com/llms.txt',
    params={'formats': ['markdown']}
)

# Or auto-discover via map
map_result = app.map_url('https://docs.example.com')
llms_files = [url for url in map_result['links'] if 'llms' in url]
```

## Performance Characteristics

**Speed:**
- P95 latency: 3.4 seconds per page
- Batch scraping: Up to 100 URLs in parallel
- Crawl speed: 5-10 pages/second depending on site

**Coverage:**
- 96% of web including JavaScript-heavy pages
- Automatic JavaScript rendering
- Handles React, Vue, Angular, Next.js apps
- Bypasses common anti-bot measures

**Output Quality:**
- Clean markdown without HTML artifacts
- Main content extraction removes boilerplate
- Preserved formatting (headers, lists, links, code blocks)
- Metadata extraction (title, description, author, publish date)

**Limits (Cloud Plans):**
- Free: 500 credits (~500 pages)
- Starter: $20/month, 5,000 credits
- Standard: $100/month, 50,000 credits
- Scale: $400/month, 500,000 credits
- Enterprise: Custom pricing

## Common Commands

### CLI (Self-Hosted)

```bash
# Scrape single page
firecrawl scrape https://example.com

# Crawl entire site
firecrawl crawl https://docs.example.com --limit 100

# Map site structure
firecrawl map https://example.com

# Search and scrape
firecrawl search "AI frameworks" --limit 5

# With output format
firecrawl scrape https://example.com --format markdown --output page.md

# Batch scrape from file
firecrawl batch urls.txt --output results/

# With options
firecrawl crawl https://example.com \
  --limit 50 \
  --exclude-paths "*/blog/*" \
  --only-main-content \
  --wait-for 2000
```

## API Reference

**Base URL:** `https://api.firecrawl.dev/v1`

**Authentication:** Bearer token in `Authorization` header

**Endpoints:**

- `POST /scrape` - Scrape single URL
- `POST /crawl` - Start crawl job
- `GET /crawl/:id` - Check crawl status
- `POST /map` - Get site structure
- `POST /search` - Search and scrape
- `POST /batch/scrape` - Batch scrape URLs

**Rate Limits:**
- Cloud: Based on plan (5-100 req/sec)
- Self-hosted: Unlimited

## Troubleshooting

### JavaScript Not Rendering

```python
# Increase wait time
result = app.scrape_url(
    'https://spa.example.com',
    params={
        'formats': ['markdown'],
        'waitFor': 5000,  # Wait 5 seconds
        'actions': [
            {'type': 'wait', 'selector': '#content-loaded'}
        ]
    }
)
```

### Anti-Bot Blocking

```python
# Firecrawl handles this automatically with:
# - Rotating IPs
# - Browser fingerprint randomization
# - Human-like behavior patterns

# If still blocked, try:
result = app.scrape_url(
    'https://protected.example.com',
    params={
        'formats': ['markdown'],
        'waitFor': 3000,  # Give time for challenges
        'actions': [
            {'type': 'wait', 'milliseconds': 5000}  # Extra wait
        ]
    }
)
```

### Crawl Taking Too Long

```python
# Limit depth and pages
crawl_result = app.crawl_url(
    'https://large-site.com',
    params={
        'limit': 50,  # Stop at 50 pages
        'maxDepth': 2,  # Only 2 levels deep
        'allowBackwardLinks': False,  # Don't revisit parent pages
        'allowExternalLinks': False,  # Stay on same domain
        'includePaths': ['*/docs/*']  # Only specific paths
    }
)
```

### Rate Limit Errors

```python
# Automatic retry with exponential backoff
import time

def scrape_with_retry(url, max_retries=3):
    for attempt in range(max_retries):
        try:
            return app.scrape_url(url)
        except Exception as e:
            if 'rate limit' in str(e).lower() and attempt < max_retries - 1:
                wait_time = 2 ** attempt  # Exponential backoff
                print(f"Rate limited, waiting {wait_time}s...")
                time.sleep(wait_time)
            else:
                raise
```

### Empty or Incomplete Content

```python
# Disable main content filtering
result = app.scrape_url(
    'https://example.com',
    params={
        'formats': ['markdown', 'html'],  # Get both
        'onlyMainContent': False,  # Include everything
        'removeBase64Images': False  # Keep images
    }
)

# Or increase specificity
result = app.scrape_url(
    'https://example.com',
    params={
        'formats': ['markdown'],
        'includeTags': ['article', 'main', 'div.content', 'section.post']
    }
)
```

## Comparison: Firecrawl vs Apify

| Feature | Firecrawl | Apify |
|---------|-----------|-------|
| **Primary Use Case** | AI/LLM web data ingestion | General web scraping platform |
| **Output Format** | LLM-ready markdown/JSON | Raw data, multiple formats |
| **API-First** | ✓ Designed for APIs | ✓ API + visual console |
| **Pre-Built Scrapers** | ✗ Generic scraping only | ✓ 53,000+ platform-specific Actors |
| **Speed** | Fast (P95: 3.4s) | Varies by Actor |
| **MCP Integration** | ✓ Official MCP server | ✓ Via community |
| **Skills.sh** | ✓ Official skills | ✓ Via community |
| **Autonomous Agent Mode** | ✓ Search + scrape | ✗ Requires orchestration |
| **Marketplace** | ✗ No marketplace | ✓ Actor marketplace with monetization |
| **Scheduling** | ✗ Use external scheduler | ✓ Built-in cron scheduling |
| **Storage** | ✗ Returns data directly | ✓ Datasets, key-value stores |
| **Proxy Management** | ✓ Automatic | ✓ Apify Proxy with residential IPs |
| **Learning Curve** | Low (simple API) | Medium-High (complex ecosystem) |
| **Pricing Model** | Credit-based per page | Compute time + data transfer |
| **Best For** | AI agents, RAG, research | Custom scrapers, platform-specific, complex workflows |

**Choose Firecrawl when:**
- Building AI agents that need web data
- Need LLM-ready markdown output
- Want simple API without infrastructure
- Doing autonomous research (search + scrape)
- Need fast setup and deployment

**Choose Apify when:**
- Need pre-built Instagram/TikTok/LinkedIn scrapers
- Building custom scrapers for complex sites
- Need scheduled recurring jobs
- Want to monetize your scrapers
- Need persistent storage and long-running jobs

## Related Tools

- **Apify** - Full-stack scraping platform with 53,000+ Actors for specific sites
- **Crawl4AI** - Open-source async web crawler optimized for LLMs
- **Playwright** - Browser automation framework (lower-level control)
- **Agent Browser** - Rust CLI for AI agent browser automation with MCP
- **Jina Reader** - URL-to-markdown API service
- **Browserless** - Headless browser hosting platform

## Reference

- [Official Website](https://firecrawl.dev/)
- [GitHub Repository](https://github.com/mendableai/firecrawl) - 171.1k stars
- [API Documentation](https://docs.firecrawl.dev/api-reference)
- [Python SDK](https://github.com/mendableai/firecrawl-py)
- [JavaScript SDK](https://github.com/mendableai/firecrawl-js)
- [MCP Server](https://github.com/mendableai/firecrawl-mcp)
- [Skills.sh Integration](https://skills.sh/mendable/firecrawl)
- [Discord Community](https://discord.gg/firecrawl)

## Lessons Learned

- LLM-ready markdown output saves significant post-processing time
- Automatic JavaScript rendering covers 96% of modern web apps
- Search + scrape in one call enables autonomous research agents
- Main content extraction is highly accurate for documentation and articles
- MCP integration makes Firecrawl a first-class AI assistant tool
- Batch scraping is essential for training data collection
- Map endpoint enables intelligent crawl planning
- Interact mode bridges gap between static scraping and full browser automation
- Webhook notifications critical for long-running crawls
- Credit-based pricing is more predictable than compute-time pricing
- Self-hosted option provides unlimited scraping for high-volume needs
- Skills.sh integration extends capabilities to coding assistants

## Status

Status: production-ready
Version: v1.0 (API), v0.1 (MCP Server)
Stars: 171.1k on GitHub
License: AGPL-3.0 (open-source) + Hosted service available
Last Updated: August 2026
