
# How to Scrape Walmart Product Data with Node.js

## Introduction

In this tutorial, we’ll show you how to build a Walmart web scraper from scratch using **Node.js**. You'll learn how to extract product data while bypassing Walmart's anti-bot detection. This scraper will allow you to access dynamic content efficiently without harming the server.

---

**Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)**

---

## TL;DR: Full Walmart Node.js Scraper

For those in a hurry, here’s the full code you can use to scrape Walmart data.

```javascript
const axios = require('axios');
const cheerio = require('cheerio');

const WALMART_PAGE_URL = 'https://walmart.com/search?q=computer+screen';
const API_URL = 'https://api.scraperapi.com';
const API_KEY = ''; // Enter your API key here

const parseProductReview = (inputString) => {
  const ratingRegex = /(\d+\.\d+)/;
  const reviewCountRegex = /(\d+) reviews/;

  const ratingMatch = inputString.match(ratingRegex);
  const reviewCountMatch = inputString.match(reviewCountRegex);

  const rating = ratingMatch?.length > 0 ? parseFloat(ratingMatch[0]) : null;
  const reviewCount = reviewCountMatch?.length > 1 ? parseInt(reviewCountMatch[1]) : null;

  return { rating, reviewCount };
};

const webScraper = async () => {
  const queryParams = new URLSearchParams({
    api_key: API_KEY,
    url: WALMART_PAGE_URL,
    render: true,
  });

  try {
    const response = await axios.get(`${API_URL}?${queryParams.toString()}`);
    const html = response.data;

    const $ = cheerio.load(html);
    const productList = [];

    $("div[data-testid='list-view']").each((_, el) => {
      const link = $(el).prev('a').attr('href');
      const price = $(el).find('.f2').text();
      const priceCents = $(el).find('.f2 + span.f6').text();
      const description = $(el).find("span[data-automation-id='product-title']").text();
      const reviews = $(el).find('.w_iUH7').last().text();
      const delivery = $(el).find("div[data-automation-id='fulfillment-badge']").find('span.b').last().text();

      const { rating, reviewCount } = parseProductReview(reviews);

      productList.push({
        description,
        price: `$${price}.${priceCents}`,
        averageRating: rating,
        totalReviews: reviewCount,
        delivery,
        link: `https://www.walmart.com${link}`
      });
    });

    console.log(productList);
  } catch (error) {
    console.error(error.response.data);
  }
};

void webScraper();
```

### Getting Started

Before running the script:

1. Install the required dependencies (`axios` and `cheerio`).
2. Insert your API key from the [ScraperAPI dashboard](https://bit.ly/Scraperapi).
3. Run the script with Node.js.

---

## Scraping Walmart Product Data in Node.js

To demonstrate scraping Walmart, we’ll create a script that searches for **computer screens** and extracts the following information:

- Product name
- Price
- Average rating
- Total number of reviews
- Delivery options
- Product link

The data will be exported as **JSON**, making it easy to use for further analysis.

---

## Prerequisites

Before starting, ensure you have the following:

- **Node.js (v18+) and npm**: Download and install from [Node.js official site](https://nodejs.org/en/download).
- **Basic JavaScript knowledge**: Understanding of Node.js and APIs.
- **ScraperAPI account**: Sign up and get [5,000 free API credits](https://bit.ly/Scraperapi).

---

## Step 1: Set Up the Project

1. Create a new project folder:

   ```bash
   mkdir walmart-scraper
   cd walmart-scraper
   ```

2. Initialize a new Node.js project:

   ```bash
   npm init -y
   ```

3. Create an `index.js` file:

   ```bash
   touch index.js
   ```

4. Add a simple `console.log` statement in `index.js` to test the setup:

   ```javascript
   console.log("Hello, Walmart Scraper!");
   ```

5. Run the script:

   ```bash
   node index.js
   ```

You should see `Hello, Walmart Scraper!` in your terminal.

---

## Step 2: Install Dependencies

Install the following packages:

- **Axios**: For making HTTP requests.
- **Cheerio**: For parsing and extracting data from the HTML.

```bash
npm install axios cheerio
```

---

## Step 3: Identify the DOM Selectors

Navigate to [Walmart](https://www.walmart.com) and search for "computer screen." Inspect the search result page to identify the relevant DOM elements.

For example:

- **Product title selector**: `span[data-automation-id='product-title']`
- **Price selector**: `.f2`
- **Reviews selector**: `.w_iUH7`
- **Delivery details selector**: `div[data-automation-id='fulfillment-badge']`

### Pro Tip: Test Selectors with jQuery

Use the browser console to test your selectors:

```javascript
$("span[data-automation-id='product-title']")
```

---

## Step 4: Build the Walmart Scraper

Using the DOM selectors, update the `index.js` script to fetch Walmart product data. Here's a breakdown:

### Scraper Configuration

- **API Key**: Use your ScraperAPI key to authenticate.
- **JS Rendering**: Walmart’s site relies on JavaScript, so enable rendering in your API requests.

### Scraper Code

The scraper will:

1. Send an HTTP request to ScraperAPI with the Walmart URL.
2. Parse the HTML response using Cheerio.
3. Extract product details (name, price, reviews, etc.).
4. Export the data in JSON format.

---

## Conclusion

Scraping Walmart product data with Node.js becomes seamless using ScraperAPI and tools like Axios and Cheerio. This setup enables you to extract dynamic web content efficiently while bypassing anti-bot mechanisms.

Ready to automate your data scraping tasks? Start building your scraper today with [ScraperAPI](https://bit.ly/Scraperapi) and make data collection hassle-free!
