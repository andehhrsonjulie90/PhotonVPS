
# Crawling a JSON REST API: A Comprehensive Tutorial

## Introduction

This tutorial demonstrates how to crawl a JSON REST API using the REST Crawler in OpenSearchServer, and how to set up an engaging search page for your indexed data. 

As an example, we'll crawl the **YouTube API** to search for videos and playlists based on specific criteria.

Here's an example of the end result:

![Example of final results](https://www.opensearchserver.com/documentation/tutorials/oss-json-api-final.png)

---

**Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)**

---

## 1. YouTube API Overview

For this tutorial, we’ll use the **Search:list** endpoint from the YouTube V3 API. The API provides rich video and playlist data.

### Parameters Used
Here are the parameters for our example:
- **`part`**: `id,snippet` (fetch metadata like title, description, and thumbnails)
- **`q`**: `washington` (search results related to this keyword)
- **`order`**: `date` (results sorted by most recent first)
- **`maxResults`**: `50` (fetch up to 50 results per request)

### Final URL
```plaintext
https://www.googleapis.com/youtube/v3/search?order=date&part=id,snippet&q=washington&maxResults=50&key=Your_API_Key
```

> Replace `Your_API_Key` with your valid API key for Google APIs.

### Example API Response (Truncated)
```json
{
  "kind": "youtube#searchListResponse",
  "items": [
    {
      "id": {
        "kind": "youtube#video",
        "videoId": "VeYQxs331aE"
      },
      "snippet": {
        "title": "2007 Pontiac G6 Used Cars Shepherdsville",
        "description": "This 2007 Pontiac G6 is available from Craig and Landreth Cars.",
        "thumbnails": {
          "default": {
            "url": "https://i.ytimg.com/vi/VeYQxs331aE/default.jpg"
          }
        }
      }
    }
  ]
}
```

The `items` array contains objects, each representing a video or playlist. These objects will form individual documents in our index.

---

## 2. Setting Up the Schema

To begin, create an empty index named `youtube`. Then, configure the schema to match the fields you want to extract from the JSON response.

### Fields to Add
- **`title`**: Video or playlist title
- **`etag`**: Unique identifier for the document
- **`description`**: Description of the video or playlist
- **`thumbnail`**: URL of the video thumbnail

### Default and Unique Fields
- Default field: `title`
- Unique field: `etag`

The unique field ensures that documents are updated rather than duplicated during crawls.

---

## 3. Configuring the REST Crawler

### Step 1: Create a New Crawl
Navigate to the `Crawler` > `REST` tab and create a new crawl. Configure the following settings:

- **Name**: `YouTube`
- **Language**: `English`
- **URL**: Paste the YouTube API URL.
- **HTTP Method**: `GET`
- **Path to Documents**: Use the JSONPath expression `$.items` to point to the `items` array.

![Configuring the REST Crawler](https://www.opensearchserver.com/documentation/tutorials/oss-json-api-crawler1.png)

### Step 2: Map Fields
In the `FieldMap` tab, map the JSON keys to your schema fields. For example:
- `$.snippet.title` → `title`
- `$.etag` → `etag`
- `$.snippet.description` → `description`
- `$.snippet.thumbnails.default.url` → `thumbnail`

### Step 3: Run the Crawler
Click the green "Run" button to start the crawl. If successful, the status will display "Complete."

---

## 4. Creating a Search Query

Once the data is indexed, create a query to retrieve relevant documents.

### Configuring the Query
- Query type: `Search (field)`
- Default search field: `title`
- Return fields: `title`, `description`, `thumbnail`
- Enable snippets to highlight search keywords.

![Configuring the Query](https://www.opensearchserver.com/documentation/tutorials/oss-json-api-query1.png)

Test the query to ensure it works as expected.

---

## 5. Setting Up a Renderer

To display the results, configure a renderer.

### Global Configuration
- Use the query template you created earlier.
- Add JavaScript and CSS to enhance the appearance of the results.

### Example JavaScript for Display
```html
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
<script>
  jQuery(function($) {
    $('.oss-one-result').each(function() {
      var kind = $(this).find('.oss-kind-video').html().trim();
      var videoURL = "https://www.youtube.com/watch?v=" + $(this).find('.oss-video-id').html();
      $(this).find('.oss-video-id').html(`<a href="${videoURL}" target="_blank">Watch Video</a>`);
    });
  });
</script>
```

### Example CSS for Styling
```css
body {
  font-family: Arial, sans-serif;
  background: #f9f9f9;
}
.oss-one-result {
  background: #fff;
  border: 1px solid #ddd;
  padding: 15px;
  margin-bottom: 10px;
}
```

---

## 6. Automating the Crawl

You can schedule crawls to run periodically:
1. Go to the `Scheduler` tab.
2. Create a new job for the `REST crawler - run` task.
3. Use variables in the API URL to search for different keywords dynamically.

---

## Conclusion

With these steps, you can efficiently crawl a JSON REST API, index its data, and create an engaging search interface. 

---

**Ready to take your web scraping projects to the next level?** ScraperAPI simplifies your workflow with powerful features like proxy management and CAPTCHA handling. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)
