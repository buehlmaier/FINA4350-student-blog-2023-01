---
Title: Literature reviews (by "Group 6")
Date: 2023-03-24 12:12
Category: Progress Report
Tags: Group 6 EvGPT
---

*Author: Zhu Jiarui*
*Co-Author: Fung Ho Kit, Yang Fan, Li Xinran*

To start our project which aims to generate a framwork which can forecast stock price with historical stock data and realtime forum discussions, we firstly investigated several exsisting works and here are three papers we found might be useful for our project.

## **CNN**: Deep Convolutional Neural Networks for Sentiment Analysis of Short Texts
*Dos Santos, C., & Gatti, M. (2014, August).*

This research conducted by IBM Research proposed **CharSCNN** (Character to Sentence Convolutional Neural Network), uses two convolutional layers to extract relevant features from words and sentences of any size.

1. Initial Representation Levels embeddings

    1. Word-Level Embeddings

        matrix multiplication

    2. Character-Level Embeddings

        character-wise CNN

2. Sentence-Level Representation and Scoring

    Produce local features around each word in the sentence and then combines them using a max operation to create a fixed-sized feature vector for the sentence.

Although this paper only conducted polarity analysis and classify the news into positice/negitive only, it generates a unique score for each news and filter it by a certain threshold. We could modify it and keep the score information in our work.


## **BERT**: FinBERT: Financial Sentiment Analysis with Pre-trained Language Models
*Araci, D. (2019).*

Researchers from University of Amsterdam FinBERT proposed a language model based on BERT, to tackle NLP tasks in financial domain. After further trained on
TRC2-financial and Financial PhraseBank database, FinBERT got outstanding performance on financial news and discussions.


## **LSTM**: Deep learning for stock prediction using numerical and textual information
*Akita, R., Yoshihara, A., Matsubara, T., & Uehara, K. (2016, June).*

This work published on IEEE proposed a novel application of deep learning models, Paragraph Vector, and Long Short-Term Memory (LSTM), to financial time series forecasting. It provides a workable approach for us to combine text information and historical financial data.

1. Mapping

    Distributed Memory Model of Paragraph Vectors (PV-DM) and Distributed Bag of Words version of Paragraph Vector (PV-DBOW)

2. Financial value converting

    $\Large value_{cn}^t = \frac{2*price_{cn}^t-(max{cn}+min_{cn})}{max_{cn}-min_{cn}}$

After finishing processing the financial and text data individually, the authors utilized the matrix multiplication to unify their vector dimension so that the influence of both data can have similar weights. Then they directly merge the two time sequence of vectors and feed it into LSTM model, so that they wish to forecast the coming stock price.
