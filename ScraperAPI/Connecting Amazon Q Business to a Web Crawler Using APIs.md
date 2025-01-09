
# Connecting Amazon Q Business to a Web Crawler Using APIs

## Introduction

Connecting Amazon Q Business to a web crawler using the Amazon Q API can streamline your data integration and retrieval processes. This guide provides step-by-step instructions on configuring and leveraging the Amazon Q API for effective crawling and indexing.

---

**Stop wasting time on proxies and CAPTCHAs! ScraperAPI’s simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)**

---

## API Overview and Configuration

To connect Amazon Q Business to a web crawler, use the `CreateDataSource` API. This API enables you to configure the crawling process, including specifying seed URLs, sitemap URLs, authentication credentials, and other essential properties.

For detailed parameter information, refer to the [CreateDataSource API documentation](https://docs.aws.amazon.com/amazonq/latest/api-reference/API_CreateDataSource.html).

### Key Configuration Properties

The following are critical configuration properties required in the schema:

| **Configuration**          | **Description**                                                                                         | **Type**         | **Required** |
|-----------------------------|---------------------------------------------------------------------------------------------------------|------------------|--------------|
| `type`                     | The type of data source. Options include `WEBCRAWLERV2` (Recommended) and `WEBCRAWLER`.                 | `string`         | Yes          |
| `syncMode`                 | Specifies whether to sync all documents or only new/modified/deleted documents. Options: `FORCED_FULL_CRAWL`, `FULL_CRAWL`. | `string`         | Yes          |
| `connectionConfiguration`  | Configuration information for the endpoint. Includes sub-properties like `repositoryEndpointMetadata`.  | `object`         | Yes          |
| `authentication`           | Authentication type for websites requiring it. Options: `NoAuthentication`, `BasicAuth`, `SAML`, etc.  | `string`         | Yes          |
| `seedUrlConnections`       | List of up to 100 seed URLs as starting points for crawling.                                             | `array`          | No           |
| `siteMapUrls`              | List of up to 3 sitemap URLs for crawling.                                                              | `array`          | No           |
| `repositoryConfigurations` | Defines specific content types and field mappings.                                                      | `object`         | Yes          |

---

## Web Crawler Configuration

### Example JSON Schema

Below is an example of the Web Crawler JSON schema for configuring Amazon Q API:

```json
{
  "type": "WEBCRAWLERV2",
  "syncMode": "FULL_CRAWL",
  "secretArn": "arn:aws:secretsmanager:us-west-2:0123456789012:secret",
  "connectionConfiguration": {
    "repositoryEndpointMetadata": {
      "siteMapUrls": ["https://example.com/sitemap.xml"],
      "seedUrlConnections": [
        {
          "seedUrl": "https://example.com"
        }
      ],
      "authentication": "NoAuthentication"
    }
  },
  "repositoryConfigurations": {
    "webPage": {
      "fieldMappings": [
        {
          "indexFieldName": "title",
          "indexFieldType": "STRING",
          "dataSourceFieldName": "page_title",
          "dateFieldFormat": "yyyy-MM-dd'T'HH:mm:ss'Z'"
        }
      ]
    },
    "attachment": {
      "fieldMappings": [
        {
          "indexFieldName": "attachment_title",
          "indexFieldType": "STRING",
          "dataSourceFieldName": "attachment_name",
          "dateFieldFormat": "yyyy-MM-dd'T'HH:mm:ss'Z'"
        }
      ]
    }
  },
  "additionalProperties": {
    "rateLimit": "300",
    "maxFileSize": "50",
    "crawlDepth": "2",
    "maxLinksPerUrl": "100",
    "crawlSubDomain": "true",
    "crawlAllDomain": "true",
    "honorRobots": "true"
  }
}
```

### Additional Configuration Options

- **Rate Limit:** Maximum URLs crawled per minute per website host. Default is `300`.
- **Crawl Depth:** Number of hyperlink levels to crawl from the seed URL. Default is `2`.
- **Honor Robots.txt:** Set to `true` to respect robots.txt directives.

---

## Using Authentication

If your websites require authentication, configure the `authentication` sub-property under `repositoryEndpointMetadata`. Supported types include:

- **NoAuthentication**
- **BasicAuth**
- **NTLM/Kerberos**
- **Form**
- **SAML**

Store credentials securely in AWS Secrets Manager using the following JSON structure:

```json
{
  "userName": "your_username",
  "password": "your_password"
}
```

---

## Benefits of Using Amazon Q API for Web Crawling

1. **Efficient Indexing:** Automate data retrieval and indexing from multiple websites.
2. **Customizable Configurations:** Tailor crawling properties like seed URLs, rate limits, and field mappings.
3. **Advanced Authentication Support:** Integrate seamlessly with websites requiring secure credentials.
4. **Compliance with Standards:** Respect robots.txt directives and ensure adherence to ethical scraping practices.

---

## Conclusion

Integrating Amazon Q Business with a web crawler using the Amazon Q API unlocks powerful data collection capabilities. By following the guidelines above, you can efficiently configure crawling, process data, and maintain compliance with industry standards. Maximize your data extraction efforts with proper authentication, robust configurations, and scalable crawling solutions.

---
