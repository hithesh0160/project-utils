# Apify

## Summary

Apify is a full-stack web scraping and automation platform built around Actors—serverless microservices packaged as Docker containers. With 53,000+ pre-built Actors for scraping Instagram, TikTok, Google Maps, LinkedIn, Amazon, and virtually any website, Apify provides ready-made solutions alongside infrastructure for building custom scrapers. The platform includes compute resources, storage (datasets, key-value stores, request queues), proxy management, scheduling, webhooks, and a marketplace where developers can publish and monetize their Actors. SDKs for Python and JavaScript streamline Actor development with built-in lifecycle management and pay-per-event capabilities.

## Best For

- Platform-specific scraping (Instagram, TikTok, LinkedIn, Facebook, Twitter/X, YouTube, Google Maps, Amazon, eBay, Airbnb, Yelp, Reddit)
- Building and deploying custom web scrapers as serverless Actors
- Large-scale production scraping with scheduling and monitoring
- Complex multi-step automation workflows
- Monetizing web scraping tools in the Apify Store marketplace
- Long-running crawls with persistent storage
- Browser automation for testing and data extraction
- Residential and datacenter proxy management
- Team collaboration on scraping projects
- CI/CD integration for Actor deployment

## Avoid When

- You only need simple URL-to-markdown conversion (use Firecrawl instead)
- You want instant API responses without job management (use Firecrawl for synchronous scraping)
- Building AI agents that need LLM-optimized output (Firecrawl is more AI-native)
- You need sub-second scraping latency (Apify uses queued jobs)
- You're non-technical and want no-code visual scraping (try Octoparse or ParseHub)
- You need real-time streaming data extraction
- Budget is extremely limited (Firecrawl may be cheaper for small volumes)

## Setup

### Cloud Platform (Recommended)

```bash
# Sign up at https://apify.com (free tier available)
# Get API token from https://console.apify.com/account/integrations

export APIFY_TOKEN='apify_api_your_token'
```

### Python SDK

```bash
pip install apify apify-client
```

**Basic Usage:**

```python
from apify_client import ApifyClient

client = ApifyClient('apify_api_your_token')

# Run an Actor
run = client.actor('apify/web-scraper').call(
    run_input={'startUrls': [{'url': 'https://example.com'}]}
)

# Get results
for item in client.dataset(run['defaultDatasetId']).iterate_items():
    print(item)
```

### JavaScript/TypeScript SDK

```bash
npm install apify apify-client
```

**Basic Usage:**

```typescript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: 'apify_api_your_token' });

// Run an Actor
const run = await client.actor('apify/web-scraper').call({
  startUrls: [{ url: 'https://example.com' }]
});

// Get results
const { items } = await client.dataset(run.defaultDatasetId).listItems();
items.forEach(item => console.log(item));
```

### Apify CLI

```bash
npm install -g apify-cli

# Login
apify login

# Create new Actor
apify create my-scraper
cd my-scraper

# Run locally
apify run

# Deploy to Apify platform
apify push
```

## Core Workflow

### Run Pre-Built Actors (Store)

**Instagram Scraper:**

```python
from apify_client import ApifyClient

client = ApifyClient('apify_api_your_token')

# Scrape Instagram profiles, posts, hashtags
run = client.actor('apify/instagram-scraper').call(run_input={
    'username': ['elonmusk', 'nasa'],
    'resultsLimit': 50
})

# Get results
dataset = client.dataset(run['defaultDatasetId'])
for post in dataset.iterate_items():
    print(f"Post: {post['caption']}")
    print(f"Likes: {post['likesCount']}")
    print(f"URL: {post['url']}")
```

**Google Maps Scraper:**

```python
# Extract businesses, reviews, contact info
run = client.actor('compass/crawler-google-places').call(run_input={
    'searchStringsArray': ['restaurants in San Francisco'],
    'maxCrawledPlaces': 100,
    'language': 'en',
    'includeReviews': True
})

for place in client.dataset(run['defaultDatasetId']).iterate_items():
    print(f"{place['title']} - {place['rating']} stars")
    print(f"Address: {place['address']}")
    print(f"Phone: {place['phone']}")
```

