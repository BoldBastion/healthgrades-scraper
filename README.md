[Healthgrades Scraper](https://apify.com/jungle_synthesizer/healthgrades-scraper?fpr=data)

Scrape healthcare provider profiles from [Healthgrades.com](https://www.healthgrades.com). Extract doctor names, specialties, contact details, patient ratings, board certifications, hospital affiliations, and education history across all 50 U.S. states and 40 medical specialties.

## Healthgrades Scraper Features

- Searches Healthgrades by state, specialty, city, or ZIP code — any combination
- Supplies direct profile URLs when you already know which providers you want, bypassing search entirely
- Extracts structured data from both JSON-LD and rendered HTML, so fields are complete even when the page relies on JavaScript to load
- Deduplicates providers across paginated search results — the same doctor will not appear twice
- Paginates automatically up to 20 pages per search combination, then stops
- Formats phone numbers consistently as `(XXX) XXX-XXXX` across all records
- Uses US residential proxies with anti-detection measures — required for Healthgrades
- Pay-per-event pricing at roughly $0.002 per provider record

## Who Uses Healthgrades Doctor Data?

- **Healthcare marketers** — build targeted outreach lists by specialty and geography for medical device, pharma, and software sales
- **Referral network builders** — identify specialists by location and affiliation for patient referral programs
- **Healthcare analytics teams** — aggregate provider counts, rating distributions, and experience data by market
- **Medical staffing firms** — source candidate pools filtered by specialty, city, or hospital affiliation
- **Competitive intelligence analysts** — map provider density and ratings by region to identify underserved markets

## How Healthgrades Scraper Works

1. You provide states, specialty, city, or ZIP — or a list of direct profile URLs. If you provide direct URLs, search is skipped entirely.
2. The scraper uses a browser (Playwright) to load Healthgrades search pages and waits for the provider cards to render before extracting links. This is necessary because Healthgrades requires JavaScript to display results.
3. Each profile URL is enqueued and visited individually. The scraper pulls structured data from JSON-LD first, then fills gaps from rendered HTML, then extracts SPA-rendered fields like board certifications and languages using page evaluation.
4. Records are deduplicated by provider ID and saved to the dataset. The run stops when `maxItems` is reached.

## Input

### Search by state and specialty

```
{
  "states": ["CA", "TX"],
  "specialty": "cardiologist",
  "maxItems": 500,
  "sp_intended_usage": "Lead generation for cardiac device sales",
  "sp_improvement_suggestions": "n/a"
}
```

### Narrow by city

```
{
  "states": ["NY"],
  "specialty": "dermatologist",
  "city": "Brooklyn",
  "maxItems": 100,
  "sp_intended_usage": "Market sizing",
  "sp_improvement_suggestions": "n/a"
}
```

### Scrape specific provider profiles directly

```
{
  "providerUrls": [
    { "url": "https://www.healthgrades.com/physician/dr-jane-smith-abc123" },
    { "url": "https://www.healthgrades.com/dentist/dr-john-doe-xyz789" }
  ],
  "maxItems": 10,
  "sp_intended_usage": "Profile verification",
  "sp_improvement_suggestions": "n/a"
}
```

### Input Parameters

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| states | string[] | — | One or more U.S. state codes (e.g. `CA`, `TX`). Defaults to all states if empty. |
| specialty | string | All Specialties | Filter by medical specialty. See the full list below. Leave empty for all specialties. |
| city | string | — | Filter by city name. Works best when combined with a state filter. |
| zip | string | — | Filter by ZIP code. Healthgrades searches within a radius of the provided ZIP. |
| providerUrls | array | — | Direct Healthgrades profile URLs. When provided, bypasses search entirely. |
| maxItems | integer | `100` | Maximum provider records to return. Set to `0` for unlimited. |
| proxyConfiguration | object | US Apify proxy | Proxy settings. US residential proxies are required for reliable results. |
| sp_intended_usage | string | — | Required. Describe how you plan to use the data. |
| sp_improvement_suggestions | string | — | Required. Feedback on what could be improved. |

**Supported specialties:** Allergist, Anesthesiologist, Cardiologist, Chiropractor, Dentist, Dermatologist, Endocrinologist, Family Practice, Gastroenterologist, Geriatrician, Hematologist, Immunologist, Infectious Disease, Internist, Nephrologist, Neurologist, Neurosurgeon, OB/GYN, Oncologist, Ophthalmologist, Optometrist, Oral Surgeon, Orthopedic Surgeon, ENT, Pain Management, Pathologist, Pediatrician, Physiatrist, Plastic Surgeon, Podiatrist, Psychiatrist, Psychologist, Pulmonologist, Radiologist, Rheumatologist, Sports Medicine, Surgeon, Urologist, Vascular Surgeon.

## Healthgrades Scraper Output Fields

```
{
  "provider_name": "Dr. Jane Smith, MD",
  "first_name": "Jane",
  "last_name": "Smith",
  "title": "MD",
  "specialty": "Cardiologist",
  "address": "1234 Medical Center Blvd",
  "city": "Los Angeles",
  "state": "CA",
  "zip": "90024",
  "phone": "(310) 555-0182",
  "patient_rating": 4.7,
  "review_count": 143,
  "years_of_experience": 22,
  "education": [
    "Johns Hopkins University School of Medicine",
    "UCLA Medical Center - Residency"
  ],
  "hospital_affiliations": [
    "Cedars-Sinai Medical Center",
    "Ronald Reagan UCLA Medical Center"
  ],
  "board_certifications": [
    "American Board of Internal Medicine - Cardiovascular Disease"
  ],
  "languages": ["English", "Spanish"],
  "profile_url": "https://www.healthgrades.com/physician/dr-jane-smith-abc123"
}
```

| Field | Type | Description |
| --- | --- | --- |
| provider_name | string | Full name with professional title (e.g. `Dr. Jane Smith, MD`) |
| first_name | string | Provider first name |
| last_name | string | Provider last name |
| title | string | Professional credential (MD, DO, DDS, DMD, DPM, etc.) |
| specialty | string | Primary medical specialty |
| address | string | Practice street address |
| city | string | Practice city |
| state | string | Practice state abbreviation |
| zip | string | Practice ZIP code |
| phone | string | Practice phone number formatted as `(XXX) XXX-XXXX` |
| patient_rating | number | Patient satisfaction rating on a 0–5 scale |
| review_count | number | Number of patient reviews |
| years_of_experience | number | Years in medical practice |
| education | string[] | Medical school and residency history |
| hospital_affiliations | string[] | Affiliated hospitals |
| board_certifications | string[] | Board certifications with specialty designation |
| languages | string[] | Languages spoken by the provider |
| profile_url | string | Full Healthgrades profile URL |

## FAQ

**How many providers does the Healthgrades Scraper cover?**
Healthgrades indexes over 1.5 million healthcare providers across the U.S. The scraper can reach all of them given sufficient `maxItems` and state/specialty coverage. A national crawl across all specialties is a large job — plan accordingly.

**Why does this scraper require a browser instead of direct HTTP requests?**
Healthgrades renders search results and profile data using JavaScript. A plain HTTP request returns an empty shell. The scraper uses Playwright to load and execute the page before extracting data — which is why it requires more memory and time than an HTML-only crawler.

**Do I need residential proxies?**
Yes. Healthgrades actively blocks datacenter IP ranges. The scraper defaults to US Apify residential proxies, which is the correct configuration. Running without residential proxies will result in blocks and empty results.

**How long does a typical run take?**
A run of 500 records across a single state and specialty takes roughly 15–30 minutes depending on proxy latency and page load times. Larger national crawls with multiple specialties can run for several hours. Set `timeoutSecs` to at least 14400 (4 hours) for large jobs.

**Can I scrape a specific list of doctors?**
Yes. Provide their Healthgrades profile URLs directly in `providerUrls`. The scraper will visit each URL directly and skip search entirely — faster and more precise for known targets.

**What data is available on every profile vs. only some?**
`provider_name`, `specialty`, `city`, `state`, `phone`, and `profile_url` are available on nearly all profiles. Fields like `board_certifications`, `hospital_affiliations`, `years_of_experience`, and `languages` depend on what each provider has filled out on Healthgrades — not every profile has all fields populated.

**Is it legal to scrape Healthgrades?**
The scraper collects publicly available information from Healthgrades. You are responsible for ensuring your use of the data complies with applicable laws and regulations, including HIPAA if you are processing data in a healthcare context. This scraper does not access patient records or any protected health information.

**How much does it cost to run?**
Pricing is pay-per-event at approximately $0.002 per provider record. A run of 1,000 providers costs roughly $2. Actual costs depend on run time and memory usage — check your Apify usage dashboard for exact figures.

## Need More Features?

Need a custom specialty filter, additional output fields, or a bulk national dataset? [Get in touch](https://apify.com/jungle_synthesizer/healthgrades-scraper).

## Why Use Healthgrades Scraper?

- **Browser-based extraction** — Handles the JavaScript-rendered search results and profile pages that defeat simpler scrapers. Data is pulled from structured JSON-LD where available, with HTML fallback for completeness.
- **Direct URL mode** — Skip search entirely when you have a known list of providers. Useful for verification, enrichment, and monitoring workflows.
- **Priced per record** — Pay only for the data you actually collect. No monthly subscription, no minimum commitment.