
# Scraping eBay Sold Items Using Beautiful Soup

Web scraping can be a powerful tool to extract valuable data from platforms like eBay. In this tutorial, we will demonstrate how to scrape **sold items on eBay** using Python's `BeautifulSoup` and `requests` libraries. Additionally, we will explore alternate options, like using APIs, to streamline the process.

---

## Why Scrape eBay Sold Items?

Extracting data from eBay's sold listings offers several benefits:

- **Market Research**: Analyze product pricing and trends over time.
- **Competitive Analysis**: Gain insights into how similar products are performing in the market.
- **Informed Decisions**: Use real sales data to make informed business and pricing strategies.

By automating this data collection process, you can save significant time and effort.

---

## Prerequisites

Before starting, ensure you have the following Python libraries installed:

1. **BeautifulSoup**: For parsing HTML data.
2. **Requests**: For making HTTP requests.
3. **Pandas**: For exporting data to CSV.

Install these libraries by running:

```bash
pip install beautifulsoup4 requests pandas lxml
```

---

## Step 1: Understanding the Challenge of Requests Blocking

eBay and other platforms often block bot-like behavior by detecting the default `user-agent` string used by libraries like `requests`. To bypass this, we must:

1. **Change the User-Agent**: Mimic a real browser.
2. **Rotate User-Agents**: Use different user-agent strings to reduce the chance of being flagged.
3. **Handle Pagination**: Automate traversing through multiple pages to scrape all the relevant results.

---

## Step 2: Scraping eBay Sold Items with BeautifulSoup

Here’s a Python script to scrape sold items for a specific search query. In this example, we’ll scrape sold listings for "Oakley sunglasses."

### Code Example

```python
from bs4 import BeautifulSoup
import requests
import pandas as pd

# Custom headers to mimic browser behavior
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/106.0.0.0 Safari/537.36"
}

# Parameters for eBay search
params = {
    '_nkw': 'oakley+sunglasses',  # Search query
    'LH_Sold': '1',               # Only sold items
    '_pgn': 1                     # Page number
}

data = []  # To store scraped data

while True:
    # Send request to eBay
    response = requests.get('https://www.ebay.com/sch/i.html', params=params, headers=headers, timeout=30)
    soup = BeautifulSoup(response.text, 'lxml')

    print(f"Scraping page: {params['_pgn']}")

    # Extract product details
    for product in soup.select(".s-item__pl-on-bottom"):
        title = product.select_one(".s-item__title span").text
        price = product.select_one(".s-item__price").text
        try:
            sold_date = product.select_one(".s-item__title--tagblock .POSITIVE").text
        except:
            sold_date = None
        
        data.append({
            "title": title,
            "price": price,
            "sold_date": sold_date
        })

    # Check for the "Next" button to continue pagination
    if soup.select_one(".pagination__next"):
        params['_pgn'] += 1
    else:
        break

# Save results to a CSV file
pd.DataFrame(data=data).to_csv("ebay_sold_items.csv", index=False)
print("Data saved to ebay_sold_items.csv")
```

### **What the Code Does:**

1. **Custom Headers**: Mimics a browser request to avoid blocks.
2. **Pagination Handling**: Uses the `pagination__next` button to navigate through pages.
3. **Data Extraction**: Collects product titles, prices, and sold dates.
4. **CSV Export**: Saves the scraped data into a CSV file.

---

## Step 3: Output

After running the script, a file named `ebay_sold_items.csv` will be created, containing:

| Title                  | Price   | Sold Date       |
|------------------------|---------|-----------------|
| Oakley Sunglasses A    | $50.00  | Jan 01, 2025    |
| Oakley Sports Glasses  | $75.99  | Dec 31, 2024    |

---

## Step 4: Alternative Solution Using ScraperAPI

Scraping platforms like eBay can sometimes block requests, even with headers. To bypass this, you can use **ScraperAPI**. It manages proxies, rotates IPs, and handles CAPTCHAs for you.

👉 **Stop wasting time on proxies and CAPTCHAs! ScraperAPI’s simple API handles millions of web scraping requests so you can focus on analyzing the data. Start your free trial today!**  
[https://www.scraperapi.com/?fp_ref=coupons](https://www.scraperapi.com/?fp_ref=coupons)

### Code Example with ScraperAPI

```python
import requests
import pandas as pd

# Replace with your ScraperAPI key
API_KEY = 'SCRAPE9837861'

# API endpoint and parameters
url = 'https://api.scraperapi.com'
params = {
    'api_key': API_KEY,
    'url': 'https://www.ebay.com/sch/i.html?_nkw=oakley+sunglasses&LH_Sold=1'
}

response = requests.get(url, params=params)
soup = BeautifulSoup(response.text, 'lxml')

data = []

# Extract product details
for product in soup.select(".s-item__pl-on-bottom"):
    title = product.select_one(".s-item__title span").text
    price = product.select_one(".s-item__price").text
    try:
        sold_date = product.select_one(".s-item__title--tagblock .POSITIVE").text
    except:
        sold_date = None

    data.append({
        "title": title,
        "price": price,
        "sold_date": sold_date
    })

# Save results to CSV
pd.DataFrame(data=data).to_csv("ebay_sold_items_scraperapi.csv", index=False)
print("Data saved to ebay_sold_items_scraperapi.csv")
```

---

## Alternative: eBay Organic Results API (Paid Option)

For a more robust and scalable solution, you can use **Ebay Organic Results API** by SerpApi. This paid API handles blocks and parsing on the backend, allowing you to focus on data analysis.

### Code Example with SerpApi

```python
from serpapi import EbaySearch
import pandas as pd
import os

params = {
    "api_key": os.getenv("API_KEY"),  # SerpApi key
    "engine": "ebay",
    "ebay_domain": "ebay.com",
    "_nkw": "oakley+sunglasses",
    "LH_Sold": "1"
}

search = EbaySearch(params)
results = search.get_dict()

data = []

for result in results.get("organic_results", []):
    data.append({
        "title": result.get("title"),
        "price": result.get("price")
    })

# Save to CSV
pd.DataFrame(data=data).to_csv("ebay_sold_items_serpapi.csv", index=False)
print("Data saved to ebay_sold_items_serpapi.csv")
```

---

## Conclusion

Scraping eBay sold listings can unlock valuable insights for market analysis and business decisions. While BeautifulSoup provides a cost-effective approach, APIs like **ScraperAPI** or **SerpApi** can simplify the process by handling anti-scraping challenges.

Start scraping eBay today to gain a competitive edge and make data-driven decisions for your business!

👉 **Get started with ScraperAPI now:** [https://www.scraperapi.com/?fp_ref=coupons](https://www.scraperapi.com/?fp_ref=coupons)