**Amazon Product Scraper:**

```python
# Scrape products, prices, reviews
run = client.actor('junglee/amazon-crawler').call(run_input={
    'startUrls': ['https://www.amazon.com/s?k=laptop'],
    'maxItems': 50,
    'proxy': {'useApifyProxy': True}
})

for product in client.dataset(run['defaultDatasetId']).iterate_items():
    print(f"{product['title']}: ${product['price']}")
```

**LinkedIn Scraper:**

```python
# Extract profiles, companies, jobs
run = client.actor('apify/linkedin-profile-scraper').call(run_input={
    'profileUrls': [
        'https://www.linkedin.com/in/example-profile'
    ]
})
```

**Twitter/X Scraper:**

```python
run = client.actor('apify/twitter-scraper').call(run_input={
    'searchTerms': ['#AI agents'],
    'maxTweets': 100,
    'tweetLanguage': 'en'
})
```

### Universal Web Scraper Actor

```python
# Generic scraper for any website
run = client.actor('apify/web-scraper').call(run_input={
    'startUrls': [
        {'url': 'https://news.ycombinator.com'}
    ],
    'pageFunction': '''
        async function pageFunction(context) {
            const { request, log, jQuery: $ } = context;
            
            const title = $('title').text();
            const stories = [];
            
            $('.athing').each((i, el) => {
                const story = {
                    title: $(el).find('.titleline a').text(),
                    url: $(el).find('.titleline a').attr('href'),
                    score: $(el).next().find('.score').text()
                };
                stories.push(story);
            });
            
            return { title, stories };
        }
    ''',
    'proxyConfiguration': {'useApifyProxy': True}
})
```

### Crawling Websites

**Python SDK (for building Actors):**

```python
from apify import Actor
from crawlee.playwright_crawler import PlaywrightCrawler

async def main():
    async with Actor:
        # Get input
        actor_input = await Actor.get_input() or {}
        start_urls = actor_input.get('startUrls', [])
        
        # Create crawler
        crawler = PlaywrightCrawler(
            max_requests_per_crawl=100,
            request_handler=request_handler
        )
        
        # Add URLs
        await crawler.add_requests(start_urls)
        
        # Run crawler
        await crawler.run()

async def request_handler(context):
    page = context.page
    
    # Extract data
    data = await page.evaluate('''() => {
        return {
            title: document.title,
            text: document.body.innerText.slice(0, 1000)
        };
    }''')
    
    # Save to dataset
    await context.push_data(data)
```

**JavaScript SDK:**

```typescript
import { Actor } from 'apify';
import { PlaywrightCrawler } from 'crawlee';

await Actor.init();

const crawler = new PlaywrightCrawler({
    requestHandler: async ({ page, request, enqueueLinks }) => {
        const title = await page.title();
        
        // Extract data
        const data = await page.evaluate(() => {
            return {
                url: window.location.href,
                title: document.title,
                headings: Array.from(document.querySelectorAll('h1, h2'))
                    .map(h => h.textContent)
            };
        });
        
        // Save to dataset
        await Actor.pushData(data);
        
        // Enqueue links
        await enqueueLinks({
            globs: ['https://example.com/blog/**'],
        });
    },
});

const startUrls = await Actor.getInput().startUrls;
await crawler.run(startUrls);

await Actor.exit();
```

## Building Custom Actors

### Actor Structure

```
my-actor/
├── .actor/
│   └── actor.json          # Actor metadata
├── .dockerignore
├── Dockerfile              # Optional custom Docker
├── requirements.txt        # Python deps
├── src/
│   └── main.py            # Entry point
└── INPUT_SCHEMA.json      # Input validation
```

### Simple Actor Example (Python)

**src/main.py:**

