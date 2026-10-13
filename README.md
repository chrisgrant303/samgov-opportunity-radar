# SAM.gov Opportunity Radar – Federal Contract & Bid Alerts

[![Run on Apify](https://img.shields.io/badge/Run%20on-Apify-00a86b)](https://apify.com/cce_partners/samgov-opportunity-radar)

**▶ Try it on the Apify Store: https://apify.com/cce_partners/samgov-opportunity-radar**

SAM.gov Opportunity Radar is a **government contracts monitor** that watches SAM.gov and returns only the **new or changed** U.S. federal contract opportunities on each run. You get clean, flat, spreadsheet-ready rows, with no duplicates and no personal contact data.

Use it as a no-code **SAM.gov API alternative**. It calls the official SAM.gov Opportunities API with your own free key and handles paging, deduplication, change detection, and flattening for you. Schedule it daily and you have **federal contract opportunity alerts** you can send to Google Sheets, Slack, Zapier, Make, n8n, or any webhook.

> This repo contains documentation and examples only. The Actor runs on Apify: **[open the Apify Store listing](https://apify.com/cce_partners/samgov-opportunity-radar)**.

## What it does
- **Returns changes only.** It remembers what each named monitor has already seen, so every run returns `new` and `changed` notices only (amendments, new deadlines, award details).
- **Filters** by title keywords, NAICS codes, set-aside (8(a), HUBZone, SDVOSB, WOSB, EDWOSB, small business…), notice type (solicitation, presolicitation, sources sought, award…), place-of-performance state, agency code, and a lookback window of up to 364 days.
- **Produces clean output** of 30+ flat columns that work well in spreadsheets and BI tools.
- **Adds optional tags:** sector, set-aside category, stage (market research / open solicitation / award), days left to respond, and an optional AI one-line summary.
- **Is private by design.** Contracting-officer names, emails, and phone numbers are dropped on purpose.
- **Uses an official source.** It reads public U.S. government data through SAM.gov's own API. It is not a browser scraper.

## Who it's for
- Small businesses and 8(a) / HUBZone / SDVOSB / WOSB firms looking for **federal contract opportunities**
- Government contractors, BD teams, and capture managers who track NAICS codes
- Proposal consultants and GovCon analysts building bid pipelines
- Developers who want a **government contracts scraper** without maintaining SAM.gov API code

## Example input
```json
{
  "samApiKey": "YOUR_OWN_SAM_GOV_PUBLIC_API_KEY",
  "monitorName": "it-services-va",
  "keywords": "cybersecurity",
  "naicsCodes": ["541512", "541519"],
  "setAside": "SBA",
  "noticeTypes": ["o", "k", "r"],
  "state": "VA",
  "lookbackDays": 7,
  "maxItems": 1000,
  "aiTagging": true
}
```
You need a free SAM.gov public API key (sam.gov → Account Details → Public API Key).

## Sample output fields
| Field | Example |
|---|---|
| `noticeId` | `a1b2c3...` |
| `title` | `Cybersecurity Support Services` |
| `solicitationNumber` | `W91...-26-R-0001` |
| `noticeType` / `baseType` | `Solicitation` |
| `postedDate` / `responseDeadline` / `archiveDate` | ISO dates |
| `department` / `subTier` / `office` | Agency hierarchy |
| `naicsCode` / `pscCode` | `541512` / `D310` |
| `setAsideCode` / `setAside` | `SBA` / `Total Small Business` |
| `popCity` / `popState` / `popCountry` | Place of performance |
| `awardDate` / `awardNumber` / `awardAmount` / `awardeeName` | Award notices only |
| `samUrl` | Link to the notice on SAM.gov |
| `changeType` | `new` or `changed` |
| `tag_sector`, `tag_setAsideCategory`, `tag_stage`, `tag_daysToRespond`, `tag_summary` | When tagging is enabled |

There are no contact fields (no names, emails, or phone numbers).

## Pricing (pay per event)
- **$0.01** per run
- **$4.00 per 1,000** new or changed opportunities
- **$2.00 per 1,000** AI-tagged opportunities (optional)

You are never charged for unchanged notices. See the current pricing on the [Apify Store listing](https://apify.com/cce_partners/samgov-opportunity-radar).

## How to set up daily federal contract alerts
1. Open the [Actor on Apify](https://apify.com/cce_partners/samgov-opportunity-radar) and click **Try for free**.
2. Paste your SAM.gov API key and set your filters.
3. Add a **Schedule** (for example, daily at 7 AM) and an integration (Google Sheets, Slack, email, webhook).

## FAQ
**Is this a SAM.gov API alternative?** It is a managed wrapper around the official API. You skip the code, paging, deduplication, and change tracking.
**Does it scrape SAM.gov pages?** No. It uses the official public Opportunities API.
**Can I run several searches?** Yes. Give each one its own `monitorName`.

---
Published by [CCE Partners](https://apify.com/cce_partners). Not affiliated with GSA or SAM.gov.
**→ [SAM.gov Opportunity Radar on Apify Store](https://apify.com/cce_partners/samgov-opportunity-radar)**
