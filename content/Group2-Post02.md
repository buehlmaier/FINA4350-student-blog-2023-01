---
Title: Process of Data Collection, Preprocessing and Merging (by "Group 2")
Date: 2023-04-10 10:00
Category: Progress Report
Tags: Group2
---

By Group 2

## Data Collection

We imported FedTools, an open-source Python library for scraping Federal Reserve data, to increase scraping speed during the data collection. Collecting FOMC minutes and statements were quickly done as a result.

```bash
pip install Fedtools
```

```python
from FedTools import MonetaryPolicyCommittee
from FedTools import BeigeBooks
from FedTools import FederalReserveMins
from FedTools import MonetaryPolicyCommittee
from FedTools import BeigeBooks
from FedTools import FederalReserveMins
```

To collect data on FED speeches and testimonies, as we needed to web scrape the FED websites as there are no libraries for speeches and testimonies. 

After analysing the websites' network activities, we found an unofficial API to collect a JSON of the titles, dates and links of all the speeches and testimonies. So, we use requests to get the JSON and parse it. Then for every speech or testimony there, we use requests to visit the links and use BeautifulSoup to parse them, as we found that Javascript is not used to render the content.

Getting list of speeches from unofficial API:

```python
import requests
import json

result = requests.get("https://www.federalreserve.gov/json/ne-speeches.json")

#check if successfully retreived the website
if result.status_code != 200:
    raise Exception("Cannot connet to FED website.")

result = result.content.decode("utf-8-sig") #decode json according to the websites encoding

speeches = json.loads(result) #parse the result as json
```

Rate hikes were directly collected from Bloomberg terminals.  


## Data Preprocessing


As our collected datasets are original documents with tonnes of words, we wanted to reduce the number of terms used as inputs for the machine learning models.  To do so, stemming was done during the data preprocessing stage. As a result, the confusion around words with similar meanings could have been more manageable.


```python
    from nltk.stem import PorterStemmer
    ps = PorterStemmer()
    s = "This is the string to be preprocessed. #you can change this."
    s = re.sub(r"[^a-zA-Z.]", " ",str(s)) #remove symbols except full stop.
    s = re.sub(r'\s+',' ',str(s))
    s = ' '.join([ps.stem(w) for w in s.split() if len(w)>3 and w not in stop_words])
    s = s.lower()
```

The collected dataset of rate hikes was in descending order which was the opposite of the other datasets. Thus we reordered the dataset simply using .iloc method.


```python
data = df.iloc[::-1]

```



## Data Merging

During the data merging of our pre-processed data, we found one condition to consider.  When we cross-checked the dates on the minutes and  FOMC statement, we realised that although the meeting of FOMC was held before the release date of the rate hike, the minutes were released after the rate hike’s release date. Moreover, rate hikes are announced in the FOMC statement. Therefore, we had to change the query we used before to adjust the date.

We merged the pre-processed datasets by subquuries joining the datasets. We join with a condition that the content is released is between dates of rate hikes and the dates of last rate hike.



```python
ratehike["From_Date"] = ratehike['Date'].shift(1) #the last rate hike release date

df = pysqldf("SELECT r.Date, r.From_Date, r.rate_hike, \
            IFNULL((SELECT s.Content FROM speeches s WHERE s.Date >= r.From_Date AND s.Date < r.Date),'') || ' ' ||\
            IFNULL((SELECT t.content FROM testimony t WHERE t.Date >= r.From_Date AND t.Date < r.Date),'') \
            AS data FROM ratehike as r WHERE data <> ' '")            
```


After merging the data set, we realised it needed to be neater to read the rate hike rates as the numbers were in decimals. Therefore, we multiplied the whole column by 10000 to make it into basis point for easier understanding for future reference.
