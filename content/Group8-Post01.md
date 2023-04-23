---
Title: Our Group Work Process: Using NLP to Predict Credit Downgrades (by "Group 8")
Date: 2022-03-28 21:09
Category: Progress Report
Tags: Group8
---

We just finished our proposal presentation and wanted to take a moment to reflect on our group work process. Our project was initially inspired by a past group’s work on fraud detection. We wondered if we could do something similar and apply it to the credit space for a project that seemed feasible.

1. **Project overview**: Our project looks to use natural language processing (NLP) processes to see if publicly available information could be used to predict the likelihood of a credit downgrade. Credit was an interesting focus given recent events with the China Real Estate market. It seemed like the causes of declines in credit rating across an industry were all very similar and due to an external factor.

2. **Group dynamics**: Our group dynamics were unique in that only Toby had extensive coding experience, while the rest of us had finance and credit experience. Despite this, we were able to come together and develop a plan. We plan to focus on the China real estate industry and look at past downgrades to train the machine learning model or to select a bag of words from credit agency reports and look at the frequency of those words appearing in our web-scraped text.

3. **Initial work**: Our initial work focused on finding the best sources of publicly available information to web-scrape, such as credit downgrades, company earning transcripts, annual reports, along with establishing a set of timelines for companies that were downgraded as case studies we can look at. We believe that by using NLP techniques and analyzing publicly available information, we can create a model that predicts credit downgrades accurately. Currently, we plan to use selenium to webscrape, it seems requests won’t work as there is too much javascript.

We are excited to continue working together and see where our project takes us. Stay tuned for updates on our progress!

##Some Code Snippets:

```#webscraping

from selenium import webdriver
from selenium.webdriver.chrome.service import Service

# from selenium.webdriver.common.keys import Keys

# from selenium.webdriver.common.by import By

from bs4 import BeautifulSoup
import os
import time

home = os.path.expanduser('~')  # Home directory/folder.
driver = webdriver.Chrome(service=Service(home + '/bin/chromedriver'))


# can turn into automatic input with csv file

driver.get('https://www.fitchratings.com/research/corporate-finance/fitch-downgrades-evergrande-subsidiaries-hengda-tianji-to-restricted-default-09-12-2021')


# allow JS/Ajax to load

time.sleep(5)


# filename

title = driver.title.replace(' ', '-') + '.html'


# basic scraping

with open(title, 'w' , encoding='utf-8') as f:
    f.write(driver.page_source)
    s = BeautifulSoup(driver.page_source, 'lxml')
    #print(s.prettify())
    raw_text = s.get_text()
    print(raw_text)

    
# TBC: add pre-processing here

# TBC: add BoW here
```
    
