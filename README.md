<p align="center">
    <a href="https://scrapezone.com/"><img src="https://app.scrapezone.com/img/logo.svg" alt="Scrapezone Logo" width="300" height="60"></a>
  </a>
</p>

<h2 align="center">
  Getting Data Was Never Easier
</h2>

<p align="center">
Forget about proxies, servers, and IP addresses. Just get the data you need.
</p>

Google and eCommerce HTML Scraper
Send a request with up to 1,000 URLs and receive the raw, unblocked HTML files.

## Quick Start

1. Create a new account at: https://app.scrapezone.com
2. Copy your scrape username and password:
   ![Username and Password](/images/user_pass.png)
3. Start getting the data you need.

## Sending a request

A request is sent in batches of 1-1,000 URLs.

Endpoint: POST http://api.scrapezone.com/scrape

Parameters:

`query`: a list of URLs to scrape.

`callback_url`: the URL to send the response to once the scrape is done (Optional).

`country`: the country from which the request should be originated. Supported countries:

`'us', 'fr', 'it', 'de', 'uk'`

Request Example:

```
curl --user user:pass \
--header "Content-Type: application/json" \
--request POST \
--data '{"query":["https://www.amazon.com/Best-Sellers-Electronics/zgbs/electronics"]}' \
https://api.scrapezone.com/scrape
```

### Response

The response will be formatted in the following way:
`job_id`: a list of URLs to scrape.

`callback_url`: the URL to send the response to once the scrape is done.

`parser_name`: the name of the parser to use on the results. For more info check [Parsed Results](https://github.com/Scrapezone/examples/blob/master/README.md#parsed-results)

Response Example:

```
{
  "job_id": "12345678987654321",
  "callback_url": "YOUR_CALLBACK_URL",
  "parser_name": "Requested parser name"
}
```

## Getting the results:

There are two methods of getting the response:

- Using continuous polling (GET /scrape/job_id)
- Using a callback URL

### GET /scrape/job_id

An endpoint to check the scrape status and download the results once the scrape is done.
Status:
Status can be

Callback URL
If a callback URL was given in the request, once the scrape is done we will send a POST request to that URL, containing the response object.

The response object will be in the following format:

```
{
  "job_id": "12345678987654321",
  "callback_url": "THE_CALLBACK_URL",
  "status:" <scraping/done/faulted>,
  "html_files:"
    [
    {
       "url": <given_url_1>,
       "output": <URL of the downloadable html file>
    },
    ...
    {
       "url": <given_url_n>,
       "output": <URL of the downloadable html file>
    },
    ]
}
```

“html_files” will be sent only for scrapes with status “done”, otherwise “results” will be null.

## Parsed Results

Parsed results allow you to get a JSON or CSV file with the parsed data!
Available parsers:
|Scraper Name|Description|Results File Structure|
|-----------|-----------|--------------------|
|amazon_product_display|Amazon Product Display Page |[Documentation](/parsers/amazon_product_display.md)|
|amazon_search|Amazon search or category page|[Documentation](/parsers/amazon_search.md)|
|bestbuy_product_display|BestBuy Product Display Page |[Documentation](/parsers/bestbuy_product_display.md)|
|ebay_product_display|Ebay Product Display Page |[Documentation](/parsers/ebay_product_display.md)|
|etsy_product_display|Etsy Product Display Page |[Documentation](/parsers/etsy_product_display.md)|
|flipkart_product_display|Flipkart Product Display Page |[Documentation](/parsers/flipkart_product_display.md)|
|google_news|Google News Results Page|[Documentation](/parsers/google_news.md)|
|google_search|Google Search Results Page|[Documentation](/parsers/google_search.md)|
|homedepot_product_display|The Home Depot Product Display Page|[Documentation](/parsers/homedepot_product_display.md)|
|lowes_product_display|Lowes Product Display Page |[Documentation](/parsers/lowes_product_display.md)|
|target_product_display|Target Product Display Page |[Documentation](/parsers/target_product_display.md)|
|walmart_product_display|Walmart Product Display Page |[Documentation](/parsers/walmart_product_display.md)|
|wayfair_product_display|Wayfair Product Display Page |[Documentation](/parsers/wayfair_product_display.md)|

### Example Request

This requst will result in the parsed product details of 2 Amazon products.

```
curl --user user:pass \
--header "Content-Type: application/json" \
--request POST \
--data '{"query":["https://www.amazon.com/dp/B08J65DST5", "https://www.amazon.com/dp/B07FZ8S74R"], \
"parser_name": "amazon_product_display"}' \
https://api.scrapezone.com/scrape
```
