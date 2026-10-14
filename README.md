# SAM.gov Scraper – New Federal Contract Opportunity Alerts

[![Run on Apify](https://img.shields.io/badge/Run%20on-Apify-blue)](https://apify.com/cce_partners/samgov-opportunity-radar)

👉 **Run it now on Apify: https://apify.com/cce_partners/samgov-opportunity-radar**

A **SAM.gov scraper and RFP alert tool** that returns only **new or amended federal contract opportunities** (solicitations, presolicitations, sources sought, combined synopses and awards) matching your NAICS codes, set-asides, agency and state.

**No API key needed.** By default it reads SAM.gov's keyless official public nightly extract (up to 24h old). Add an optional free SAM.gov API key for real-time results.

## Why this instead of SAM.gov saved searches?
- **Only new or changed.** Each monitor remembers every notice it has seen and returns a notice again only when it's amended (deadline, description, award).
- **No key required.** Keyless mode uses SAM.gov's official public nightly extract (up to 24h old). Add a key for real-time results.
- **No personal contact data.** No names, emails or phone numbers, so the output is safe to share and store.
- **Pay per result, no platform usage fees.** Unchanged notices are never charged.

## Use cases

### 1. 8(a) IT services firm: daily new solicitations
```json
{
  "naicsCodes": ["541511", "541512", "541519"],
  "setAside": "8A",
  "noticeTypes": ["o", "k"],
  "lookbackDays": 7,
  "monitorName": "8a-it-daily"
}
```
Schedule it daily and connect Google Sheets. Each morning you get only the new or amended IT solicitations.

### 2. SDVOSB contractor: Texas construction bids and sources sought
```json
{
  "naicsCodes": ["236220", "238210"],
  "setAside": "SDVOSBC",
  "state": "TX",
  "noticeTypes": ["o", "p", "r"],
  "lookbackDays": 14,
  "monitorName": "sdvosb-tx-construction",
  "aiTagging": true
}
```
Sources sought (`r`) and presolicitations (`p`) give you early warning before the RFP drops. Tags add the sector, set-aside category and days to respond.

### 3. Capture team: cybersecurity across all agencies
```json
{
  "keywords": "cybersecurity",
  "noticeTypes": ["o", "k", "r"],
  "lookbackDays": 30,
  "maxItems": 2000,
  "monitorName": "cyber-capture"
}
```

## Notice type codes
o = Solicitation, p = Presolicitation, k = Combined Synopsis/Solicitation, r = Sources Sought, s = Special Notice, a = Award, u = J&A, g = Sale of Surplus, i = Intent to Bundle.

## Set-aside codes
SBA (Total Small Business), SBP (Partial SB), 8A / 8AN (8(a)), HZC / HZS (HUBZone), SDVOSBC / SDVOSBS (Service-Disabled Veteran-Owned), WOSB / WOSBSS / EDWOSB / EDWOSBSS (Women-Owned), VSA / VSS (Veteran).

## FAQ
**Do I need a SAM.gov API key?** No. Without a key the Actor uses SAM.gov's official public nightly extract, which can be up to 24 hours old. A free key (sam.gov → Account Details → Public API Key) gives you real-time results.

**How does "only new or changed" work?** Each `monitorName` keeps a fingerprint of every notice it has seen. A notice is returned again only if it's amended. Turn on `resetState` to start fresh.

**Can I run several searches?** Yes. Give each filter set its own `monitorName`.

**Does it include contracting officer emails or phone numbers?** No, by design. Use the `samUrl` link to see contacts on SAM.gov.

**How much does it cost?** $0.01 per run plus $4 per 1,000 new or changed opportunities, and $2 per 1,000 AI tags if you turn them on. A daily alert with 20 new notices costs about $0.09.

**Can I use it from an AI agent?** Yes. Call it through the Apify MCP server or API. The output is flat JSON with stable fields such as `noticeId`, `title`, `responseDeadline` and `samUrl`.

**Is this affiliated with SAM.gov or GSA?** No. It's an independent tool that uses SAM.gov's official public data.

## Two data modes

| | **Keyless (default when no key)** | **API key (recommended for real-time)** |
|---|---|---|
| Source | Official SAM.gov public nightly extract ([Data Services](https://sam.gov/data-services/Contract%20Opportunities/datagov?privacy=Public)) | SAM.gov Get Opportunities Public API |
| Freshness | **Up to 24h old** (file regenerated nightly) | Real-time |
| Setup | None | Free key: sam.gov > Account Details > Public API Key |
| Daily quota | None from SAM.gov | Your key's daily limit |

If you supply `samApiKey`, the Actor always uses the API. **Official public nightly extract, up to 24h old; add a free SAM.gov API key for real-time results.**

We never scrape SAM.gov web pages. Keyless mode uses only the public bulk extract that SAM.gov publishes for download.

### Field differences in keyless mode
- `awardeeUei` is always empty (not in the extract).
- `naicsCodes` (secondary NAICS list) is empty; `naicsCode` (primary) is present.
- `popCity` has the name only, no city code.
- `agencyPathCode` is built from CGAC / FPDS / AAC codes and may differ from the API's code path; `organizationCode` filtering is approximate.
- `keywords` is a local case-insensitive title match (the API runs its own title search), so counts can differ slightly.
- Covers notices currently in the extract (active notices, roughly the last year), latest version only.

## Output
Same columns in both modes: noticeId, title, solicitationNumber, noticeType, baseType, active, postedDate, responseDeadline, archiveDate, department, subTier, office, agencyPathCode, naicsCode, pscCode, setAsideCode, setAside, office/place-of-performance location, award fields, samUrl, additionalInfoUrl, changeType, optional tag_* columns. No contact names, emails or phones, in either mode.

## Cost
A keyless test run with 512 MB memory and maxItems 10 took about 6 s and cost about $0.0003 in platform usage.

## Companion tools
- **[SEC Form D New Raises Feed](https://apify.com/cce_partners/sec-form-d-new-raises-feed)**: new private fundraising filings from SEC Form D.
- **GovCon Recompete Radar** (coming soon): spot expiring federal contracts before they're recompeted.

---
**Get started:** https://apify.com/cce_partners/samgov-opportunity-radar
