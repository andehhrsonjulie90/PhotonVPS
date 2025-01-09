
# How to Scrape Walmart Search Results Efficiently

## Introduction

In this blog post, we'll guide you through extracting **filters**, **featured items**, **related queries**, **organic results**, and handling pagination using Python and the Walmart Search Engine Results API (SERP API). This comprehensive tutorial demonstrates how to streamline data extraction with an API to avoid the complexities of manual parsing.

---

**Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)**

---

## Why Use an API?

APIs provide several advantages, making data extraction more efficient and reliable:

- **No need to create a parser**: Eliminate the hassle of maintaining web scrapers.
- **Bypass anti-bot mechanisms**: APIs solve CAPTCHAs and IP blocks for you.
- **Cost-effective solutions**: Save on proxies, CAPTCHA solvers, and browser automation tools.
- **Fast response times**: APIs like SerpApi offer response times as low as ~2.5 seconds.

By utilizing an API, you focus on data analysis rather than the technical challenges of web scraping.

---

## What Will Be Scraped?

This tutorial focuses on extracting Walmart search results, including:

- Search filters
- Featured items
- Related queries
- Organic results
- Pagination

### Sample Data Display

By default, Walmart search results return 40 results per page. For demonstration purposes, we'll work with a smaller dataset for clarity.

![Walmart Search Results](https://user-images.githubusercontent.com/81998012/206739706-6f46e9ab-260b-45c4-8ada-820b4ac1126f.png)

---

## Full Python Code for Scraping Walmart Search Results

Here's the complete Python script to scrape Walmart search results with pagination:

```python
from serpapi import GoogleSearch
from urllib.parse import urlsplit, parse_qsl
import json

params = {
    'api_key': 'YOUR_API_KEY',  # Replace with your SerpApi API key
    'engine': 'walmart',       # Specify Walmart as the search engine
    'query': 'coffee maker',   # Search query
    'spelling': True,          # Enable spelling corrections
    'sort': 'best_match',      # Sort results by relevance
    'min_price': 100,          # Minimum price filter
    'max_price': 150,          # Maximum price filter
}

search = GoogleSearch(params)
results = search.get_dict()

walmart_results = {
    'search_information': results.get('search_information'),
    'filters': results.get('filters'),
    'organic_results': [],
    'featured_item': results.get('featured_item'),
    'related_queries': results.get('related_queries'),
}

while 'next' in results.get('serpapi_pagination', {}):
    walmart_results['organic_results'].extend(results['organic_results'])
    search.params_dict.update(dict(parse_qsl(urlsplit(results.get('serpapi_pagination', {}).get('next')).query)))
    results = search.get_dict()

print(json.dumps(walmart_results, indent=2, ensure_ascii=False))
```

---

## Step-by-Step Code Breakdown

### 1. Install Required Libraries

Install the required Python package for SerpApi:

```bash
pip install google-search-results
```

### 2. Import Libraries

```python
from serpapi import GoogleSearch
from urllib.parse import urlsplit, parse_qsl
import json
```

### 3. Define Parameters

Parameters for the Walmart Search API:

```python
params = {
    'api_key': 'YOUR_API_KEY',
    'engine': 'walmart',
    'query': 'coffee maker',
    'spelling': True,
    'sort': 'best_match',
    'min_price': 100,
    'max_price': 150,
}
```

Replace `'YOUR_API_KEY'` with your SerpApi API key.

### 4. Retrieve Initial Search Results

```python
search = GoogleSearch(params)
results = search.get_dict()
```

This initializes the search object and retrieves JSON-formatted data from the Walmart search results.

### 5. Handle Pagination

To extract results across all pages, use a `while` loop to check if the next page exists:

```python
while 'next' in results.get('serpapi_pagination', {}):
    walmart_results['organic_results'].extend(results['organic_results'])
    search.params_dict.update(dict(parse_qsl(urlsplit(results.get('serpapi_pagination', {}).get('next')).query)))
    results = search.get_dict()
```

This loop adds results from the current page to the `walmart_results['organic_results']` list and updates the parameters for the next page.

---

## Extracting Data Fields

To retrieve specific fields from the organic results, use the following structure:

```python
# Example of extracting individual fields
title = results['organic_results'][0]['title']
thumbnail = results['organic_results'][0]['thumbnail']
rating = results['organic_results'][0]['rating']
reviews = results['organic_results'][0]['reviews']
price = results['organic_results'][0]['primary_offer']['offer_price']
```

Here, `results['organic_results'][0]` refers to the first product, while subsequent indices correspond to other products.

---

## Advantages of Using SerpApi

1. **Simplified Pagination**: SerpApi automatically handles pagination through its API.
2. **Real-Time Spell Check**: Automatically correct misspelled queries.
3. **Fast and Reliable**: API requests are processed efficiently without the need for browser emulation.

---

## Example Output in JSON

Once the script runs successfully, it outputs the Walmart search results in a structured JSON format, like this:

```json
{
  "search_information": {
    "query": "coffee maker",
    "total_results": 120
  },
  "filters": [...],
  "organic_results": [
    {
      "title": "Coffee Maker 12-Cup",
      "price": "$99.99",
      "rating": 4.5,
      "reviews": 345
    },
    ...
  ],
  "featured_item": {...},
  "related_queries": [...]
}
```

---

## Conclusion

Scraping Walmart search results has never been easier with SerpApi's Walmart Search Engine Results API. It streamlines the process of extracting data, managing pagination, and parsing structured JSON results.

---

**Simplify your web scraping workflow with ScraperAPI. Focus on your data while we handle proxies, CAPTCHAs, and dynamic rendering. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)**
