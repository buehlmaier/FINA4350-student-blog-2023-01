---
Title: Our Group Work Process: Using NLP to Predict Credit Downgrades, Part II (by "Group 8")
Date: 2022-04-30 21:09
Category: Progress Report
Tags: Group8
---


After our midterm progress report, we have made signiifcant strides in completing the scraping of reports from credit rating agencies. We are also in the process of preprocessing and cleaning the data so that it can be tokenized and analyzed. We will then analyze the data using two approaches 1. Human Approach (we create a score) 2. Machine learning model using unsupervised learning for less human bias (clustering)

1. **Webscaping**: The main bottleneck in this stage was adjusting the code to account for each stage of the login page loading, along with pop-ups such as requests for cookies or terms and conditions. From a technical perspective, there was lots of Javascript which meant we had to rely on Selenium to webscrape. It was a little difficult to use Selenium at first, especially when Moody (one of the rating agencies) changed their website format as we finished the first iterataion of the code. This meant we had to recode the scraping process.

```#webscraping
# allow JS/Ajax to load
    WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.XPATH, '//form/div/div/div/input')))
        
    #remove login pop-up
    element = driver.find_element(By.XPATH, '//form/div/div/div/input')
    element.click()
    element.send_keys("fina4350.nlp@gmail.com") # temp email 
    element = driver.find_element(By.XPATH, '//form/div/button')
    element.click()
    WebDriverWait(driver, 5).until(EC.presence_of_element_located((By.XPATH, '//form/div/div[2]/div/input')))
    element = driver.find_element(By.XPATH, '//form/div/div[2]/div/input')
    element.click()
    element.send_keys("fina4350") # temp password
    element = driver.find_element(By.XPATH, '//form/div/button')
    element.click()
    
    # allow JS/Ajax to load
    WebDriverWait(driver, 20).until(EC.presence_of_element_located((By.XPATH, '//main/div[2]')))
    print(driver.title)

```

2. **Preprocessing of Data**: In terms of preprocessing of data, we had to limit the scope of text scraping on the website to limit the text corpus. This meant we had to identify specific HTML elements we wanted to scrape, making the short walk through on HTML covered in class very useful for our project. In terms of preprocessing techniques, we used apha-numeric only, along with lemmitzatio. We still need to get rid of stop words and manual filtering of irrelvant words. Still some ways to go.

3. **Tokenization**: Quite a smooth process, we used NLTK only.

``` #tokenization and preprocessing
wnl = WordNetLemmatizer()
    arr = list([w for w in word_tokenize(text.lower()) if w.isalpha() \
                and len(w) > 2 and not w in set(stopwords.words('english'))])
    
    for tk in arr:
        if tk not in existing_words or tk not in special_words:
                if tk in set(words.words()):
                    stemmed_arr.append(wnl.lemmatize(tk))
                    existing_words.add(tk)
        else:
            stemmed_arr.append(wnl.lemmatize(tk))

```

4. **Analysis**: At first, we were a little confused on how we can conduct the quantitative analysis using the Bag of Words method. Learning of sentimentality in the lectures, we learnt we could apply that concept to generate statistical probabilities (a score from -1 to 1). We can also combine this with frequency to create a score. However, there is bound to be errors as the context of words may be incorrectly interpreted. There was less issue with the Machine Learning model.

5. **Implementation**: We have several initial ideas on how we can implement our analysis on a larger set of data. One idea is that we could use the model from downgrade reports to look at the sentiment on earnings transcripts, but in theory it should work for any textual data that is related to the company. It is important we have a way to adjust to each type of text as the code in webscraping is quite specific to the medium of text.

6. **Forseeable Limitations**: The code will likley run slow. The entire project also relies on the BoW from Moodys only, as we struggled to scrape from S&P and Fitch. The analysis can also only be applied to the China Real Estate market; we are unsure whether it would work for other industries, or even the RE market in other regions. The amount of manual coding word needed to tweak the scraping parameters make our work very unscalable. Our scraping is also prone to errors, and even the small sample size (n=~140) is much smaller than typical studies that have thousands of texts.


We are excited to wrap up the project and submit our final report.