```python
from apify import Actor
from bs4 import BeautifulSoup
import httpx

async def main():
    async with Actor:
        # Get input
        actor_input = await Actor.get_input() or {}
        url = actor_input.get('url')
        
        Actor.log.info(f'Scraping {url}...')
        
        # Fetch page
        async with httpx.AsyncClient() as client:
            response = await client.get(url)
        
        # Parse HTML
        soup = BeautifulSoup(response.text, 'html.parser')
        
        # Extract data
        data = {
            'url': url,
            'title': soup.find('title').get_text() if soup.find('title') else None,
            'headings': [h.get_text() for h in soup.find_all(['h1', 'h2', 'h3'])],
            'links': [a.get('href') for a in soup.find_all('a', href=True)][:10]
        }
        
        # Save to dataset
        await Actor.push_data(data)
        
        Actor.log.info('Done!')
```

**.actor/actor.json:**

```json
{
    "actorSpecification": 1,
    "name": "my-web-scraper",
    "title": "My Web Scraper",
    "description": "Scrapes a URL and extracts basic information",
    "version": "1.0",
    "dockerfile": "./Dockerfile",
    "readme": "./README.md",
    "input": "./INPUT_SCHEMA.json",
    "storages": {
        "dataset": {
            "actorSpecification": 1,
            "views": {
                "overview": {
                    "title": "Overview",
                    "transformation": {
                        "fields": ["url", "title"]
                    }
                }
            }
        }
    }
}
```

**INPUT_SCHEMA.json:**

```json
{
    "title": "My Web Scraper Input",
    "type": "object",
    "schemaVersion": 1,
    "properties": {
        "url": {
            "title": "URL to scrape",
            "type": "string",
            "description": "The URL of the page to scrape",
            "editor": "textfield",
            "example": "https://example.com"
        }
    },
    "required": ["url"]
}
```

### Deploy Actor

```bash
# Login
apify login

# Push to Apify platform
apify push

# Or with GitHub integration
git push origin main
# Configure auto-build in Apify Console
```

## Storage

### Datasets (Structured Data)

```python
# Push data during Actor run
await Actor.push_data({'name': 'John', 'age': 30})

# Access after run
from apify_client import ApifyClient
client = ApifyClient('token')

dataset = client.dataset('dataset-id')

# Get all items
items = dataset.list_items().items
print(items)

# Download as CSV
dataset.download_items(item_format='csv', file='output.csv')

# Download as JSON
dataset.download_items(item_format='json', file='output.json')

# Iterate large datasets
for item in dataset.iterate_items():
    print(item)
```

### Key-Value Store (Files & Metadata)

```python
# Save during Actor run
await Actor.set_value('my-key', {'data': 'value'})
await Actor.set_value('screenshot', b'...', content_type='image/png')

# Read in Actor
value = await Actor.get_value('my-key')

# Access via client
from apify_client import ApifyClient
client = ApifyClient('token')

store = client.key_value_store('store-id')

# Get value
value = store.get_record('my-key').value

# Set value
store.set_record('my-key', {'data': 'new value'})
```

### Request Queue (Crawl Management)

```python
# Inside Actor
from apify import Actor

async with Actor:
    # Get or create queue
    queue = await Actor.open_request_queue()
    
    # Add requests
    await queue.add_request({'url': 'https://example.com'})
    await queue.add_request({
        'url': 'https://example.com/page2',
        'userData': {'depth': 2}
    })
    
    # Process requests
    while not queue.is_empty():
        request = await queue.fetch_next_request()
        if request:
            # Process request
            # ...
            await queue.mark_request_as_handled(request)
```

## Proxy Management

### Apify Proxy

```python
# In Actor configuration
run = client.actor('apify/web-scraper').call(run_input={
    'startUrls': [{'url': 'https://example.com'}],
    'proxyConfiguration': {
        'useApifyProxy': True,
        'apifyProxyGroups': ['RESIDENTIAL'],  # or SHADER, BUYPROXIES94952
        'apifyProxyCountry': 'US'
    }
})
```

