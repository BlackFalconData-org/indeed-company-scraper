# Indeed Company Scraper

Extract structured data from [indeed.com](https://indeed.com) — Turn any Indeed company page into clean, structured JSON. Get star-rated employee reviews with full text plus a firmographic overview of industry, size and revenue. Monitor new reviews incrementally, with compact results for AI agents.

**[Indeed Company Scraper on Apify →](https://apify.com/blackfalcondata/indeed-company-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every listing returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from indeed.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from indeed.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired listings. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per listing is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "maxResults": 50
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `companies` | array | — | Company names or Indeed company URLs. One string or a JSON array (e.g. ["Google", "https://www.indeed.com/cmp/Amazon-com"]). Names are resolved automatically — "general motors" finds the right company page. |
| `startUrls` | array | — | Alias for Companies — paste direct Indeed company or reviews URLs (indeed.com/cmp/...). Merged with the Companies list. |
| `includeReviews` | boolean | `true` | Scrape the company's employee reviews (star ratings, five sub-ratings, review title and full text, job title, location, employment status). |
| `includeOverview` | boolean | `true` | Emit one firmographic overview record per company: rating, review count, industry, employee range, revenue, HQ, founded year, CEO, website, happiness score, and category ratings. |
| `includeInterviews` | boolean | `false` | Also emit one interview-insights record per company: how candidates got interviews, hiring-process duration, and sample interview questions and experiences. |
| `maxReviews` | integer | `100` | Maximum reviews to scrape per company (newest first). Set 0 for all available reviews (best-effort — large companies have thousands). |
| `compact` | boolean | `false` | Output only core fields per record. Ideal for AI agents, MCP workflows, and LLM context windows where token budget matters. |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Return only reviews not seen in a previous run for the same company set. Ideal for monitoring new reviews on a schedule. State is keyed automatically; set State Key to isolate universes. |
| `stateKey` | string | — | Optional stable identifier for the tracked company set. Leave empty to auto-derive from the Companies list. Use distinct keys for unrelated monitors to keep their state separate. |
| `telegramToken` | string | — | Telegram bot token from @BotFather. Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID where alerts are sent (e.g. "-100123456789" for a private group, or "@yourchannel"). |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Get one from Server Settings → Integrations → Webhooks. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. Create at api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | WhatsApp Business phone number ID from Meta Business Manager (the numeric ID, not the phone number). Free service-conversation messages within 24 hours of last user-initiated contact. |
| `whatsappAccessToken` | string | — | Meta Cloud API access token with `whatsapp_business_messaging` scope. Get a permanent token from a system user in Meta Business Manager. |
| `whatsappTo` | string | — | Recipient phone in E.164 format (e.g. +919876543210). Recipient must have messaged your business number within the last 24 hours. |
| `webhookUrl` | string | — | Generic webhook URL that receives a JSON POST with the review payload + run metadata. Universal escape hatch for n8n / Make / Zapier / your own backend. |
| `webhookHeaders` | object | — | Optional headers (e.g. {"Authorization": "Bearer xyz"}) sent with the webhook POST. |
| `notificationLimit` | integer | `5` | Maximum number of reviews included in each notification message (1–20). Excess reviews are still in the dataset. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for newly-seen reviews. |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results. Best-effort across MCP connectors. |
| `mcpIssueTeam` | string | — | Only for issue-tracker connectors: the team (name or ID) the summary issue is created under, if required. |

---

## Output fields

Every listing returns the same 25-field schema. Missing values are `null` — never omitted.

- `recordType`
- `company`
- `sourceUrl`
- `indeedUrl`
- `rating`
- `reviewCount`
- `reviewCountFormatted`
- `happinessGrade`
- `happinessScore`
- `description`
- `industry`
- `sectorNames`
- `employeeRange`
- `employeeRangeRaw`
- `revenue`
- `revenueRaw`
- `foundedYear`
- `headquarters`
- `website`
- `ceoName`
- `ceoApproval`
- `logoUrl`
- `categoryRatings`
- `happinessResponses`
- `scrapedAt`

---

## Sample output

One object per listing. Here is a real example from a production run:

```json
{
  "recordType": "overview",
  "company": "Google",
  "sourceUrl": "https://www.indeed.com/cmp/Google",
  "indeedUrl": "https://www.indeed.com/cmp/Google",
  "rating": 4.3,
  "reviewCount": 6235,
  "reviewCountFormatted": "6.2K",
  "happinessGrade": "EXCELLENT",
  "happinessScore": 77,
  "description": "Since our founding in 1998, Google has grown by leaps and bounds. Starting from two computer science students in a university dorm room, we now have thousands of employees and offi…",
  "industry": "Internet & Web Services",
  "sectorNames": [
    "Internet & Web Services"
  ]
}
```

*Truncated — full records contain 25 fields. See Output fields for the complete schema.*

**[Try Indeed Company Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/indeed-company-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Result | $0.002 |

See the [actor on Apify](https://apify.com/blackfalcondata/indeed-company-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape indeed.com?**
Use this actor on Apify to extract structured data from indeed.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get indeed.com data as JSON, CSV, or Excel?**
The actor writes each listing to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape indeed.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check indeed.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for listings extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage. Expired listings can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

- [Indeed Job Scraper](https://apify.com/blackfalcondata/indeed-scraper?fpr=1h3gvi) — Indeed job listings: title, company, salary, apply URL and full descriptions across 62 markets
- [Indeed Salary Scraper](https://apify.com/blackfalcondata/indeed-salary-scraper?fpr=1h3gvi) — Indeed pay data: median/min/max by job title and location, top-paying companies and cities

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Indeed Company Scraper](https://apify.com/blackfalcondata/indeed-company-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 07*
