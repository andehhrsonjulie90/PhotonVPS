
# How to Use Google Sheets to Scrape Product Page Data and Build Shopping Ad Feeds

## Introduction

Have you ever needed to extract data from websites? For example, when creating a new shopping ad campaign, you might lack a product feed to quickly build the ads. Ideally, we'd have the required content like landing pages and product names available in an easy-to-import format like Excel or Google Sheets. However, many advertisers unfamiliar with data scraping often end up copying and pasting hundreds or thousands of product details manually.

Recently, I was tasked with downloading data for around 150 products from 15 client website pages. Manually copying each product's name and landing page URL into a spreadsheet would have been time-consuming and error-prone. Fortunately, by using **ImportXML** in Google Sheets, the task was completed efficiently and accurately.

---

**Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)**

---

## Table of Contents

1. [What is ImportXML?](#what-is-importxml)
2. [How Does ImportXML Help Scrape Web Elements?](#how-does-importxml-help-scrape-web-elements)
3. [Google Sheet + ImportXML Case Demonstration](#google-sheet--importxml-case-demonstration)
4. [Step 1: Create a New Google Sheet](#step-1-create-a-new-google-sheet)
5. [Step 2: Add URLs of the Pages to Scrape](#step-2-add-urls-of-the-pages-to-scrape)
6. [Step 3: Locate XPath](#step-3-locate-xpath)
7. [Step 4: Extract Data into Google Sheets](#step-4-extract-data-into-google-sheets)
8. [Conclusion](#conclusion)

---

## What is ImportXML?

According to [Google's explanation](https://support.google.com/docs/answer/3093342?hl=en), ImportXML allows you to fetch structured data, including XML, HTML, CSV, TSV, RSS, and Atom feeds, from a webpage.

Essentially, ImportXML is a Google Sheets function that lets you scrape structured data from web pages, such as text, metadata, and more, without needing any coding knowledge.

---

## How Does ImportXML Help Scrape Web Elements?

The function requires just two parameters:

- **URL:** The webpage from which you want to scrape information.
- **XPath:** A language to navigate and identify elements in an XML document, used here to locate the data on the webpage.

### Example

To extract the title from the Wikipedia page on Moon landing, enter the following formula in a Google Sheets cell:

```plaintext
=IMPORTXML("https://en.wikipedia.org/wiki/Moon_landing", "//title")
```

The result: `Moon landing – Wikipedia`.

If you want to retrieve the meta description of a page, use:

```plaintext
=IMPORTXML("https://example.com/", "//meta[@name='description']/@content")
```

#### Common XPath Queries

Here are some commonly used XPath queries:

- **Page Title:** `//title`
- **Meta Description:** `//meta[@name='description']/@content`
- **H1 Tags:** `//h1`
- **Links on the Page:** `//@href`

---

## Google Sheet + ImportXML Case Demonstration

Google Sheets' ImportXML function is a secret weapon for many everyday tasks. When combined with other formulas, it can handle more advanced data tasks that might otherwise require Python or similar tools.

### Case: Scraping Article Data for Ads

Imagine you're tasked with creating a content ad campaign featuring the 30 latest articles in a specific category. Instead of manually collecting data from each page, you can automate the process with ImportXML.

---

## Step 1: Create a New Google Sheet

Begin by opening a blank Google Sheets document.

![Create a new Google Sheets document.](https://tomhello.com/wp-content/uploads/2021/08/%E4%BB%8E%E7%A9%BA%E7%99%BD%E7%9A%84-Google-Sheets-%E6%96%87%E6%A1%A3%E5%BC%80%E5%A7%8B%E3%80%82.png)

---

## Step 2: Add URLs of the Pages to Scrape

Input the URLs of the pages from which you want to scrape data. For instance, add the URL for articles under the "Pay-Per-Click" category on Search Engine Journal.

![Add URLs of the pages to scrape.](https://tomhello.com/wp-content/uploads/2021/08/%E6%B7%BB%E5%8A%A0%E8%A6%81%E6%8A%93%E5%8F%96%E7%9A%84%E9%A1%B5%E9%9D%A2%E7%9A%84-URL%E3%80%82.png)

---

## Step 3: Locate XPath

Find the elements you want to scrape, such as article titles, and copy their XPath paths.

1. Open Chrome DevTools by right-clicking an element and selecting **Inspect**.
2. Highlight the desired element in the DevTools window.
3. Right-click the highlighted HTML and choose **Copy > Copy XPath**.

![Locate and copy the XPath.](https://tomhello.com/wp-content/uploads/2021/08/%E6%9F%A5%E6%89%BE%E5%B9%B6%E5%A4%8D%E5%88%B6%E8%A6%81%E6%8F%90%E5%8F%96%E7%9A%84-XPath-%E5%85%83%E7%B4%A0%E3%80%82.png)

---

## Step 4: Extract Data into Google Sheets

Back in Google Sheets, use the **ImportXML** function to extract data:

```plaintext
=IMPORTXML(B1, "//*[starts-with(@id, 'title')]")
```

### Key Notes:

1. In the example formula, the URL in cell `B1` is referenced as the first parameter.
2. Modify XPath to ensure it works for dynamic elements. For instance, replace double quotes with single quotes to avoid syntax errors in formulas.

#### Example Output:

Titles from the webpage are populated in the spreadsheet.

![Extracted article titles.](https://tomhello.com/wp-content/uploads/2021/08/%E5%9C%A8-Google-%E8%A1%A8%E6%A0%BC%E4%B8%AD%E5%AF%BC%E5%85%A5%E7%9A%84%E6%96%87%E7%AB%A0%E5%92%8C-URL%E3%80%82.png)

### Extracting Additional Data

To extract URLs:

```plaintext
=IMPORTXML(B1, "//*[starts-with(@id, 'title')]/@href")
```

To extract summaries, authors, or other data, adjust the XPath accordingly.

![Extracted article summaries and authors.](https://tomhello.com/wp-content/uploads/2021/08/%E6%89%80%E6%9C%89%E6%95%B0%E6%8D%AE%E9%83%BD%E8%A2%AB%E6%8A%93%E5%8F%96%E5%B9%B6%E5%AF%BC%E5%85%A5%E5%88%B0-Google-%E8%A1%A8%E6%A0%BC%E4%B8%AD%E3%80%82.png)

---

## Conclusion

Whether you're extracting article data or other elements like product prices or shipping costs, **ImportXML** automates the process efficiently with minimal error. This tool is particularly useful for PPC campaigns, SEO analysis, and bulk data collection.

While ImportXML in Google Sheets is free and user-friendly, it has limitations, such as scraping quotas set by Google to prevent abuse. For more advanced needs or higher data volumes, consider tools like Python-based scraping.

Streamline your data scraping workflows and save valuable time with these simple but powerful techniques!
