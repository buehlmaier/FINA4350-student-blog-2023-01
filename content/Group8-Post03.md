---
Title: Our Group Work Process: Using NLP to Predict Credit Downgrades, Part III (by "Group 8")
Date: 2023-05-04 21:09
Category: Progress Report
Tags: Group8
---

# Our Final Blog: Using NLP to Predict Credit Downgrades

After our second blog, we made significant changes to the analysis of our tokenized words. \
We have not made any changes to the initial stages (webscraping, preprocessing, tokenization).


**Analysis**: We have decided to change our analysis of the training data into two approaches: Clustering and Similarity. \
We had to change our approaches as our initial approach was a human process prone that was too qualitative and prone to flaws such as context.

1.Clustering: For clustering, we used Tf-Idf weighted bag of words to analyze frequency within and across docuemnts. \
These generated vectors based on weighting for the ML model. \
The end product of our analysis comes in the form of a visualization through word cloud.
 
The meaningful parts of the code are shown below.

```
#visualizing tf-idf cloud (code can be reproduced using file clusteringModel.py)
dense = tfidf.todense()
lst1 = dense.tolist()
df = pd.DataFrame(lst1, columns=feature_names)
Cloud = WordCloud(background_color="white").generate_from_frequencies(df.T.sum(axis=1))
plt.imshow(Cloud, interpolation='bilinear')
plt.axis("off")
plt.show()
```

Afterwards, we used the K-means clustering model to find clusters of similar words. \
We used the elbow method to optimize the number of clusters.\
We have spent significant time training the model with more data to increase the accuracy of model generation, \
while also performing text classification analysis to determine which clusters better predict defaults. \
We used this cluster to find the words that would signal future default.

The meaningful parts of the code are shown below.

```
#our k-means model (code can be reproduced using file clusteringModel.py))
true_k = 3 # change as needed
model = KMeans(n_clusters = true_k, init = 'k-means++', n_init = 1)
model.fit(tfidf)

# printing top terms per cluster
print("Top terms per cluster:")
order_centroids = model.cluster_centers_.argsort()[:, ::-1]
for i in range(true_k):
    print("Cluster %d:" % i),
    for ind in order_centroids[i, :10]:
        print(' %s' % feature_names[ind]),
    print("\n")
    
# making predictions
temp = v.fit_transform(testData)
prediction = model.fit_predict(temp)
print(prediction)
```


2.Similarity Approach: For the similarity approach, we used the doc2vec neural network to estimate the relationship between words when turning them into a vector. \
This was done by considering the context with neighbouring terms, meaning the word order matters.

The meaningful parts of the code are shown below.

```
# model for word embeddings (code can be reproduced using file doc2vec.py)
model = Doc2Vec([TaggedDocument(doc, [i]) for i, doc in enumerate(textList)], workers = 2, min_count = 3)
similar_word = model.wv.most_similar('default')
print(similar_word)
```

We wondered what the most effective neighbouring window was, given that context is dependent on the neighbouring window \
and too large of a neighbouring window would make the process too slow.\
Furthermore, there are foreseeable limitations. It is dependent on the vocabulary in the training data, meaning that out-of-vocabulary (OOV) words are not recognized by the model.

We then used soft-cosine measure to predict the similarity between documents based on contextual relationships from words. \
Since soft-cosine measure struggles with vector geometry, we incorporated the word embeddings from doc2vec so that the context is considered when the documents are compared. \
The end goal is to look analyze the similarity of new reports to our training data and see whether the downgrades would likely lead to default.

The meaningful parts of the code are shown below.

```
# SCM (code can be reproduced using file doc2vec.py)
termsim_index = WordEmbeddingSimilarityIndex(model)
termsim_matrix = SparseTermSimilarityMatrix(termsim_index, dictionary, tfidf)

def SCM(docA, docB):
    similarity = termsim_matrix.inner_product(docA, docB, normalized=(True, True))
    
# testing

for ind in range(len(textList)):
    joinedText = ' '.join(textList[ind])
    textList[ind] = joinedText
    
for ind in range(len(testData)):
    joinedText = ' '.join(testData[ind])
    testData[ind] = joinedText

print([SCM(textList[0], test) for test in testData])
```

**Results and Reflection**:
The biggest challenge we faced with our project was obtaining meaningful results. For example, \
we were not happy with the clustering of word groups provided by the model. As a result, we spent extra time training the model with more data.

The complexity of our project presented one of our biggest challenges. We set ambitious goals, resulting in a highly technical project. \
Furthermore, our initial planning lacked specificity, and we didn't have a clear vision of the intended outcome or deliverable. \
Additionally, our group had a weak understanding of the capabilities of machine learning. \
This made it challenging to plan the necessary steps and time needed to execute the project successfully. \
As a result, we had to continually revise the methodology of our analysis to ensure we were on the right track.

In addition to this, we found it challenging to communicate as a group online, as most of us had little understanding of the coding aspects of this project. \
Many of the tasks were also dependent on the completion of tasks by other people. \
We were also very unfamiliar with collaborative coding tools, such as version control. \ 
However, towards the end of the project, we improved our collaborative coding by using strategies such as pair coding.

For the soft cosine method, we lacked sufficient training data to make our measurements accurate. \
Small changes in the model can create huge changes in the measurement, making uncertainty of this model quite big.

From a bigger picture, our initial assumption of using downgrade reports as a proxy/indication of credit health deterioration can be challenged. \
We could have considered expanding on our testing data to include other financial data such as corporate bond valuations to make the testing data more comprehensive. \
The project design would be improved if we had a better financial understanding of bonds and the data commonly used to measure credit health deterioration.

For a comprehensive write-up of our results, please refer to our group report. To access the entire project code files, please visit our GitHub page.

This is the final blog for our project. We have tried to illustrate the challenges we faced and the changes we made throughout the project. \
We hope there were some useful insights for anyone reading!




