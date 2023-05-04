---
Title: Challenges encountered in Data Preprocessing and Topic Modelling on FOMC and FED data (by "Group 2")
Date: 2023-04-25 10:00
Category: Progress Report
Tags: Group2
---

By Group 2

## Intro and Objectives
The objective of our project is to predict the rate hike rate using official FED materials such as FOMC meeting minutes, FOMC statements, FED speeches and FED testimonies. As inflation remains elevated, the Fed raised the fed funds rate by 25bps to 4.75%-5% in March 2023, matching the February increase, and pushing borrowing costs to new highs since 2007. We decided on this topic as we think interest rate is imperative to every stakeholder in an economy. The general belief is that by increasing rates, borrowing cost rises and hinders the ability of consumers and businesses to obtain short-term credit. As investors ourselves, our group has decided to predict rate hike as the stock and bond market is very sensitive to interest rate and rate hike announcements. Furthermore, a higher interest rate following a rate hike announcement often causes rising dollars against foreign currencies.

## Our difficulties and approaches to overcome it:
#### Problem 1: Difficult to remove participants name in FOMC minutes

During the pre-processing stage of our data, we encountered difficulties when scrapping the FOMC minutes as the data is relatively unstructured. The coverage for the FOMC minutes were different across years, which made it even harder to standardize them to remove unrelated text before the data merging stage.

    def removeUnrelatedText(x):
        # remove words before the line
        if x.find('By unanimous vote, the minutes of the meeting of the Federal Open Market Committee held on')>0:
            x = x.split('By unanimous vote, the minutes of the meeting of the Federal Open Market Committee held on')
            if len(x)>=1:
                x = " ".join(x[1:])
                x = x[x.find('were approved.')+14:]
                return x
        else:
            return x

  Luckily, we noticed a pattern across all years of FOMC minutes. The majority of the minutes has the line “By unanimous vote, …” and before this line shows the unwanted data such as the participants of the FOMC meetings. This data is considered useless for our data analysis part and has to be removed before feeding into our machine learning model. By using the .split method, unwanted texts before the line “By unanimous vote,…” can be successfully removed. Hence, we successfully trimmed out a large chunk of text that is unwanted.
  Through this process, we’ve learnt the importance of finding a systematic way to identify patterns across different datasets in the data pre-processing stage. That is one of the biggest takeaways we’ve learnt in the process.



#### Problem 2: Difficulty in reading large csv files
We encountered a problem that our program could not read large csv files compiled from the collected data. It turns out the Python standard library CSV module enforces a default field size limit on columns, and anything with more than 128KB of text in a column will raise an error.

    maxInt = sys.maxsize
    while True:
        # decrease the maxInt value by factor 10
        # as long as the OverflowError occurs, we catch the error
        try:
            csv.field_size_limit(maxInt)
            break
        except OverflowError:
            maxInt = int(maxInt/10)
    df = pd.read_csv('FOMC meeting minutes.csv',engine="python",converters={"index":pd.to_datetime,"Federal_Reserve_Mins":str})

Our group then standardized our approach to this problem and modify this error by using the csv.field_size_limit(new_limit) function. However, we then faced with Overflow Error in some of our groupmate's computer. To solve this, we added try except function to makesure the maxInt is under the limit and will not cause Overflow error.

Moveover, we added converters to our dataframe. It is beacuse we found that when we save a dataframe to csv, the date field will become string.

## Topic Modelling: Keeping our input textual data free from excessive and unwanted information
One of the major concerns for us is to minimize the amount of unrelated textual data into our model. We encountered some difficulties by removing unncecessary words from FOMC meeting minutes and speeches, as we want to be close to remove every single word that is unrelevant. When we are preprocessing the data from FED speeches, we found out that some of the speeches are unrelated to rate hikes. Hence, after thorough consideration, we decided to do topic modelling to improve the quality of our input data.

    df = pd.read_csv("speeches_preprocessed.csv",engine='python',converters={"Date":pd.to_datetime,"Content":str})

    #vectorize the content for topic clustering
    vectorizer = TfidfVectorizer(stop_words='english',\
                                max_features= 1000,
                                max_df = 0.5, 
                                smooth_idf=True)

    X = vectorizer.fit_transform(df['Content'])

We first applied TF-IDF technique, which is a form of text vectorization depending on the term frequency and document frequency of the term. It uses a special technique for converting text into finitie length vectors. Rather than weighting on the importance of words, TF-IDF is a special form of BoW model to provide insights on what is considered more relevant and less relevant words in a document. We therefore leveraged this technique before doing topic clustering.

     # fit SVD model for topic clustering
    svd_model = TruncatedSVD(n_components=20, algorithm='randomized', n_iter=100, random_state=122)
    lsd = svd_model.fit_transform(X)

    terms = vectorizer.get_feature_names_out()

Afterwards, we used Truncated SVD, which is a technique for dimension reduction, for topic modelling. We applied Truncated SVD on TF-TDF transformed data to cluster the speeches into 20 cluster groups. Each speech in the cluster groups will have similar topics. 

    #print out terms to check which topics are relevant
    for i, comp in enumerate(svd_model.components_):
        terms_comp = zip(terms, comp)
        sorted_terms = sorted(terms_comp, key= lambda x:x[1], reverse=True)[:15]
        print("Topic "+str(i)+": ")
        for t in sorted_terms:
            print(t[0])
        print(" ")

We then output the top 15 relevant words (features) of each topic. For example, topics 0 to 3:

![Picture showing different Topics]({static}/images/Group-2-Post01_Topics.png)

We can see that topic 0 and 1 are most relevant to rate hike prediction and topics 2 and 3 are less. Thus, we only keep speeches of topics 0 and 1 for machine learning.




