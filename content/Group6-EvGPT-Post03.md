---
Title: Scraping matters (by "Group 6")
Date: 2023-04-24 12:12
Category: Progress Report
Tags: Group 6 EvGPT
---

*Author: Yang Fan*
*Co-Author: Fung Ho Kit, Li Xinran, Zhu Jiarui*

## What to Scrape
After we had settled on the theme, we decided to scrape on mainstream news media and social media platforms. Considering our selection are all US technology companies, The Wall Street Journal, Consumer News and Business Channel, Bloomberg, and Yahoo News are news media. Social media will focus on Twitter.

## How to Scrape
Web scraping is the process of extracting data from websites using automated tools. It involves sending HTTP requests to websites, retrieving HTML content, and parsing the data to extract relevant information. We use Python and several libraries for our project, including Requests, BeautifulSoup, and Selenium.

## **TWITTER**
This section is listed separately because Twitter scraping failed.

We first started using Twint, and with the workaround in its GitHub Issue #1433, Twint was confirmed to work still.
However, after a large number of requests for data, we found that Twint was not officially maintained after the Twitter API was banned, and its "until" function was not working, so we could only read the first ten days of content. Once the request is over ten days, Twint can only retrieve a few tweets, and other people have also mentioned this in issues, but there currently lacks a solution.

Updated on March 30, Twint has been archived by the owner and it is now read-only.

We started using another approach by Selenium. This method is not flexible but effective enough. The only disadvantage we are facing is that the load time of the web server is too long, and we could only scrape like around 100 tweets per minute. From the previous result of Twint, we know that there is more than 20k tweets a day on the topic of "Google", that means we would take more than four hours to scrape. So if we want to scrape the six topics' tweets a day, we would need a day. The time effort is too large, and we decide to use the four news media only.
