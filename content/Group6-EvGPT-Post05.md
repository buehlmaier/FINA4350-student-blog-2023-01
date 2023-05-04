---
Title: EveGPT Final Version (by "Group 6")
Date: 2023-04-27 12:12
Category: Progress Report
Tags: Group 6 EveGPT
---

*Author: Fung Ho Kit*
*Co-Author: Li Xinran, Yang Fan, Zhu Jiarui*

While Sentiment Scores are finished, it is excited to build the Step4 - EveGPT.

For the modeling, we use LSTM model to predict the stock price since it is a great model to track time series data. Different from my initial thoughts, i discover that it probably fail if we use stock price to predict stock price. When we try to feed the stock price, the model tend to have a overfitting problem and try to predict the stock price according to what we get in the previous day. After the talk with my friends in the securities asset management company, i got some inspirations on the prediction. Instead of using the stock price which is rather naive and useless, we can manipulate some technical indicators to yield some insights on the momentum and the growth possibility.

There is also another question to think about - how many data i should use. it is not so difficult to retrieve information like stock price or statement nowadays. However, the key is that how can we use them. After researching more on different paper, it seems that we can manipulate the Look-back Method. Instead of 1-to-1 matching, we use n-to-1 matching which means we can consider the past n days data to predict the trend and have a conclusion on the stock price. By referencing more past data, our model can have the understanding of "Trend", which can help us have a better prediction

Another challenge is that how can we evaluate our model. While the prediction may fluctuate with the stock price, it is a unbiased and efficient way to assess our model. This question is proposed by our professor in the final presentation. This is rather important and there is no such a clear solution for question. There are papers using common loss such as RMSE or MAE to evaluate the model, but the loss is not so efficient. It should be a great direction to explore.
