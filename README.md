[Europages Scraper](https://apify.com/rastriq/europages-scraper?fpr=data)

# Europages Scraper

Scrapes company profiles from **Europages**, Europe's largest B2B marketplace and supplier directory. Works across all regional domains (ES, EN, DE, FR, IT, PT) and any industry category.

## What you get

Each record is a company profile with:

| Field | Description |
| --- | --- |
| `name` | Company name |
| `country` | Country code (from address) |
| `city` | City |
| `postal_code` | Postal code |
| `address` | Street address |
| `region` | Region/state |
| `phone` | Phone number |
| `website` | Company website |
| `description` | Company description (up to 600 chars) |
| `products` | Products/services listed (up to 15) |
| `logo_url` | Company logo URL |
| `employees` | Employee count if available |
| `founded` | Founded year if available |
| `url` | Europages profile URL |
| `category` | Category slug used |
| `source` | Always `europages` |
| `scraped_at` | ISO 8601 timestamp |

## Input

| Field | Default | Description |
| --- | --- | --- |
| `category` | `alquiler-de-maquinaria` | Category slug from the Europages URL |
| `domain` | `europages.es` | Regional domain to scrape |
| `maxPages` | `0` (all) | Max listing pages |
| `maxItems` | `500` | Max company profiles |
| `proxyConfiguration` | — | Optional proxy settings |

## Example categories

- `alquiler-de-maquinaria` — machinery rental (ES)
- `construccion` — construction
- `maquinaria-agricola` — agricultural machinery
- `transporte` — transport
- `manufacturing` — manufacturing (on .com)