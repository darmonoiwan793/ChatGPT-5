
# A Step-By-Step Guide to Web Scraping with Crawlee

Web scraping is a powerful tool to extract data from websites, and **Crawlee** is one of the most efficient and beginner-friendly libraries for this purpose. Crawlee is designed to simplify data collection by offering features like **proxy rotation** and **session management**, making it an excellent choice for scraping large or dynamic websites.

In this guide, we’ll explore Crawlee, from basic usage to advanced concepts like proxy handling, session management, and scraping dynamic content.

---

## Why Use Crawlee for Web Scraping?

Crawlee provides several features that set it apart:

- **Proxy Rotation**: Prevents your IP from being blocked during data collection.
- **Session Handling**: Allows you to maintain state across multiple requests.
- **Dynamic Content Scraping**: Can handle JavaScript-heavy websites by integrating with headless browsers like Puppeteer.
- **User-Friendly Syntax**: Provides a simple, intuitive API for developers of all skill levels.

### **Need a Reliable API to Avoid Blocks?**

Stop wasting time on proxies and CAPTCHAs! ScraperAPI’s simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more.

👉 Start your free trial today! [https://www.scraperapi.com/?fp_ref=coupons](https://www.scraperapi.com/?fp_ref=coupons)

---

## Getting Started with Crawlee

### Prerequisites

Ensure you have the following installed on your machine:

1. **Node.js**: Download and install it from [Node.js official site](https://nodejs.org/).
2. **Crawlee Library**: Install it via npm.

### Install Crawlee

Open your terminal and set up a new Node.js project:

```bash
mkdir crawlee-tutorial
cd crawlee-tutorial
npm init -y
npm install crawlee
```

---

## Basic Web Scraping with Crawlee

Let’s start by scraping the **Books to Scrape** website—a beginner-friendly platform with a simple HTML structure.

### Inspect the Website

Use your browser’s Developer Tools to inspect the webpage and identify the HTML tags containing the data you want. For example:

- **Book Titles**: Found in the `<a>` tag within an `<h3>` element.
- **Book Prices**: Found in the `<p>` tag with the class `price_color`.

### Writing the Code

Create a `scrape.js` file and add the following code:

```javascript
const { CheerioCrawler } = require('crawlee');

const crawler = new CheerioCrawler({
    async requestHandler({ $, log }) {
        const books = [];
        $('article.product_pod').each((index, element) => {
            const title = $(element).find('h3 a').attr('title');
            const price = $(element).find('.price_color').text();
            books.push({ title, price });
        });
        log.info(books);
    },
});

crawler.run(['https://books.toscrape.com/']);
```

### Run the Script

Execute the script in your terminal:

```bash
node scrape.js
```

You’ll see an array of book titles and prices printed to your terminal.

---

## Advanced Crawlee Features

### 1. Proxy Rotation

Proxies route your requests through different IP addresses, helping you avoid blocks and rate limits.

#### Setting Up Proxies

First, sign up for a proxy service like Bright Data. Once you have your proxy credentials, update your script to include proxy configuration:

```javascript
const { CheerioCrawler, ProxyConfiguration } = require('crawlee');

const proxyConfiguration = new ProxyConfiguration({
    proxyUrls: ['http://USERNAME:PASSWORD@HOST:PORT'],
});

const crawler = new CheerioCrawler({
    proxyConfiguration,
    async requestHandler({ $, log }) {
        const books = [];
        $('article.product_pod').each((index, element) => {
            const title = $(element).find('h3 a').attr('title');
            const price = $(element).find('.price_color').text();
            books.push({ title, price });
        });
        log.info(books);
    },
});

crawler.run(['https://books.toscrape.com/']);
```

Replace `USERNAME`, `PASSWORD`, `HOST`, and `PORT` with your proxy details.

---

### 2. Session Management

Sessions allow you to maintain state across multiple requests, such as handling cookies or logged-in sessions.

#### Example Code with Sessions

```javascript
const { CheerioCrawler, SessionPool } = require('crawlee');

(async () => {
    const sessionPool = await SessionPool.open();
    const session = await sessionPool.createSession();

    const crawler = new CheerioCrawler({
        useSessionPool: true,
        async requestHandler({ $, session, log }) {
            log.info(`Using session: ${session.id}`);
            const books = [];
            $('article.product_pod').each((index, element) => {
                const title = $(element).find('h3 a').attr('title');
                const price = $(element).find('.price_color').text();
                books.push({ title, price });
            });
            log.info(books);
        },
    });

    await crawler.run(['https://books.toscrape.com/']);
})();
```

Run this script, and you’ll see session IDs logged with each request.

---

### 3. Scraping Dynamic Content with Puppeteer

For JavaScript-heavy websites, Crawlee integrates with Puppeteer—a headless browser capable of rendering JavaScript.

#### Example: Scraping YouTube Comments

Create a `scrapeDynamicContent.js` file:

```javascript
const { PuppeteerCrawler } = require('crawlee');

const crawler = new PuppeteerCrawler({
    async requestHandler({ page, log }) {
        await page.goto('https://www.youtube.com/watch?v=wZ6cST5pexo', {
            waitUntil: 'networkidle2',
        });

        const comments = await page.evaluate(() => {
            return Array.from(document.querySelectorAll('#content-text')).map(
                (el) => el.innerText
            );
        });

        log.info('Comments:', comments);
    },
});

crawler.run(['https://www.youtube.com/watch?v=wZ6cST5pexo']);
```

Run the script:

```bash
node scrapeDynamicContent.js
```

The output will include the top comments from the video.

---

## Best Practices for Web Scraping

1. **Respect Robots.txt**: Always check the `robots.txt` file of a website to understand its scraping policies.
2. **Avoid Overloading Servers**: Use delays between requests to avoid overwhelming the server.
3. **Monitor IP Usage**: Use proxy rotation to avoid getting blocked.

---

## Conclusion

Crawlee is a versatile and powerful library that simplifies web scraping for developers of all skill levels. Whether you’re scraping static pages or dynamic content, Crawlee has the tools you need.

Ready to take your scraping projects to the next level? Use **ScraperAPI** to handle proxies, CAPTCHAs, and more.

👉 **Get started with ScraperAPI today:** [https://www.scraperapi.com/?fp_ref=coupons](https://www.scraperapi.com/?fp_ref=coupons)
