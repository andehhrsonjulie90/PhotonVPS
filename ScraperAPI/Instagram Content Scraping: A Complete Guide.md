
# Instagram Content Scraping: A Complete Guide

## Overview

This guide provides a detailed breakdown of how to scrape content from Instagram using cookies, user agents, and query hashes. Whether you're looking to scrape all posts from a user or gather content under a specific tag, this tutorial covers the necessary steps, API endpoints, and essential code to automate the process.

---

> **Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Instagram, Amazon, Google, Walmart, and more. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)**

---

## Instructions for Scraping Instagram

### Prerequisites

1. **Login Required**: Scraping Instagram requires login credentials, which are used to generate a `cookie` and must be accompanied by a `user-agent`.
2. **Request Rate Limits**: Instagram imposes request limits. Scraping hundreds of resources without delays may lead to restrictions. If restricted, pause the requests for a certain duration before continuing.
3. **Content Entry Points**:
   - Scrape all posts published by a specific user.
   - Scrape all posts under a specific tag.

These two methods use different `cookies` and request URLs.

---

### Step-by-Step Process

1. **Log in to Instagram**  
   Log in from the desktop and save the following details:
   - `cookie`
   - `query_hash`
   - `user-agent`

   These details will be attached to all subsequent requests.

2. **Simulate Requests**  
   Request the user's homepage or tag page and parse the HTML to:
   - Retrieve the `userId` or tag name.
   - Fetch the first page of data along with the cursor for subsequent pages.

3. **Fetch Multiple Pages**  
   Use the API endpoint to retrieve additional pages using the `cursor`. Once all data is collected, start downloading the content.

4. **Download Resources**  
   - **Images**: Download directly using the URLs provided in the response.  
   - **Videos**: Make additional requests to retrieve video URLs, then download them.

---

### API Endpoints

- **User Posts**:  
  ```plaintext
  https://www.instagram.com/graphql/query/?query_hash=a5164aed103f24b03e7b7747a2d94e3c&variables={"id":"%s","first":${purePage},"after":"%s"}
  ```

- **Tag Posts**:  
  ```plaintext
  https://www.instagram.com/graphql/query/?query_hash=1780c1b186e2c37de9f7da95ce41bb67&variables={"tag_name":"%s","first":${purePage},"after":"%s"}
  ```

- **Video URL**:  
  ```plaintext
  https://www.instagram.com/p/%s/?__a=1
  ```

---

## Key Code Implementation

### Fetch User or Tag HTML

```javascript
const getHtml = (item) => {
  let userName = item.name;
  let type = item.type;
  let url;

  if (type === 'user') {
    url = `${baseUrl}${userName}/`;
    headers.cookie = userCookie;
  } else {
    url = `${baseUrl}explore/tags/${userName}/`;
    headers.cookie = tagCookie;
  }

  const options = {
    method: 'GET',
    url: url,
    headers: headers,
  };

  return new Promise((resolve, reject) => {
    request(options, (error, response, body) => {
      if (error) return reject(error);

      const $ = cheerio.load(body);
      let html = $.html();

      // Extract userId or tag name
      const userId =
        type === 'user'
          ? html.match(/"profilePage_([0-9]+)"/)[1]
          : html.match(/"name":"([a-zA-Z_]+)",/)[1];
      console.log(`ID/Tag successfully retrieved: ${userId}`);

      // Parse first page data
      const data = JSON.parse(
        html.match(/<script type="text\/javascript">window._sharedData = (.*?);<\/script>/)[1]
      );

      const firstPageData =
        type === 'user'
          ? data.entry_data.ProfilePage[0].graphql.user.edge_owner_to_timeline_media
          : data.entry_data.TagPage[0].graphql.hashtag.edge_hashtag_to_media;

      const edges = firstPageData.edges;
      const count = firstPageData.count;
      const pageInfo = firstPageData.page_info;

      // Store first page media data
      edges.forEach((item) => storeMedia(item));

      resolve({
        totalPage: Math.ceil(count / purePage),
        userId: userId,
        cursor: pageInfo.end_cursor,
      });
    });
  });
};
```

---

### Fetch All User Content

```javascript
const getAllUrls = (item, totalPage, uid, cursor) => {
  let actions = [async.constant(item, uid, cursor)];
  let limit = totalPage > pageLimit ? pageLimit : totalPage;

  for (let i = 0; i < limit; i++) {
    actions.push(fetchData);
  }

  return new Promise((resolve, reject) => {
    async.waterfall(actions, (error, result) => {
      console.log(
        `All posts retrieved successfully. Total posts: ${media.length}, Videos: ${videoCount}, Images: ${imgCount}`
      );
      resolve(media);
    });
  });
};
```

---

### Fetch Video URL

```javascript
const fetchVideoUrl = (mode, shortcode) => {
  const url = util.format(getVideoUrl, shortcode);

  if (mode === 'user') {
    headers.cookie = userCookie;
  } else {
    headers.cookie = tagCookie;
  }

  const options = {
    method: 'GET',
    url: url,
    headers: headers,
  };

  return new Promise((resolve, reject) => {
    request(options, (error, response, body) => {
      let videoUrl = '';
      if (error) {
        console.error(`Failed to fetch video URL for shortcode: ${shortcode}`);
        resolve(videoUrl);
      }

      try {
        const data = JSON.parse(body);
        videoUrl = data.graphql.shortcode_media.video_url;
      } catch (error) {
        console.error(`Failed to parse video URL for shortcode: ${shortcode}`);
      }

      resolve(videoUrl);
    });
  });
};
```

---

### Download Media

```javascript
const download = (category, media, next) => {
  const filePath =
    media.type === 'video'
      ? `${videoDlPath}/${media.id}.mp4`
      : `${imgDlPath}/${media.id}.jpg`;

  request(media.url)
    .pipe(fs.createWriteStream(filePath))
    .on('finish', () => {
      console.log(`Downloaded ${media.type}: ${filePath}`);
      next(null);
    })
    .on('error', (err) => {
      console.error(`Failed to download ${media.type}: ${filePath}`, err);
      next(null);
    });
};
```

---

### Error Handling and Timeouts

Include timeouts to avoid bans:
```javascript
setTimeout(() => {
  return next(null, item, uid, offset);
}, 2000);
```

Retry on failure:
```javascript
if (data.status === 'fail') {
  console.error('API response failed. Retrying after 1 minute...');
  return setTimeout(() => next(null, item, uid, offset), 60000);
}
```

---

## Summary

By following this guide, you can build a robust Instagram scraping tool to fetch content based on users or tags. The outlined steps, API endpoints, and code examples provide a strong foundation for automating data collection while adhering to request limits and avoiding detection.

👉 **[Start your free trial with ScraperAPI today!](https://bit.ly/Scraperapi)**
