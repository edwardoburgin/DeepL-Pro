
# How to Use HTML Crawler API: OxyLabs Real-Time Crawler for Other Sites

## Overview

HTML Crawler API is a powerful tool for extracting public web data efficiently. With OxyLabs Real-Time Crawler, you can automate the retrieval of data from various sites, ensuring speed, accuracy, and reliability. This guide walks you through how to use the HTML Crawler API, its integration methods, and its features.

---

**Stop wasting time on proxies and CAPTCHAs!**  
ScraperAPI’s simple API handles millions of web scraping requests, so you can focus on the data. Extract structured data from Amazon, Google, Walmart, and more.  
👉 [Start your free trial today!](https://www.scraperapi.com/?fp_ref=coupons)

---

## Quick Start Guide

HTML Crawler API supports **basic HTTP authentication**, requiring a username and password for access. Below is a quick example to get started using the API.

### Example: Making a Basic Request

```bash
curl --user "USERNAME:PASSWORD" 'https://realtime.oxylabs.io/v1/queries' \
-H "Content-Type: application/json" \
-d '{"source": "universal", "url": "https://ip.oxylabs.io"}'
```

Replace `USERNAME` and `PASSWORD` with your credentials to access the API.

### Key Features
- **Flexible integration methods** for real-time data extraction.
- **Support for structured data output** (e.g., JSON) for efficient parsing.
- **Callback and storage options** for asynchronous and batch processing.

For further assistance, contact the OxyLabs support team via email: [email protected]

---

## Integration Methods

HTML Crawler API offers three primary integration methods:

1. **Push-Pull**: Ideal for asynchronous tasks.
2. **Realtime**: Provides immediate results by keeping the connection open.
3. **SuperAPI**: Acts as a proxy for seamless integration with existing setups.

### 1. Push-Pull Method (Recommended)

This method enables you to send queries, retrieve job IDs, and check their status before fetching results. Here’s an example:

#### Single Query Example:
```bash
curl --user "USERNAME:PASSWORD" 'https://data.oxylabs.io/v1/queries' \
-H "Content-Type: application/json" \
-d '{
  "source": "universal",
  "url": "https://stackoverflow.com/questions/tagged/python",
  "callback_url": "https://your.callback.url",
  "storage_type": "s3",
  "storage_url": "YOUR_BUCKET_NAME"
}'
```

#### Sample API Response:
```json
{
  "callback_url": "https://your.callback.url",
  "id": "12345678900987654321",
  "status": "pending",
  "storage_url": "YOUR_BUCKET_NAME/12345678900987654321.json"
}
```

You can check job status using the `id` or retrieve results from the `results` endpoint once the status is `done`.

---

### 2. Realtime Method

The **Realtime** integration keeps the connection open until the scraping task is completed. This method works best for immediate, one-off requests.

#### Example Request:
```bash
curl --user "USERNAME:PASSWORD" 'https://realtime.oxylabs.io/v1/queries' \
-H "Content-Type: application/json" \
-d '{"source": "universal", "url": "https://stackoverflow.com/questions/tagged/python"}'
```

---

### 3. SuperAPI Method

SuperAPI simplifies scraping by acting as a proxy. Use your preferred language to make GET or POST requests directly.

#### Example GET Request:
```bash
curl -k -x realtime.oxylabs.io:60000 -U "USERNAME:PASSWORD" \
"https://stackoverflow.com/questions/tagged/python"
```

---

## Advanced Features

### Batch Queries
Submit up to 1,000 URLs or keywords in a single batch for efficient processing.

#### Example Batch Request:
```bash
curl --user "USERNAME:PASSWORD" 'https://data.oxylabs.io/v1/queries/batch' \
-H "Content-Type: application/json" \
-d '{
  "url": [
    "https://stackoverflow.com/questions/tagged/python",
    "https://stackoverflow.com/questions/tagged/golang"
  ],
  "source": "universal",
  "callback_url": "https://your.callback.url"
}'
```

---

### Callback Setup
Use the **Callback** feature to receive a POST notification once the scraping task is complete. The callback will include a URL to download the scraped data.

#### Sample Callback Output:
```json
{
  "callback_url": "https://your.callback.url",
  "id": "12345678900987654321",
  "status": "done",
  "results_url": "https://data.oxylabs.io/v1/queries/12345678900987654321/results"
}
```

---

### Upload to Cloud Storage
Store your scraped data directly in an Amazon S3 or Google Cloud Storage bucket for convenience.

#### Example S3 Setup:
1. Create a bucket in your AWS account.
2. Set the appropriate permissions using the JSON policy provided by OxyLabs.

#### Example Storage URL:
```json
"storage_url": "YOUR_BUCKET_NAME/job_ID.json"
```

---

## Parameter Guide

### User-Agent Types
Specify the type of user-agent to use for scraping. Available options include:
- `desktop_chrome`
- `mobile_android`
- `tablet_ios`

#### Example Parameter:
```json
"user_agent_type": "desktop_chrome"
```

### Rendering Options
Enable JavaScript rendering for dynamic pages by setting the `render` parameter:
- `html`: Returns a rendered HTML document.
- `png`: Returns a screenshot of the webpage.

#### Example Parameter:
```json
"render": "html"
```

### Geo-Location
Target specific geographic regions by setting the `geo_location` parameter. Download the full list of supported geo-locations [here](https://docs.oxylabs.io/resources/universal-supported-geo_location-values.csv).

---

## Account Management

### Usage Statistics
Monitor your API usage and limits by querying the `stats` endpoint.

#### Example Request:
```bash
curl --user "USERNAME:PASSWORD" 'https://data.oxylabs.io/v1/stats'
```

#### Sample Output:
```json
{
  "data": {
    "sources": [
      {
        "results_count": "100",
        "title": "universal"
      }
    ]
  }
}
```

---

HTML Crawler API is a robust solution for all your web scraping needs, offering flexibility and efficiency. Start automating your data extraction tasks today and unlock the potential of the web!
