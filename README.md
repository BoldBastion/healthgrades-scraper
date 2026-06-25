[Healthgrades Scraper](https://apify.com/alizarin_refrigerator-owner/healthgrades-scraper?fpr=data)

# Healthgrades Doctor & Provider Scraper

Scrape Healthgrades profiles for doctors, dentists, and healthcare providers. Extract patient reviews, ratings, specialties, board certifications, insurance accepted, hospital affiliations, and practice information. Essential for healthcare reputation monitoring and provider research.

---

## Quick Start

### Test with Demo Mode (free, no API key needed)

```
{
  "demoMode": true,
  "providerUrl": "https://example.com"
}
```

### Run with real data

```
{
  "demoMode": false,
  "scrapeType": "search",
  "providerUrl": "https://example.com",
  "gender": "any",
  "acceptingNewPatients": false,
  "includeReviews": true,
  "maxReviewsPerProvider": 25,
  "maxResults": 50
}
```

---

## Input Parameters

| Parameter | Type | Default | Required | Description |
| --- | --- | --- | --- | --- |
| `scrapeType` | string | `"search"` | No | Type of scraping to perform |
| `providerUrl` | string | - | No | Direct Healthgrades profile URL (for provider_profile type) |
| `providerName` | string | - | No | Doctor or provider name to search |
| `specialty` | string | - | No | Medical specialty (e.g., 'Cardiologist', 'Dentist', 'Dermatologist') |
| `location` | string | - | No | City, State or ZIP code |
| `insuranceAccepted` | string | - | No | Filter by insurance provider |
| `minRating` | number | - | No | Minimum rating (1.0-5.0) |
| `gender` | string | `"any"` | No | Filter by provider gender |
| `acceptingNewPatients` | boolean | `false` | No | Only show providers accepting new patients |
| `includeReviews` | boolean | `true` | No | Scrape patient reviews for each provider |
| `maxReviewsPerProvider` | integer | `25` | No | Maximum reviews to collect per provider |
| `maxResults` | integer | `50` | No | Maximum number of providers to scrape |
| `proxyConfiguration` | object | - | No | Proxy settings for the scraper |
| `demoMode` | boolean | `true` | No | Return sample data without actual scraping (for testing) |
| `webhookUrl` | string | - | No | URL to POST results when scraping completes (Zapier, Make, n8n, custom endpoint) |

---

## Pricing

This actor uses **pay-per-event** billing:

| Event | Description | Price |
| --- | --- | --- |
| Provider Scraped | Each Healthgrades provider profile scraped | $0.05 |

**Demo mode is free** -- no charges for sample data.

---

## Troubleshooting

### "API error 429" or "Rate limit"

Too many requests. Wait a minute and try again, or reduce the number of items per run.

### No results or empty dataset

Check the run log for error messages. Common causes:

- Invalid input format (check the examples above)
- The target data doesn't exist or is too small to track

### How do I test without an API key?

Enable **Demo Mode** in the input. This returns realistic sample data so you can verify the output format works for your workflow.

---

**Built by John Rippy | [Actor Arsenal](https://actorarsenal.com)**