**Proxy Types:**
- `SHADER` - Datacenter proxies (fast, cheaper)
- `RESIDENTIAL` - Residential IPs (slower, more expensive, better for anti-bot)
- `GOOGLE_SERP` - Optimized for Google Search

**In Python Actor:**

```python
from apify import Actor

async with Actor:
    proxy_config = await Actor.create_proxy_configuration()
    proxy_url = await proxy_config.new_url()
    
    # Use with httpx
    async with httpx.AsyncClient(proxies={'all://': proxy_url}) as client:
        response = await client.get('https://example.com')
```

## Scheduling

```python
# Create scheduled Actor run via API
schedule = client.schedule('my-schedule').get_or_create({
    'name': 'Daily Instagram Scrape',
    'isEnabled': True,
    'cronExpression': '0 9 * * *',  # Every day at 9 AM
    'actions': [{
        'type': 'RUN_ACTOR',
        'actorId': 'apify/instagram-scraper',
        'runInput': {
            'username': ['target_account'],
            'resultsLimit': 50
        }
    }]
})
```

**Via Console:** https://console.apify.com/schedules

## Webhooks

```python
# Set up webhook to notify on Actor completion
run = client.actor('apify/web-scraper').call(
    run_input={'startUrls': [{'url': 'https://example.com'}]},
    webhooks=[{
        'eventTypes': ['ACTOR.RUN.SUCCEEDED'],
        'requestUrl': 'https://your-server.com/webhook',
        'payloadTemplate': '''
        {
            "actorId": {{actorId}},
            "runId": {{runId}},
            "datasetId": {{defaultDatasetId}}
        }
        '''
    }]
)
```

**Webhook Events:**
- `ACTOR.RUN.CREATED`
- `ACTOR.RUN.SUCCEEDED`
- `ACTOR.RUN.FAILED`
- `ACTOR.RUN.ABORTED`
- `ACTOR.RUN.TIMED_OUT`

## MCP Integration

```bash
# Install Apify MCP server
npm install -g @apify/mcp-server

# Or run with npx
npx @apify/mcp-server
```

**Configure (Claude Desktop):**

```json
{
  "mcpServers": {
    "apify": {
      "command": "npx",
      "args": ["-y", "@apify/mcp-server"],
      "env": {
        "APIFY_API_TOKEN": "apify_api_your_token"
      }
    }
  }
}
```

**Available MCP Tools:**
- `apify_run_actor` - Run any Actor from Apify Store
- `apify_get_dataset` - Retrieve Actor results
- `apify_search_actors` - Find Actors by keyword

**Usage:**

```
User: "Get the latest posts from @nasa on Instagram"
Assistant: [uses apify_run_actor with apify/instagram-scraper]

User: "Find restaurants in Chicago with ratings"
Assistant: [uses apify_run_actor with Google Maps scraper]
```

## Skills.sh Integration

```bash
# Add Apify skills to AI coding assistant
npx skills add apify/actor-development
npx skills add apify/ultimate-scraper

# List available skills
npx skills list | grep apify

# Skills available:
# - apify-actor-development
# - apify-ultimate-scraper
# - apify-web-scraper
```

## Monetization (Actor Store)

### Publish Actor to Store

```bash
# Make Actor public
apify push --public

# Set pricing (in Apify Console)
# - Free tier available
# - Pay-per-event pricing
# - Flat fee options
```

**Pricing Models:**
- Free with credit attribution
- Pay-per-result (e.g., $0.001 per scraped item)
- Pay-per-run (e.g., $0.10 per run)
- Monthly subscription tiers

### Actor Development for Revenue

```python
# Enable pay-per-event in Actor
from apify import Actor

async with Actor:
    # Charge for each result
    await Actor.push_data({'result': 'data'})
    
    # Platform automatically tracks usage
    # Revenue share: 80% developer, 20% Apify
```

**Popular Actor Revenue:**
- Top Actors: $500-$5,000/month
- Niche scrapers: $50-$500/month
- Platform-specific: Higher earnings

## CI/CD Integration

