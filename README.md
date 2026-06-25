[Healthgrades Scraper](https://apify.com/h4sh/healthgrades-scraper?fpr=data)

Healthgrades doctor/profile scraper for healthcare lead generation, physician directory monitoring, and ratings research. Supports Healthgrades search URLs, specialty/location search, and direct doctor profile URLs. Uses HTTP first, then Camoufox browser fallback for protected pages.

## Features

- Conservative default run size for Apify daily tests and smoke tests.
- Residential proxy-ready configuration with US defaults.
- HTTP/HTML first where practical, with Camoufox browser fallback for protected pages.
- Multiple extraction strategies: embedded JSON, JSON-LD/app state, URL-pattern discovery, and DOM card parsing.
- Clean structured JSON pushed to the default dataset.

## Input

```
{
  "maxResults": 10,
  "proxyConfiguration": {
    "useApifyProxy": true,
    "apifyProxyGroups": [
      "RESIDENTIAL"
    ],
    "apifyProxyCountry": "US"
  },
  "specialty": "family-medicine",
  "location": "new-york-ny"
}
```

See `.actor/input_schema.json` for all options.

## Output fields

`name`, `specialty`, `rating`, `reviewCount`, `address`, `city`, `state`, `phone`, `profileUrl`, `sourceUrl`.

## Example output

```
{
  "sourceUrl": "https://example.com/search",
  "scrapedAt": "2026-04-28T00:00:00+00:00"
}
```

## Pricing guidance

## 💊 Pairs Well With

**[GoodRx Drug Price Scraper](https://apify.com/h4sh/goodrx-drug-price-scraper)** — pharmacy pricing for prescription drugs in 11 seconds.

Together they form the **Healthcare Data Pack**: provider intelligence + drug affordability data. Enterprise pricing for >100K combined records/month — contact via Apify support.

## Pricing guidance

Recommended Apify Store PPE pricing: **$2 per 1,000 results** for search-result records. Increase pricing if detail-page enrichment is enabled in a future version.

## Limitations

The target site may apply WAF, Cloudflare, PerimeterX, or similar anti-bot controls. The actor logs exact block evidence, attempts an alternative pattern/browser fallback, and exits gracefully if no data can be retrieved.