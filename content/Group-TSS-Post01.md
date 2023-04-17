---
Title: NLP Project's Blog Post (by "Group TSS")
Date: 2023-04-14
Category: Progress Report
Tags: TSS
---

By Group "TSS"

#Microsofts' Sales Growth Forecast

###Introduction

In deciding our topic for this project, our goal was to focus on something that could have a useful and intuitive application in the context of finance and the investment scenario. Thinking about the main factors that influence stock investment decisions and the performance of companies, we decided that projecting the growth of sales using NLP and text analytics for a determined stock/company would be a great fit for our project objectives. As our target company, we decided to go with Microsoft; given that this company is so diverse in terms of the products they offer, and that it is one of the companies with the Cap rate (capitalization rate) in the technology industry and in the SP&500, we figured, it would provide a convenient and reliable dataset for our analysis.

###Motivation

As mentioned previously, projecting the sales growth rate for a company is one of the most significant aspects of analyzing a company's performance expectations, and this is our main motivation for this project. When valuing a company – whether one does it through DCF (Discounted Cash Flows) or any other cash flow valuation method – the single, most relevant assumption to construct in one’s financial model, is the sales figure which would be projected into the future since all the other financial statement accounts would depend on this input. This is the reason why we believe focusing on the sales growth rate for a company is so significant in financial markets and investments decisions. 

###Goal

As mentioned previously, projecting the sales growth rate for a company is one of the most significant aspects of analyzing a company's performance expectations, and this is our main motivation for this project. When valuing a company – whether one does it through DCF (Discounted Cash Flows) or any other cash flow valuation method – the single, most relevant assumption to construct in one’s financial model, is the sales figure which would be projected into the future since all the other financial statement accounts would depend on this input. This is the reason why we believe focusing on the sales growth rate for a company is so significant in financial markets and investments decisions. 

###Target Company: Microsoft

While doing our research to select a target company, we believed that Microsoft really portrayed all our specific requirements to accomplish our project goal. The company was founded more than 40 years ago, and it has been publicly traded since 1986, thus it definitely provides extensive financial data for our analysis. Related to our sentiment analysis, Microsoft’s widely used and popular products provide us with a exceptionally strong foundation for our consumer sentiment analysis; we decided to focus on some of Microsoft’s’ most popular and most reviewed online apps from the google play store for our sentiment analysis source and dataset. Those online apps include Microsoft Teams, Microsoft 365 Office, and Microsoft Authenticator. Overall, we are confident that the data we would obtain from this company would set the right stage for our model development.

###Methodology
1. *Data Source*

As mentioned before, some of Microsoft’s apps will constitute a big part of our data sources. The objective is to apply text analytics to the mobile app store user comments. We used Google Play and Apple App Store for the user comments input. Google Play Store has a specialized package for web scraping called google-play-scraper, which we used on top of BeautifulSoup. The Apple App Store has a similar package called app_store_scraper. We focused on these two app stores because they provide large amounts of user comments across popular products and time periods, and a more centralized channel for application downloading.

Some of the challenges encountered while collecting these data were related to the immense number of reviews for the products; the user comments of popular apps, i.e., Outlook, Microsoft365, exceeded 100,000 from 2021 to now. Moreover, such a large scale of data may overwhelm the web scraping process. Other challenges were due to the Apple App Store, which has a sensitive detector on mass web scraping that will block the frequently visited IP address.

- Sample code from Apple app store web scraping:

```
 {
   "cell_type": "code",
   "execution_count": 3,
   "id": "f583060c",
   "metadata": {},
   "outputs": [],
   "source": [
    "from app_store_scraper import AppStore\n",
    "\n",
    "import pandas as pd\n",
    "\n",
    "import numpy as np\n",
    "\n",
    "import json\n",
    "\n",
    "import os"
   ]
```

   - Sample code from Google Play app store web scraping:

```
{
   "cell_type": "code",
   "execution_count": 3,
   "id": "da9b3602",
   "metadata": {},
   "outputs": [],
   "source": [
    "from google_play_scraper import Sort, reviews_all, reviews"
   ]
  },
```

Our other data input is the sales figures for the last three years (12 quarters) for Microsoft. We obtained these figures from Microsoft's quarterly (10Qs) and Yearly reports (10Ks) as reported on the [Edgar website](https://www.sec.gov/edgar/browse/?CIK=789019&owner=exclude).

 
  Year  |Q1     |Q2     |Q3     |Q4     |
----|-------|-------|-------|-------|
2020|	35,021|	38,033|	37,154|	43,076|
2021|	41,706|	46,152|	45,317|	51,728|
2022|	49,360|	51,865|	50,122|	52,747|

The previous sales figures data will be used as a benchmark for model testing. The model will be based on the sentiments analysis performed on the customer’s reviews.

2. *Data Processing*

One major challenge we encountered obtaining the review data from the app stores was cleaning and formatting the data. Given the presence of emojis in the review comments, we had figured out a way to transform those emojis images into textual data. 

- Sample code from the emoji conversion process:

```
{
 "cells": [
  {
   "cell_type": "markdown",
   "id": "e45006b8",
   "metadata": {},
   "source": [
    "# Textual Data Process\n",
    "### This part will do data cleaning and manipulation of product's reviews extracted by web scraper. Methods includes: \n",
    "- Emoji conversion \n",
    "- Stop word removal (e.g. 'a', 'the')\n",
    "- Lower case conversion (e.g., 'Lower' -> 'lower')\n",
    "- Stemming and lemmatization (e.g., 'doing' -> 'do')\n",
    "- Store as _cleaned.csv"
   ]
  },
```

  3. *Training Method*

For our training method, we are going to focus on Bag of words and word tokenization to process and manipulate the text related to our sentiment analysis data from the customer’s reviews. The challenging part about our sentiment analysis was managing the overwhelming quantity of data obtained from the product’s reviews, and making sure that the scores assigned to the comments were correctly qualified as positive or negative given the context of the comment.

- Sample output data from sentiment analysis:

```
  "                                                  content  score  \\\n",
       "0                                 easi use user friendli       5   \n",
       "1        easi use help tool smooth busi  smilingfacewi...      5   \n",
       "2                                            great updat       5   \n",
       "3        okay  usng  tri  slightlysmilingfaceembarrass...      5   \n",
       "4                                               work well      5   \n",
       "...                                                   ...    ...   \n",
       "148555                                              excel      5   \n",
       "148556                                          great app      5   \n",
       "148557                     fast  easi  effect  could ask       5   \n",
       "148558                                    nguthi iryt yaz      5   \n",
       "148559                                              enjoy      5   \n",
```



  4. *Testing & Model Evaluation*

Possibly metrics to be used are Accuracy, F1 score, or Confusion Matrix to do back-testing on our model.

5. *Visualization*

We still have to complete our model before deciding on how to make it visual, but some options include:

- Histograms
- Word clouds
- Scatter plot
- Heatmaps

Even though it has been quite challenging to work with this great amount of data, and also making sure we both, as a collaborative team, work efficiently together, we are working hard to achieve our goal, and to construct a reliable, effective model. We are aware that we may enounter more challenges along the way but at the end, we know we can only learn from our mistakes.
