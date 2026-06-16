[Europages Scraper](https://apify.com/santamaria-automations/europages-scraper?fpr=data)

Extract B2B company data from [Europages.com](https://www.europages.com), Europe's largest business directory with millions of companies across 30+ countries.

## What data do you get?

For each company, the scraper extracts:

- **Company name** and description
- **Website URL** and phone number
- **Full address** (street, postal code, city, country)
- **GPS coordinates** (latitude/longitude)
- **Employee count** (e.g., "50-99", "1000+")
- **Year founded**
- **Supplier type** (Manufacturer, Distributor, Service provider, etc.)
- **Product categories** and certifications (ISO, etc.)
- **Company logo**

## Use with AI Agents (MCP)

Connect this actor to any MCP-compatible AI client — Claude Desktop, Claude.ai, Cursor, VS Code, LangChain, LlamaIndex, or custom agents.

**Apify MCP server URL:**

```
https://mcp.apify.com?tools=santamaria-automations/europages-scraper
```

**Example prompt once connected:**

> "Use `europages-scraper` to find all manufacturers in Germany for 'metalworking'. Return company name, city, employee count, website, and phone as a table."

Clients that support dynamic tool discovery (Claude.ai, VS Code) will receive the full input schema automatically via `add-actor`.

## How to use

### Option 1: Paste a search URL

1. Go to [europages.co.uk](https://www.europages.co.uk) and search for companies
2. Apply filters (country, supplier type, employee count, etc.)
3. Copy the search results URL
4. Paste it into the **Search URLs** field

Works with all Europages domains and URL formats:

- `https://www.europages.co.uk/en/search?q=metalworking&countries=DE`
- `https://www.europages.co.uk/en/search/supplier-type/production?countries=CH_DE_IT&q=schiffbau`
- `https://www.europages.co.uk/companies/germany/software.html`

### Option 2: Use search query + country

Set the **Search Query** (e.g., "software") and **Country** (e.g., "germany") fields. The scraper builds the URL automatically.

### Input example

```
{
  "searchQuery": "metalworking",
  "country": "DE",
  "language": "en",
  "maxResults": 100,
  "proxyConfiguration": {
    "useApifyProxy": true
  }
}
```

### Input parameters

| Parameter | Type | Description |
| --- | --- | --- |
| `searchUrls` | array | Europages search result URLs (supports all country domains and URL formats) |
| `searchQuery` | string | Search keyword (alternative to searchUrls) |
| `searchType` | string | `company` or `product` (default: `company`) |
| `country` | string | Country code for search, e.g. `DE`, `FR`, `NL` (default: `DE`) |
| `language` | string | Language for descriptions and categories. Supports all 21 Europages languages: `en`, `fr`, `de`, `es`, `it`, `nl`, `tr`, `cs`, `da`, `et`, `el`, `lt`, `hu`, `no`, `pl`, `pt`, `ro`, `sl`, `fi`, `sv`, `bg` (default: `en`) |
| `maxResults` | integer | Maximum companies to scrape (default: 100) |
| `includeDetails` | boolean | Fetch detail pages for social URLs, certifications (default: false). Most data is available without this. |
| `proxyConfiguration` | object | Proxy settings |

## Output example

```
{
  "id": "20029302",
  "name": "Innowise Group",
  "description": "Company for Software Development...",
  "website": "https://innowise.com/de",
  "phone": "+4968198899959",
  "email": null,
  "address": "Grosse Gallusstrasse 16, 60312 Frankfurt am Main",
  "city": "Frankfurt am Main",
  "postal_code": "60312",
  "country": "Germany",
  "country_code": "DE",
  "latitude": 50.11159,
  "longitude": 8.6734,
  "logo_url": "https://media.visable.com/...",
  "employee_count": "1000+",
  "year_founded": "2007",
  "supplier_type": "Service provider",
  "categories": ["Software development", "IT consulting"],
  "certifications": ["ISO 9001"],
  "source_url": "https://www.europages.co.uk/en/company/innowise-group-20029302",
  "source_platform": "europages.com",
  "scraped_at": "2026-03-26T10:30:00Z"
}
```

## Tips

- **Language** controls descriptions, categories, and supplier types. Set `language: "en"` for English, `language: "de"` for German, etc. All 21 Europages languages are supported.
- **Include Details = OFF** (default) is fast and already returns most data: website, phone, description, address, GPS, employee count, supplier type, categories, and more.
- **Include Details = ON** adds social URLs (LinkedIn, Facebook), certifications, and email from the detail page. Only enable if you need these.
- **Datacenter proxy** works fine for Europages, no need for residential.

## Pricing

This actor uses pay-per-result pricing. Each company is charged exactly once — the price depends on whether you enable `includeDetails` or not:

| Event | Price | When charged |
| --- | --- | --- |
| Actor start | $0.005 | Once per run |
| Company found | $0.003 | Only when `includeDetails` is OFF — returns basic search data (name, city, country, employee count) |
| Company with full details | $0.005 | Only when `includeDetails` is ON — returns full profile (website, phone, address, GPS, description, categories, certifications, etc.) |

### Cost examples

**With full details (`includeDetails` = ON, default):**
1 × $0.005 start + 1,000 × $0.005 detail = **$5.005 total**

**Without details (`includeDetails` = OFF, faster):**
1 × $0.005 start + 1,000 × $0.003 search = **$3.005 total**

## Related Actors

**International B2B Directories**

- [Made-in-China Scraper](https://apify.com/santamaria-automations/made-in-china-scraper) -- Chinese manufacturer and supplier directory
- [GlobalSources Scraper](https://apify.com/santamaria-automations/globalsources-scraper) -- Asian manufacturer and supplier directory
- [Clutch.co Scraper](https://apify.com/santamaria-automations/clutch-co-scraper) -- B2B agency reviews and ratings

**DACH Business Directories**

- [wlw.de Scraper](https://apify.com/santamaria-automations/wlw-de-scraper) -- German B2B supplier directory
- [Gelbe Seiten Scraper](https://apify.com/santamaria-automations/gelbeseiten-de-scraper) -- German Yellow Pages
- [Herold.at Scraper](https://apify.com/santamaria-automations/herold-at-scraper) -- Austrian Yellow Pages
- [FirmenABC.at Scraper](https://apify.com/santamaria-automations/firmenabc-at-scraper) -- Austrian business directory
- [search.ch Scraper](https://apify.com/santamaria-automations/search-ch-scraper) -- Swiss business directory

**Enrich your leads**

- [Website Email & Phone Scraper](https://apify.com/santamaria-automations/website-email-scraper) -- Extract emails and phones from company websites
- [Website Contact Extractor](https://apify.com/santamaria-automations/website-contact-extractor) -- Extract team members and decision-makers
- [Google Maps Scraper](https://apify.com/santamaria-automations/google-maps-scraper) -- Find businesses by location
- [Trustpilot Reviews Scraper](https://apify.com/santamaria-automations/trustpilot-scraper) -- Get company reviews and ratings

If something is not working or you're missing a feature or data field, please [open an issue](https://console.apify.com/actors/nxWa8REWYk8Epik7y/issues) and we'll look into it.