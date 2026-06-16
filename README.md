[Europages Scraper](https://apify.com/kawsar/europages-scraper?fpr=data)

Search Europages by keyword and pull company profiles into a clean, structured dataset. Names, addresses, phone numbers, websites, founding years, employee counts, coordinates, supplier types, and product keywords — one row per company, ready to use.

## What it does

Europages is one of Europe's largest B2B directories, listing several million companies across 26 countries. This actor searches it by keyword, pages through the results automatically, and pushes each company record to your dataset as it goes. Each page returns 30 companies; the actor keeps paging until it hits your per-keyword limit or runs out of results.

You can run multiple keywords in a single job. Each keyword gets its own independent search — so if you add three keywords at 100 results each, you get up to 300 records total, all in one run.

## How it works

The actor fetches Europages search result pages and parses the structured company data embedded in each page's server-side rendered state. No scraping tricks, no browser automation — the data comes directly from the page payload, which means it's fast and reliable.

Each page is fetched with built-in bypass infrastructure to handle anti-bot measures. If a page returns an incomplete response, the actor retries up to 3 times before moving on.

## Input

| Field | Required | Default | Description |
| --- | --- | --- | --- |
| `searchQueries` | Yes | — | One or more keywords to search (e.g. `embroidery`, `solar panels`, `steel pipes`) |
| `maxItems` | No | 100 | Max company records to collect **per keyword** (max 1,000) |
| `requestTimeoutSecs` | No | 60 | Per-request timeout in seconds — raise to 90 or 120 if you see timeout errors |

### Example input

```
{
  "searchQueries": ["embroidery", "workwear", "textile printing"],
  "maxItems": 200
}
```

## Output

Each dataset record is one company. Fields are omitted when the data is not publicly available rather than returned as null.

| Field | Type | Description |
| --- | --- | --- |
| `companyName` | string | Company name |
| `companyBio` | string | Short description from the company profile |
| `city` | string | City |
| `countryCode` | string | ISO country code (e.g. `FR`, `DE`, `IT`) |
| `street` | string | Street address |
| `postalCode` | string | Postal / zip code |
| `phoneNumber` | string | Phone number |
| `website` | string | Company website |
| `logoUrl` | string | Logo image URL |
| `latitude` | number | Geographic latitude |
| `longitude` | number | Geographic longitude |
| `foundingYear` | integer | Year the company was founded |
| `employeeCount` | string | Headcount range (e.g. `11-50`, `51-200`) |
| `productCount` | integer | Number of products listed on Europages |
| `europagesUrl` | string | Direct link to the company's Europages profile |
| `europagesId` | integer | Internal Europages company ID |
| `keywords` | array | Product and activity keywords from the profile |
| `supplierTypes` | array | Supplier labels (e.g. `Manufacturer`, `Distributor`) |
| `packageTier` | string | Europages membership tier — omitted for free listings |
| `scrapedAt` | string | ISO 8601 timestamp of when the record was collected |

### Example output record

```
{
  "companyName": "UNITON",
  "companyBio": "Since 2001, UNITON has been operating its various branches of activity in the European market, specialising in embroidery and related textile services.",
  "city": "Beaurepaire",
  "countryCode": "FR",
  "street": "Chemin du Pied Menu, ZI Les Fromentaux",
  "postalCode": "38270",
  "phoneNumber": "+33474482814",
  "website": "http://www.uniton.fr/",
  "logoUrl": "https://media.visable.com/company/9d8740d7-...",
  "latitude": 45.324625,
  "longitude": 5.052226,
  "foundingYear": 2001,
  "employeeCount": "5-9",
  "productCount": 10,
  "europagesUrl": "https://www.europages.co.uk/en/company/uniton-20160084/",
  "europagesId": 1344161,
  "keywords": ["embroidery", "textiles", "workwear"],
  "supplierTypes": ["distribution"],
  "scrapedAt": "2026-04-29T10:00:00+00:00"
}
```

## Output format and export

Results are stored in an Apify dataset and available in multiple formats from the Apify console:

- **JSON** — default, one object per record
- **CSV** — flat spreadsheet, opens directly in Excel or Google Sheets
- **XLSX** — Excel format
- **JSONL** — one JSON object per line, useful for streaming pipelines

Use the Apify API to fetch results programmatically or connect via Zapier, Make, or any webhook-based integration.

## Performance

| Metric | Value |
| --- | --- |
| Results per page | 30 companies |
| Retries per page | Up to 3 on empty response |
| Max results per keyword | 1,000 |
| Actor memory | 512 MB |
| Actor timeout | 1 hour |

Speed depends on network latency and Europages server response time. Most searches collect 30 results in 3–6 seconds per page. A full run of 1,000 results across 34 pages typically takes 3–8 minutes.

## Good use cases

- Building prospect lists of suppliers or manufacturers across multiple product categories in one run
- Finding European companies in a niche before entering a new market
- Enriching a CRM with contact data, coordinates, and founding years
- Mapping supplier density by country or product type
- Monitoring which companies list a particular product or service over time

## Limitations

- **Max 1,000 results per keyword.** Europages limits how deep you can paginate. For very broad keywords that match tens of thousands of companies, narrow your search term or split by region using separate keyword runs.
- **Contact data varies.** Phone numbers are only shown when the company has chosen to make them public.
- **Keyword matching is Europages' own.** The actor collects whatever Europages returns for a given search term — it does not apply additional filtering.
- **Free listings have no package tier.** The `packageTier` field only appears for companies on paid Europages plans.

## Tips

**Be specific with keywords.** `cnc machining` returns sharper results than `manufacturing`. `solar panel installation` beats `energy`. The more precise the term, the more relevant the companies.

**Use multiple keywords in one job.** Instead of running three separate jobs, add all your terms at once. Each runs independently with its own result limit.

**Combine with country targeting.** Europages search results already include a `countryCode` field on every record. After collection, filter your dataset by country in a spreadsheet or downstream pipeline.

**Raise the timeout if needed.** If you see timeout errors, set `requestTimeoutSecs` to 90 or 120. The default of 60 seconds covers most searches.