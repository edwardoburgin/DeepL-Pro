# How I Built a Craft Beer Search Engine for Free

## Introduction

Craft beers are delicious—expensively delicious. When I first moved to Singapore, I was amazed by the variety of craft beer options available. Coming from Malaysia, I felt spoiled for choice here.

Soon, my friends and I became swept up in the craft beer fever. However, there was one major problem—craft beers are relatively pricey. Growing up in a frugal household, I found myself spending hours browsing online for the best deals. This process quickly turned from an exciting hunt to an exhausting frustration.

That’s when the idea struck: why not create a search engine for craft beers?

---

**Stop wasting time on proxies and CAPTCHAs!**  
ScraperAPI’s simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more.  
👉 [Start your free trial today!](https://www.scraperapi.com/?fp_ref=coupons)

---

## Preface

This article details my decisions and thought process behind creating [Burplist](https://burplist.me/), a fast and simple search engine for craft beers. 

Rather than diving into code examples, this is a high-level overview of how and why certain approaches were taken. If you’re interested in the technical details, including the [source code](https://github.com/ngshiheng/burplist), links to related guides are included in different sections of this post.

## Goals

I had four primary goals for Burplist:

1. **Fast Search**: The search feature must be quick and responsive.
2. **Accurate Pricing**: Price information must be reliable and updated daily.
3. **Low Running Costs**: Ideally, the project should be free to run.
4. **Ethical Web Scraping**: Ensure no harm is done to the sites being scraped.

With these goals in mind, I began brainstorming and building Burplist.

## Sources of Craft Beer in Singapore

Where can you buy craft beers online in Singapore? A quick Google search for "craft beer in Singapore" reveals several options:

- Independent craft beer sellers
- Large e-commerce platforms like [Shopee](https://shopee.sg/) and [Redmart (Lazada)](https://redmart.lazada.sg/)
- Grocery stores offering discounts and promotions on craft beers

Altogether, these sources provide more than 10 websites to choose from, making it easier to find deals.

## Web Crawlers: Gathering the Data

### Why Use Web Crawling?

To ensure fast search functionality, data needs to be stored on our servers rather than being fetched on-demand. For this, I used web scraping to gather information during off-peak internet hours. The collected data is stored in a hosted database, allowing users to query it efficiently.

### Python Scrapy Framework

Burplist uses over 10 unique crawlers (spiders), each responsible for extracting data from a specific website. These crawlers are built using [Scrapy](https://scrapy.org/), a powerful Python web crawling framework ideal for mid- to large-scale projects. 

**Why Scrapy?**  
Scrapy simplifies the often tedious process of post-processing and cleaning scraped data using its built-in item pipeline. At Burplist, Scrapy pipelines are used to:

- Clean and normalize data from multiple sources
- Validate scraped items to ensure essential fields are present
- Remove duplicate entries
- Store data in the database

You can find Burplist's [web crawler source code here](https://github.com/ngshiheng/burplist) and tips on working with Scrapy [here](https://jerrynsh.com/5-useful-tips-while-working-with-python-scrapy/).

### Respecting Rate Limits

The golden rule of web scraping is to avoid harming the website. To adhere to this:

- Crawlers run only once a day during off-peak hours.
- Rate limits are respected to avoid overwhelming the target site.
- `HttpCacheMiddleware` is used during development to cache requests and minimize unnecessary server load.

## Hosting, Database, and Scheduling

### Hosting with Heroku

To keep costs low (ideally free), I chose [Heroku](https://www.heroku.com/). Heroku's free tier offers generous resources, including 1,000 free dyno hours per month with a verified credit card. Learn more about deploying Scrapy spiders on Heroku for free [here](https://jerrynsh.com/how-to-deploy-python-scrapy-spiders-for-free-on-cloud/).

### Heroku Scheduler and PostgreSQL

- **Scheduler**: Burplist uses [Heroku Scheduler](https://devcenter.heroku.com/articles/scheduler) to run crawlers daily during off-peak hours. This uses about 30/1,000 free dyno hours monthly, leaving ample resources for other tasks.
- **PostgreSQL**: The free Hobby Dev tier provides a 10,000-row limit, which has been sufficient so far. A garbage collector service runs weekly to remove outdated products from the database.

### Frontend Development

While Burplist’s UI isn’t flashy, it was built quickly using [PyWebIO](https://github.com/pywebio/PyWebIO), which allowed me to create an interactive web app using only Python and Markdown—no HTML or CSS required. The frontend is hosted on Heroku as well, consuming 744/1,000 free dyno hours monthly. Check out the [frontend repository](https://github.com/ngshiheng/burplist-frontend).

## Handling Cold Starts and Downtime

### Mitigating Cold Starts

Heroku’s free dynos sleep after 30 minutes of inactivity, causing a delay when users access the site. To mitigate this, I use [UpTimeRobot](https://uptimerobot.com/) to ping the site every 5 minutes, ensuring it stays awake.

## Managing Proxies

When scraping large e-commerce sites, anti-scraping measures are common. Rather than managing my own proxy infrastructure, I use [ScraperAPI](https://www.scraperapi.com/?fp_ref=coupons). It simplifies proxy management, and its free tier fits my project needs perfectly.

---

## Closing Thoughts

I built [Burplist](https://burplist.me/) out of frustration from searching multiple sites for the best craft beer deals. It took around six weeks to build the MVP, and I’m thrilled that the site is live and practically free to run (except for the domain name).

As a huge fan of productivity tools, I hope Burplist saves people time. If you're interested in data science and want to collaborate on charts or analytics, let me know—I’d love to share what I’ve built!

Thanks for reading, and cheers to craft beers!
