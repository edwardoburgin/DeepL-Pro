
# Comprehensive Guide to Using TS-Walmart-Scraper for Walmart Product Data Extraction

**TS-Walmart-Scraper** is a robust TypeScript-based web scraping tool built on the Crawlee library. It is designed to extract essential data from products listed on Walmart.com. Whether you need data from category URLs, brand URLs, search keywords, or specific product pages, this scraper simplifies the process.

---

## Features and Setup Guide

### Clone the Repository

Start by cloning the repository from GitHub and navigating to the project folder:

```bash
git clone https://github.com/<username>/TS-Walmart-Scraper.git
cd TS-Walmart-Scraper
```

### Install Dependencies

Use the following command to install the necessary dependencies:

```bash
npm install
```

---

Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. Start your free trial today! 👉 [https://www.scraperapi.com/?fp_ref=coupons](https://www.scraperapi.com/?fp_ref=coupons)

---

## How to Use TS-Walmart-Scraper

The scraper accepts a JSON input file named `INPUT.json`, which should be placed in the following directory:  
`project_folder\storage\key_value_stores\default\`.

### Example JSON Input File

The `INPUT.json` file must include the following fields:

- **`productUrls`**: An array of specific product page URLs to scrape.
- **`listingUrls`**: An array of category or brand page URLs to scrape (includes product listings and pagination).
- **`keywords`**: An array of search keywords for Walmart.com.
- **`maxPrice`**: The maximum price range for products to scrape.
- **`minPrice`**: The minimum price range for products to scrape.
- **`startPageNumber`**: The page number to start scraping from.
- **`finalPageNumber`**: The page number to stop scraping at.

### Special Notes:
- Set `minPrice` and `maxPrice` to `0` to scrape products across all price ranges.
- Set `startPageNumber` and `finalPageNumber` to `0` to scrape all available pages.

### Run the Scraper

Once your `INPUT.json` file is ready, run the scraper using the following command:

```bash
npm start
```

---

## Output Data Structure

The scraper generates a series of JSON files, each containing detailed information about a single product. These files can be found in the directory:  
`project_folder\storage\datasets\default\`.

### Output Fields:

The JSON files include the following fields:

- **`URL`**: The product page URL.
- **`idCodes`**: Unique identifiers, including SKU and UPC codes.
- **`seller`**: Details about the seller and brand, such as `brand`, `brandURL`, `seller`, and `sellerURL`.
- **`title`**: The product title.
- **`media`**: Media URLs, including `main` image URL, `gallery` of images, and video objects (with `title` and `url` fields).
- **`pricing`**: Pricing details, including `salePrice`, `fullPrice`, and `currencySymbol`.
- **`isAvailable`**: A boolean indicating product availability.
- **`isGiftEligible`**: A boolean indicating if the product is eligible for gifting.
- **`isUsed`**: A boolean indicating whether the product is used.
- **`rating`**: Ratings and reviews for the product and seller, including `itemRating`, `itemReviews`, `sellerRating`, and `sellerReviews`.
- **`orderLimits`**: Minimum and maximum order limits (`min` and `max` fields).
- **`category`**: Category details, including `fullPath` and `pathParts` (array of objects with `name` and `url` fields).
- **`info`**: Additional product information, including `shortDescription`, `longDescription`, and `specifications` (array of attributes and values).
- **`variants`**: Variant information, including availability, pricing, and options.

---

## Licensing and Collaboration

This project is licensed under the **AGPL-3.0 license**. For more details, refer to the [LICENSE](https://github.com/eneiromatos/TS-email-scraper/blob/develop/LICENSE.md) file.

We welcome contributions to improve TS-Walmart-Scraper. Feel free to fork the repository, open issues, or submit pull requests. Your feedback and suggestions are highly appreciated.

---

Start optimizing your data extraction process today with TS-Walmart-Scraper and tools like ScraperAPI. For seamless scraping without proxies or CAPTCHAs, try ScraperAPI now:  
👉 [https://www.scraperapi.com/?fp_ref=coupons](https://www.scraperapi.com/?fp_ref=coupons)
