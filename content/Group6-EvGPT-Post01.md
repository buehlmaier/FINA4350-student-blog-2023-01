---
Title: Bag Of words (by "Group 6")
Date: 2023-02-05 12:12
Category: Progress Report
Tags: Group 6 EvGPT
---



*Author: Fung Ho Kit*  
*Co-Author: Yang Fan, Zhu Jiarui, Li Xinran*  

During the lecture, Dr.BUEHLMAIER showed us the monitoring program for Twitter. The program catches my message and downloads it on the laptop. While the program can capture the tweets containing words that are specified in a word bank, a question appeared in my mind - What kind of words should I monitor?

I think the situations should be complicated given the complex operations in the financial markets. By applying the divide-and-conquer method, a categorization should work well to better classify the various need in different situations

From my point of view, a possible way to categorize is that we divided the whole pie into:

- i) Capital markets (bond, equity securities, derivative securities);
- iv) Cryptocurrency markets

The reasons for my division are:

- i) The first one is the knowledge I obtained from the course [FINA2320] and [FINA2330], I think that there are different focuses in these different markets, and a separation of word bank increase the possibility that we can scrape useful information. For example, the stock markets should focus on the companies' behaviors. Once we fix the portfolio of stocks, we can closely monitor the tweet related to those companies' operations, such as a large cash flow or some agency problem that occurred.

- ii) The second one is the learning I gained from my current internship experience [Optymize Potocol @Optymize_xyz]. I noticed that it is an increasing new market in web3. The behaviors of the market are greatly different from those of traditional markets. More attention should be put on the scam issue, the extent of success in building the community, and so on.

- iii) I do not add the money market into the word banks. It is because the money markets feature in short YTM (year to maturity) and higher opportunity costs. The money market often yields just 2% or 3% while common stocks have returned about 8% to 10% on average. Although the money market owns the advantages of liquidity, monitoring this market is against the initial purpose of this project, which is to create text analytics for better investment strategies in a longer time interval. Thus, the money market is filtered out.

Considering these reasons, I think it is practical, necessary, and efficient to set up multiple word banks for different markets to yield high performance in scraping information for financial analytics.

After the separation of markets, work bank construction is rather essential. Here are some initial thoughts on the focus on each markets.

For crypto market, things are more uncertain and usually out of our expectation. From my point of view, we cannot rely on traditional financial insights to analysis the market trend heavily. First of all, we should focus on the bear market and bull market discussion. The general environment usually are the consequences of multiple aspects, such as policies posed on digital assets, new machanism on chain and so on. Monitoring some gaints and OGs on twitter and Analysis on their prediction on the conditions insides the crypto market is very important. It will help us avoid risk in the investment of crypto.
