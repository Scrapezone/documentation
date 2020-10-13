# Documentation
Google and eCommerce HTML Scraper
Send a request with up to 1,000 URLs and receive the raw, unblocked HTML files.
For trial accounts, the service is limited to Google.com and Amazon.com pages.

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
`job_id`:  a list of URLs to scrape.

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
* Using continuous polling (GET /scrape/job_id)
* Using a callback URL

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
|Parser Name|Description|Example Results File|
|-----------|-----------|--------------------|
|amazon_product_display|Amazon Product Display Page  | [JSON](https://file)    [CSV](https://test)|
|amazon_search|Amazon search or category page| [JSON](https://file)    [CSV](https://test)|
|walmart_product_display|Walmart Product Display Page | [JSON](https://file)    [CSV](https://test)|
|bestbuy_product_display|BestBuy Product Display Page | [JSON](https://file)    [CSV](https://test)|
|target_product_display|Target Product Display Page  | [JSON](https://file)    [CSV](https://test)|
|wayfair_product_display|Wayfair Product Display Page  | [JSON](https://file)    [CSV](https://test)|
|etsy_product_display|Etsy Product Display Page  | [JSON](https://file)    [CSV](https://test)|
|ebay_product_display|Ebay Product Display Page  | [JSON](https://file)    [CSV](https://test)|
|lowes_product_display|Lowes Product Display Page | [JSON](https://file)    [CSV](https://test)|
|homedepot_product_display|The Home Depot Product Display Page  | [JSON](https://file)    [CSV](https://test)|
|google_search|Google Search Results Page| [JSON](https://file)    [CSV](https://test)|
|google_news|Google News Results Page| [JSON](https://file)    [CSV](https://test)|

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
