
# Python Daily Practice (21): Scraping Asynchronous Data

## Introduction

In a previous article, [Python Daily Practice (15): Scraping Dynamically Loaded Data](https://bit.ly/Scraperapi), we discussed how to scrape dynamically loaded content. This article provides a more detailed explanation. Many websites display content that is not retrieved from the server in one go. For example, on image search websites, when scrolling down, new images continuously appear. These images are fetched asynchronously from the server. This process, common on sites like JD.com (Jingdong), is an example of asynchronous data loading.

---

**Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)**

---

## 1. Asynchronous Loading and AJAX

Traditional webpages required reloading the entire page to update dynamic content. This approach synchronized the loading of static and dynamic content. However, this caused issues like slow page loading if dynamic content was large or had errors. To solve this, asynchronous loading was introduced.

With asynchronous loading, static content (HTML, CSS, JavaScript, etc.) loads first, while dynamic parts are fetched asynchronously through separate server requests. This technique is called **AJAX** (Asynchronous JavaScript and XML).

![AJAX illustration](https://ask.qcloudimg.com/http-save/yehe-1011815/4cqp9b6e18.png)

### What is AJAX?

AJAX has two primary features:
1. **Asynchronous requests**: AJAX does not block the main thread. Even if data loads slowly, the page remains responsive.
2. **Data transmission format**: While AJAX initially relied on XML, JSON is now the standard due to its lightweight nature.

## 2. Basic Principles of AJAX

AJAX functionality involves three key steps:
1. **Sending requests**: Typically HTTP requests.
2. **Parsing responses**: Often JSON data.
3. **Rendering data**: Displaying the parsed data on specific elements of a webpage.

### 2.1 Sending Requests

For browser compatibility, it is recommended to use libraries like `jQuery` for sending requests. Download `jQuery` from its [official website](https://jquery.com/download/).

Here’s an example of sending an AJAX request:

```javascript
$.get("https://example.com/api", function(result) {
  // Process the server response
  console.log(result);
});
```

### 2.2 Parsing Responses

AJAX responses often come in JSON format. Use the following code to convert a JSON string into a JavaScript object:

```javascript
const jsonData = JSON.parse(result);
console.log(jsonData);
```

### 2.3 Rendering Data

Rendering involves appending fetched data to specific elements on the webpage. Here’s an example of appending data as `<li>` elements to a `<ul>`:

```javascript
$('#practice_list').append(`<li>${data.title}</li>`);
```

### 2.4 Simulating Asynchronous Loading with Flask

Using Flask, you can simulate a webpage with asynchronous loading. The project structure may look like this:

```
static/
templates/
app.py
```

Place your static files (like `jQuery`) under the `static/` directory and HTML templates under `templates/`.

Flask example for an asynchronous page:

```python
from flask import Flask, render_template, jsonify

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/data')
def data():
    return jsonify([
        {"id": 1, "name": "Item 1"},
        {"id": 2, "name": "Item 2"}
    ])

if __name__ == '__main__':
    app.run(debug=True)
```

## 3. Reverse Engineering

To identify whether a webpage uses asynchronous data loading, developers often use tools like Chrome Developer Tools. Here's how to analyze such pages:

### Step 1: Inspect the Page
Open the page in Chrome, right-click to inspect, and examine the elements. You’ll notice dynamically loaded elements in the `Elements` tab.

![Inspecting elements](https://ask.qcloudimg.com/http-save/yehe-1011815/yqj86xcvrb.png)

### Step 2: Monitor Network Activity
Switch to the `Network` tab in Developer Tools. Under the `XHR` filter, locate the API calls responsible for fetching dynamic content.

![Network XHR requests](https://ask.qcloudimg.com/http-save/yehe-1011815/trcue1ktjk.png)

### Step 3: Use XHR Filtering
Filter network requests using `XHR` to find asynchronous data requests. Analyze the response in the `Preview` tab to identify the API’s data structure.

![Preview of asynchronous data](https://ask.qcloudimg.com/http-save/yehe-1011815/8tbv7w001r.png)

## 4. Project: Scraping Dynamic Data from the National Medical Products Administration

Let’s implement scraping data from the **National Medical Products Administration (NMPA)** website. The homepage dynamically loads enterprise data via AJAX.

### Step 1: Analyze the Website

1. Inspect the network requests.
2. Identify the AJAX endpoint and payload.

### Step 2: Scrape AJAX Data

Use Python’s `requests` library to send POST requests to the identified endpoint. Parse the JSON response to extract data:

```python
import requests

url = "http://scxk.nmpa.gov.cn:81/xk/"
payload = {"id": "12345"}  # Example ID

response = requests.post(url, data=payload)
data = response.json()

print(data)
```

## Conclusion

By following the techniques discussed in this article, developers can efficiently scrape asynchronous data. Key steps include understanding AJAX principles, analyzing network requests, and implementing scraping using tools like Flask or Python.

**ScraperAPI** can further simplify your scraping needs by handling complex challenges like proxies and CAPTCHAs:

👉 [Start your free trial with ScraperAPI today!](https://bit.ly/Scraperapi)

The skills covered here empower developers to navigate dynamic websites, extract data reliably, and build robust scraping solutions.
