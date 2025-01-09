
# How to Build a News Crawler with the ScraperAPI

Imagine you're a developer who needs to track the latest news from multiple sources for a project. Instead of manually visiting each news website for updates, you can automate this process and save time by building a news crawler.

In this article, we'll guide you through creating a news crawler using Python Flask and the [ScraperAPI](https://bit.ly/Scraperapi). You'll learn how to set up ScraperAPI, implement crawling logic, and display the extracted news on a web page.

---

### Stop wasting time on proxies and CAPTCHAs!  
ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more.  
👉 [Start your free trial today!](https://bit.ly/Scraperapi)

---

## Prerequisites

To follow this tutorial, you'll need:

- Basic knowledge of Python and Flask.
- Python 3.11.0 and APScheduler 3.10.1. (Other versions might vary in functionality.)
- An API key for ScraperAPI, which you'll use for web scraping tasks.

---

## Setting Up Your Environment

### 1. Get Your ScraperAPI Key
Log in to your [ScraperAPI account](https://bit.ly/Scraperapi) and get your API key from the account dashboard.

### 2. Install Required Libraries
Set up your Python environment and install the necessary libraries:

```bash
# Create a project directory
mkdir news-crawler && cd news-crawler

# Create a virtual environment
python -m venv newsenv

# Activate the virtual environment
# On Mac/Linux
source newsenv/bin/activate
# On Windows
newsenv\Scripts\activate

# Install ScraperAPI library
pip install requests
```

---

### Stop wasting time on proxies and CAPTCHAs!  
ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more.  
👉 [Start your free trial today!](https://bit.ly/Scraperapi)

---

## Identifying Websites to Scrape

Before building the crawler, identify the websites you want to scrape. This tutorial uses CNN, NBC, and Yahoo. Each site has its own HTML structure, so you'll need to define specific `extract_rules` to capture news headlines.

Example `extract_rules`:

- **CNN**: Extract headlines from specific tags like `<h2>` or `<a>`.
- **NBC**: Adjust selectors to fit their HTML structure.
- **Yahoo**: Use different rules tailored to its unique layout.

### Sample URLs
Define the URLs of these sources in your code so your crawler can fetch data dynamically.

---

## Building the News Crawler

### 1. Command-Line Crawler
Start by creating a Python file, `news_crawler.py`, and writing the basic crawler logic:

```python
from requests import get

API_KEY = 'SCRAPE9837861'  # Replace with your ScraperAPI key
API_URL = 'https://api.scraperapi.com'

def fetch_news(url, extract_rules):
    response = get(
        API_URL,
        params={
            'api_key': API_KEY,
            'url': url,
            'render': 'false',
        }
    )
    return response.json()

# Define URLs and rules for CNN, NBC, and Yahoo
news_sources = {
    "CNN": {"url": "https://cnn.com", "extract_rules": {}},
    "NBC": {"url": "https://nbc.com", "extract_rules": {}},
    "Yahoo": {"url": "https://yahoo.com", "extract_rules": {}}
}

for source, details in news_sources.items():
    print(f"Fetching news from {source}...")
    headlines = fetch_news(details['url'], details['extract_rules'])
    print(headlines)
```

Run the script in your terminal:

```bash
python news_crawler.py
```

---

### Stop wasting time on proxies and CAPTCHAs!  
ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more.  
👉 [Start your free trial today!](https://bit.ly/Scraperapi)

---

### 2. Web Interface with Flask

Enhance your command-line crawler to display results on a web page using Flask.

#### Install Flask and APScheduler

```bash
pip install flask apscheduler
```

#### Flask Application Code

Create a new file, `app.py`, and write the following code:

```python
from flask import Flask, render_template
from apscheduler.schedulers.background import BackgroundScheduler

app = Flask(__name__)

# Store news headlines
news_headlines = {
    "CNN": [],
    "NBC": [],
    "Yahoo": []
}

def fetch_and_update_news():
    # Logic to scrape headlines from each source
    for source, details in news_sources.items():
        news_headlines[source] = fetch_news(details['url'], details['extract_rules'])

# Scheduler for periodic updates
scheduler = BackgroundScheduler()
scheduler.add_job(fetch_and_update_news, 'interval', minutes=10)
scheduler.start()

@app.route('/')
def display_news():
    return render_template(
        'news.html',
        headlines=news_headlines,
        last_updated="10 minutes ago"  # Replace with dynamic timestamp
    )

if __name__ == '__main__':
    fetch_and_update_news()  # Initial fetch
    app.run(debug=True)
```

#### HTML Template

Create a `templates` folder and add a `news.html` file:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>News Headlines</title>
  </head>
  <body>
    <h1>Latest News</h1>
    <p>Last Updated: {{ last_updated }}</p>

    {% for source, headlines in headlines.items() %}
      <h2>{{ source }}</h2>
      <ul>
        {% for headline in headlines %}
          <li>{{ headline }}</li>
        {% endfor %}
      </ul>
    {% endfor %}
  </body>
</html>
```

---

## Conclusion

Congratulations! You've built a Python-based news crawler that scrapes and displays headlines from multiple sources. By combining Flask and ScraperAPI, you've automated the news collection process and made it easy to display the data on a web page.

ScraperAPI simplifies crawling by managing rate limits, proxies, and CAPTCHAs for you. Explore its advanced features and capabilities by visiting the [ScraperAPI Documentation](https://bit.ly/Scraperapi).

Happy coding!