### GitHub Actions

```yaml
name: Deploy Actor

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Push Actor to Apify
        uses: apify/push-actor-action@v2
        with:
          apify-token: ${{ secrets.APIFY_TOKEN }}
          build-tag: latest
```

### GitLab CI

```yaml
deploy:
  stage: deploy
  script:
    - npm install -g apify-cli
    - apify login --token $APIFY_TOKEN
    - apify push
  only:
    - main
```

## Advanced Features

### Browser Pool Management

```python
from apify import Actor
from crawlee.playwright_crawler import PlaywrightCrawler

async with Actor:
    crawler = PlaywrightCrawler(
        max_requests_per_crawl=1000,
        max_concurrency=10,  # 10 parallel browsers
        browser_pool_options={
            'max_open_pages_per_browser': 5,
            'retire_browser_after_page_count': 100
        }
    )
    await crawler.run(['https://example.com'])
```

### Anti-Scraping Handling

```python
# Rotating user agents, headers, cookies
run = client.actor('apify/web-scraper').call(run_input={
    'startUrls': [{'url': 'https://protected-site.com'}],
    'useChrome': True,
    'useStealth': True,  # Anti-detection plugin
    'proxyConfiguration': {
        'useApifyProxy': True,
        'apifyProxyGroups': ['RESIDENTIAL']
    }
})
```

### Metamorph (Actor Chaining)

```python
# Chain multiple Actors
from apify import Actor

async with Actor:
    # Process data with another Actor
    await Actor.metamorph(
        target_actor_id='another-actor/data-processor',
        run_input={'data': 'from previous actor'}
    )
```

## Performance Characteristics

**Compute Resources:**
- Micro: 256 MB RAM, 0.1 CPU (~$0.02/hour)
- Small: 1 GB RAM, 0.25 CPU (~$0.05/hour)
- Medium: 4 GB RAM, 1 CPU (~$0.20/hour)
- Large: 16 GB RAM, 4 CPU (~$0.80/hour)
- Custom: Up to 128 GB RAM, 32 CPU

**Speed:**
- Simple scraping: 1-5 pages/second
- Browser-based: 0.5-2 pages/second
- With proxies: 0.2-1 pages/second
- Parallel runs: Scale horizontally

**Storage:**
- Dataset: Unlimited items
- Key-value store: 25 GB per store
- Request queue: Unlimited requests
- Retention: 7 days unnamed, unlimited named

**Limits:**
- Free tier: $5 credit/month (~5 hours compute)
- Actor timeout: 24 hours max
- Memory: Up to 128 GB
- Concurrent runs: Unlimited (per account)

## Common Commands

### CLI

```bash
# Create Actor from template
apify create my-actor --template python-playwright

# Run locally
apify run

# Run with input
apify run --input '{"url": "https://example.com"}'

# Deploy to platform
apify push

# Call Actor from CLI
apify call apify/web-scraper --input '{"startUrls":[{"url":"https://example.com"}]}'

# List Actors
apify actors ls

# Download dataset
apify dataset download <dataset-id>

# View Actor info
apify actor info apify/web-scraper
```

### API (cURL)

```bash
# Run Actor
curl -X POST "https://api.apify.com/v2/acts/apify~web-scraper/runs" \
  -H "Authorization: Bearer $APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "startUrls": [{"url": "https://example.com"}],
    "maxCrawlDepth": 1
  }'

# Get run status
curl "https://api.apify.com/v2/actor-runs/{run-id}" \
  -H "Authorization: Bearer $APIFY_TOKEN"

# Get dataset items
curl "https://api.apify.com/v2/datasets/{dataset-id}/items" \
  -H "Authorization: Bearer $APIFY_TOKEN"
```

## Troubleshooting

### Actor Failing or Timing Out

```python
# Increase timeout and memory
run = client.actor('apify/web-scraper').call(
    run_input={'startUrls': [{'url': 'https://example.com'}]},
    memory_mbytes=4096,  # 4 GB
    timeout_secs=3600    # 1 hour
)
```

