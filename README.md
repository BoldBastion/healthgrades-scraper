[Healthgrades Scraper](https://apify.com/lulzasaur/healthgrades-scraper?fpr=data)

Scrape doctor and healthcare provider listings from [Healthgrades.com](https://www.healthgrades.com). Extract detailed provider information including names, specialties, ratings, reviews, contact details, NPI numbers, and more.

## What data can you extract?

For each provider listing, the scraper extracts:

| Field | Description |
| --- | --- |
| `displayName` | Full name with credentials (e.g., "Dr. John Smith, MD") |
| `specialty` | Primary medical specialty |
| `specialties` | All listed specialties |
| `overallRating` | Patient rating (1-5 scale) |
| `reviewCount` | Number of patient reviews |
| `address` | Street address |
| `city` / `state` / `zip` | Location details |
| `phone` | Primary phone number |
| `officeName` | Practice or office name |
| `npi` | National Provider Identifier |
| `acceptingNewPatients` | Whether accepting new patients |
| `telehealthAvailable` | Telehealth availability |
| `gender` | Provider gender |
| `age` | Provider age |
| `yearsExperience` | Years since graduation |
| `aboutMe` | Provider bio |
| `profileUrl` | Full Healthgrades profile link |
| `insuranceCodes` | Insurance/payor codes |

## Input parameters

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `specialty` | string | `"doctor"` | Specialty to search (doctor, dentist, dermatologist, cardiologist, etc.) |
| `location` | string | `"Denver, CO"` | City and state |
| `insurance` | string |  | Optional insurance provider filter |
| `acceptingNewPatients` | boolean |  | Only show providers accepting new patients |
| `maxResults` | integer | `100` | Maximum results (up to 1000) |
| `proxyConfiguration` | object |  | Proxy settings |

## Example input

```
{
    "specialty": "dentist",
    "location": "New York, NY",
    "maxResults": 50
}
```

## Example output

```
{
    "displayName": "Dr. Jane Smith, DDS",
    "specialty": "Dentistry",
    "specialties": ["Dentistry", "Cosmetic Dentistry"],
    "overallRating": 4.2,
    "reviewCount": 48,
    "address": "123 Main St",
    "city": "New York",
    "state": "NY",
    "zip": "10001",
    "phone": "(212) 555-0100",
    "npi": "1234567890",
    "acceptingNewPatients": true,
    "telehealthAvailable": false,
    "gender": "Female",
    "profileUrl": "https://www.healthgrades.com/physician/dr-jane-smith-abc123"
}
```

## Supported specialties

- `doctor` — all physicians
- `dentist` — general dentistry
- `dermatologist` — skin care
- `cardiologist` — heart
- `pediatrician` — children
- `orthopedic-surgeon` — bones/joints
- `psychiatrist` — mental health
- `ophthalmologist` — eye care
- `neurologist` — brain/nervous system
- `urologist` — urinary tract
- `ob-gyn` — obstetrics/gynecology
- `gastroenterologist` — digestive system
- `pulmonologist` — lungs
- `endocrinologist` — hormones
- `oncologist` — cancer
- And many more...

## How it works

The scraper fetches Healthgrades search result pages and extracts provider data from the React Server Components (RSC) streaming payload. Healthgrades uses Next.js App Router with server-side rendering, embedding structured JSON data directly in the page HTML.

## Notes

- Results are limited to 20 per page (Healthgrades default)
- Some fields (board certification, education, languages) are only available on individual profile pages, not in search results
- Insurance codes are internal Healthgrades payor codes, not human-readable names
- The scraper respects rate limits with polite delays between page requests