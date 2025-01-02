
# Python Instant Web Scraping: API Guide

## Overview

Web scraping has become an essential tool for data analysis and data mining. However, content extractors often pose significant challenges to the generalization of web scraping programs. A well-defined content extractor can turn your web scraping tool into a universal framework. This guide provides detailed instructions for using GooSeeker's API to download content extractors, along with practical examples.

---

## API Description: Download `gsExtractor` Content Extractor

### 1. API Name

**Download Content Extractor**

### 2. API Overview

When building a web scraping program, most of your time is spent debugging web content extraction rules. Whether using regular expressions or XPath, creating and testing these rules can be tedious and time-consuming, especially when extracting multiple fields from a webpage. 

With GooSeeker's API, you can download a pre-tested extractor script (in XSLT format). This script is designed to run directly against the target webpage's DOM, returning XML-formatted results for all required fields in one go.

The XSLT extractor can either be created using your own tools, such as MS Mapper, or shared by others with read access. Once downloaded via the API, the extractor integrates seamlessly into your scraping framework.

For more details, see the [GooSeeker Python Web Scraping Open Source Project](http://www.gooseeker.com).

---

## 3. API Specifications

### 3.1 API Endpoint (URL)

```plaintext
http://www.gooseeker.com/api/getextractor
```

### 3.2 Request Content Type

No specific content type required.

### 3.3 Request Method

**HTTP GET**

### 3.4 Request Parameters

| **Parameter** | **Required** | **Type** | **Description**                                          |
|---------------|--------------|----------|----------------------------------------------------------|
| `key`         | Yes          | String   | API key assigned upon application                        |
| `theme`       | Yes          | String   | Extractor name defined in MS Mapper                      |
| `middle`      | No           | String   | Rule number (if multiple rules exist under the same name)|
| `bname`       | No           | String   | Organizer box name (if multiple organizer boxes exist)   |

**Note:** For terminology explanations, refer to [GooSeeker’s Glossary](http://www.gooseeker.com/terms).

### 3.5 Response Content Type

**text/xml; charset=UTF-8**

### 3.6 Response Parameters

| **Parameter**    | **Type** | **Description**                                                                                   |
|-------------------|----------|---------------------------------------------------------------------------------------------------|
| `more-extractor` | String   | Indicates the number of extractors under the same rule name. Useful when optional parameters are omitted. |

### 3.7 Error Responses

- **HTTP 400:** Returned for syntax errors in the URL or invalid parameters.  
- **HTTP 200 OK:** Application-level errors are included in the response body as an XML file with the following structure:

```xml
<error>
    <code>Specific error code</code>
</error>
```

#### Error Codes:

- `keyError`: Authentication failed.  
- `paramError`: Invalid parameters in the URL (e.g., incorrect parameter names or values).  
- `empty`: Not an error, but indicates the requested extractor does not exist (e.g., no organizer box was created for the rule).  

---

## 4. Usage Example (Python)

To generate an extractor, see the "1-Minute Guide to Creating XSLT for Web Content Extraction." Below is a Python code example for using the API:

### Example Code

```python
# -*- coding: utf-8 -*-
from urllib import request

url = "http://www.gooseeker.com/api/getextractor?key=YOUR_KEY&theme=YOUR_EXTRACTOR_NAME"

resp = request.urlopen(url)
content = resp.read()
if content:
    print(content)
```

This script fetches the extractor and prints its XML output. Replace `YOUR_KEY` and `YOUR_EXTRACTOR_NAME` with your actual API key and extractor name.

---

## Why Use Web Scraping APIs?

Stop wasting time debugging content extractors! GooSeeker’s API simplifies web scraping by providing pre-tested extractor scripts that save hours of manual effort. Pair this with ScraperAPI to handle proxies and bypass CAPTCHAs effortlessly.

👉 Start your free trial with ScraperAPI today: [https://www.scraperapi.com/?fp_ref=coupons](https://www.scraperapi.com/?fp_ref=coupons)

---

## 5. Related Documentation

- GooSeeker Glossary of Terms  
- Python Open Source Project for GooSeeker

---

## 6. GooSeeker Open Source Code Repository

Access GooSeeker’s open-source Python web scraping project to learn more about integrating content extractors and improving your scraping workflow.

---

## 7. Document Revision History

Track updates and modifications to this guide in the document history section.

