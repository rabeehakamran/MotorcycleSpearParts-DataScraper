# Motorcycle Spare Parts — Business Data Scraper (Australia)

A Python-based web scraping pipeline that collects, enriches, and cleans business listing data from **Hotfrog Australia**. Built as part of a structured data collection assignment during an AI internship.

---

## What It Does

- Scrapes motorcycle spare parts business listings across **140 pages** of Hotfrog Australia search results
- Collects: **Company Name**, **Phone Number**, and **Location/Address**
- Enriches records with **email addresses** by querying the Clearbit Autocomplete API (for domain lookup) and Hunter.io API (for email discovery)
- Cleans and deduplicates the final dataset using Pandas and regex-based email validation
- Outputs a final structured CSV: `Australia.csv`

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| `Selenium` | Dynamic page rendering and element extraction |
| `Pandas` | Data merging, deduplication, and preprocessing |
| `re` (regex) | Email format validation |
| `Clearbit Autocomplete API` | Company domain lookup |
| `Hunter.io API` | Email discovery from domain |
| `time` / `random` | Rate limiting to avoid blocks |

---

## Pipeline Overview

```
Hotfrog Search Pages
        ↓
Selenium Scraper (batch-wise, 10 pages/batch × 14 batches)
        ↓
batch_1.csv ... batch_14.csv
        ↓
Merge → combined_scraped_data.csv (1680 raw records)
        ↓
Email Enrichment (Clearbit + Hunter.io)
        ↓
Data Cleaning (dedup, drop nulls, validate emails)
        ↓
Australia.csv (1411 clean records)
```

---

## Data Cleaning Steps

- Removed duplicate entries based on `Company Name`
- Dropped rows missing both `Company Name` and `Phone`
- Validated email format using regex: `^[\w\.-]+@[\w\.-]+\.\w+$`
- Reset DataFrame index after cleaning

> Raw records collected: **1,680** → After cleaning: **1,411**

---

## Output Format

| Column | Description |
|--------|-------------|
| `Company Name` | Business name |
| `Phone` | Contact number |
| `Location` | Address / suburb |
| `Email` | Email address (where discoverable) |

---

## Files

```
├── 25-0020-I.ipynb       # Full scraping + cleaning notebook
├── Australia.csv         # Final cleaned dataset
└── README.md
```

---

## Notes

- Random delays (`time.sleep`) were added between batches and API calls to respect rate limits
- Email discovery success was limited due to many small local businesses not having indexed domains
- Scraping was done on Hotfrog Australia; country selection was registered to avoid duplication with other students