### Proxy Errors

```python
# Try different proxy type
run_input = {
    'proxyConfiguration': {
        'useApifyProxy': True,
        'apifyProxyGroups': ['RESIDENTIAL'],  # Switch from SHADER
        'apifyProxyCountry': 'US'
    }
}

# Or use custom proxies
run_input = {
    'proxyConfiguration': {
        'proxyUrls': [
            'http://user:pass@proxy1.com:8080',
            'http://user:pass@proxy2.com:8080'
        ]
    }
}
```

### Rate Limiting

```python
# Add delays between requests
from apify import Actor
import asyncio

async with Actor:
    for url in urls:
        # Process URL
        await asyncio.sleep(2)  # 2 second delay
```

### Memory Limit Exceeded

```python
# Use larger Actor instance
run = client.actor('my-actor').call(
    run_input={},
    memory_mbytes=8192  # 8 GB
)

# Or optimize Actor code to use less memory
# - Process data in batches
# - Clear variables after use
# - Use generators instead of loading all data
```

### No Results in Dataset

```python
# Check if data is being pushed
from apify import Actor

async with Actor:
    Actor.log.info('Starting scrape...')
    
    # Make sure to call push_data
    await Actor.push_data({'test': 'data'})
    
    Actor.log.info('Data pushed!')
```

## Actor Programming Model

**Philosophy:** Actors follow the UNIX philosophy—programs that do one thing well and can be easily composed into complex systems.

**Benefits:**
- **Reusable:** Package once, run anywhere
- **Composable:** Chain Actors with metamorph
- **Shareable:** Publish to Store for others
- **Scalable:** Serverless execution
- **Monetizable:** Earn from Actor usage

**Actor Lifecycle:**
1. Input validation (INPUT_SCHEMA.json)
2. Environment setup (Docker container)
3. Main code execution (src/main.py or src/main.js)
4. Data storage (datasets, KV stores)
5. Output and cleanup

## Related Tools

- **Firecrawl** - API-first crawler optimized for AI/LLM data ingestion
- **Crawlee** - Open-source scraping library powering Apify (by Apify team)
- **Playwright** - Browser automation framework (integrated in Apify)
- **Scrapy** - Python scraping framework (can run as Actor)
- **Selenium** - WebDriver automation (can run as Actor)
- **BeautifulSoup** - HTML parsing library (used in Actors)

## Reference

- [Official Website](https://apify.com/)
- [Documentation](https://docs.apify.com/)
- [API Reference](https://docs.apify.com/api/v2)
- [Python SDK](https://github.com/apify/apify-sdk-python)
- [JavaScript SDK](https://github.com/apify/apify-sdk-js)
- [Actor Store](https://apify.com/store)
- [Actor Whitepaper](https://github.com/apify/actor-whitepaper)
- [Community Forum](https://community.apify.com/)
- [Discord Community](https://discord.com/invite/jyEM2PRvMU)

## Lessons Learned

- Apify Store's 53,000+ pre-built Actors save months of development time for platform-specific scraping
- Actor marketplace monetization creates passive income for developers
- Serverless architecture scales horizontally without infrastructure management
- Residential proxies essential for Instagram, Facebook, LinkedIn scraping
- Built-in storage (datasets, KV stores, queues) eliminates external database needs
- Scheduling enables hands-off recurring data collection
- CI/CD integration with GitHub Actions streamlines Actor deployment
- MCP and Skills.sh integration make Apify first-class in AI agent ecosystems
- Pay-per-event model aligns costs with actual usage
- Python SDK lifecycle management reduces boilerplate by 80%
- Crawlee integration provides production-ready crawling patterns
- Actor composition via metamorph enables complex multi-stage pipelines
- Free tier ($5 credit) sufficient for testing and small projects

## Status

Status: production-ready
Founded: 2015
Platform Actors: 53,000+
Community: 100,000+ developers
License: Various (Actors are individually licensed)
Last Updated: August 2026
