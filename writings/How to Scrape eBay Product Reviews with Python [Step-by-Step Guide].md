
# How to Scrape eBay Product Reviews with Python [Step-by-Step Guide]

## Overview

In this guide, we’ll walk through creating a Python scraper to extract product reviews from eBay product pages. This guide uses **BeautifulSoup**, **Requests**, and **JSON** to streamline and organize your scraping process.

---

**Stop wasting time on proxies and CAPTCHAs!**  
ScraperAPI handles millions of web scraping requests seamlessly. Get structured data from Amazon, Google, Walmart, and more.  
👉 [Start your free trial now!](https://www.scraperapi.com/?fp_ref=coupons)

---

## TL;DR: The Complete eBay Product Review Scraper Script

For those eager to dive into the code, here’s the full Python script:

```python
from bs4 import BeautifulSoup
import requests
import json

API_KEY = "YOUR_API_KEY"
url = "https://www.ebay.com/itm/185982388419?hash=item2b4d6a08c3:g:4soAAOSw0gJkrbma&var=694001364789"

reviews = []

def request_function(url):
    payload = {"api_key": API_KEY, "url": url}
    response = requests.get("http://api.scraperapi.com", params=payload).text
    return response

def organize_data(product_name: str, product_price: str, reviews_list: list):
    result = {
        "product_name": product_name,
        "product_price": product_price,
        "reviews": reviews_list
    }
    output = json.dumps(result, indent=2, ensure_ascii=False).encode("ascii", "ignore").decode("utf-8")
    try:
        with open("output.json", "w", encoding="utf-8") as json_file:
            json_file.write(output)
    except Exception as e:
        print(f"Encountered an error: {e}")

response = request_function(url)
soup = BeautifulSoup(response, "lxml")
PRODUCT_NAME = soup.find("span", class_="ux-textspans ux-textspans--BOLD").text
PRODUCT_PRICE = soup.find("div", class_="x-price-primary").text.split(" ")[-1]

feedback_url = soup.find("a", class_="fdbk-detail-list__tabbed-btn fake-btn fake-btn--large fake-btn--secondary").get('href')
soup = BeautifulSoup(request_function(feedback_url), "lxml")
received_reviews = soup.find_all('div', class_="card__comment")
user_and_rating = soup.find_all('div', class_="card__from")

for each in range(len(received_reviews)):
    review = {
        "User": user_and_rating[each].text.split('(')[0],
        "Rating": user_and_rating[each].text.split("(")[-1].rstrip(")"),
        "User_Review": received_reviews[each].text,
        "Review_Time": soup.find('span', {'data-test-id': f'fdbk-time-{each + 1}'}).text
    }
    reviews.append(review)

organize_data(PRODUCT_NAME, PRODUCT_PRICE, reviews)
```

> **Note:** Replace `"YOUR_API_KEY"` with your [ScraperAPI key](https://www.scraperapi.com/?fp_ref=coupons).

---

## Understanding eBay Product Reviews

eBay product pages contain valuable data, including:
- Product name and price.
- User reviews, including feedback on product quality and seller service.

This guide focuses on extracting reviews from the eBay product and feedback pages.

---

## Requirements

### Prerequisites
1. **Python** installed on your system.
2. Libraries:
   ```bash
   pip install beautifulsoup4 requests lxml
   ```
3. A [ScraperAPI account](https://www.scraperapi.com/?fp_ref=coupons) for managing requests.

### Folder Setup
```bash
mkdir ebay-scraper
cd ebay-scraper
type nul > scraper.py
```

---

## Step-by-Step Guide

### Step 1: Import Libraries and Set Up Variables

```python
from bs4 import BeautifulSoup
import requests
import json

API_KEY = 'YOUR_API_KEY'
url = "https://www.ebay.com/itm/185982388419?hash=item2b4d6a08c3:g:4soAAOSw0gJkrbma&var=694001364789"

reviews = []
```

---

### Step 2: Create a Function to Fetch HTML

Use ScraperAPI to fetch eBay’s HTML pages without getting blocked.

```python
def request_function(url):
    payload = {"api_key": API_KEY, "url": url}
    response = requests.get("http://api.scraperapi.com", params=payload).text
    return response
```

---

### Step 3: Extract Product Name, Price, and Feedback URL

```python
response = request_function(url)
soup = BeautifulSoup(response, "lxml")
PRODUCT_NAME = soup.find("span", class_="ux-textspans ux-textspans--BOLD").text
PRODUCT_PRICE = soup.find("div", class_="x-price-primary").text.split(" ")[-1]

feedback_url = soup.find("a", class_="fdbk-detail-list__tabbed-btn fake-btn fake-btn--large fake-btn--secondary").get('href')
```

---

### Step 4: Scrape Product Reviews

Parse the feedback page to extract reviews.

```python
soup = BeautifulSoup(request_function(feedback_url), "lxml")
received_reviews = soup.find_all('div', class_="card__comment")
user_and_rating = soup.find_all('div', class_="card__from")

for each in range(len(received_reviews)):
    review = {
        "User": user_and_rating[each].text.split('(')[0],
        "Rating": user_and_rating[each].text.split("(")[-1].rstrip(")"),
        "User_Review": received_reviews[each].text,
        "Review_Time": soup.find('span', {'data-test-id': f'fdbk-time-{each + 1}'}).text
    }
    reviews.append(review)
```

---

### Step 5: Organize and Save Data to JSON

Write the scraped data into a clean JSON file.

```python
def organize_data(product_name: str, product_price: str, reviews_list: list):
    result = {
        "product_name": product_name,
        "product_price": product_price,
        "reviews": reviews_list
    }
    output = json.dumps(result, indent=2, ensure_ascii=False).encode("ascii", "ignore").decode("utf-8")
    try:
        with open("output.json", "w", encoding="utf-8") as json_file:
            json_file.write(output)
    except Exception as e:
        print(f"Encountered an error: {e}")
```

Invoke the function:
```python
organize_data(PRODUCT_NAME, PRODUCT_PRICE, reviews)
```

---

## Example Output

Here’s a snippet of the resulting JSON file:

```json
{
  "product_name": "Shipping and returns",
  "product_price": "$179.55/ea",
  "reviews": [
    {
      "User": "l***e",
      "Rating": "56",
      "User_Review": "Thanks ",
      "Review_Time": "Past month"
    },
    {
      "User": "a***r",
      "Rating": "33",
      "User_Review": "Ordered an older product by mistake. Couldn't sync with me main computer. Return was easy. Good vendor",
      "Review_Time": "Past month"
    }
  ]
}
```

---

## Conclusion

You’ve successfully created an **eBay product review scraper** using Python. This script:
- Extracts product details and reviews.
- Organizes data into a structured JSON format.

For large-scale scraping projects, consider using ScraperAPI to ensure smooth data collection. Get started with [ScraperAPI here](https://www.scraperapi.com/?fp_ref=coupons).

Happy scraping!
