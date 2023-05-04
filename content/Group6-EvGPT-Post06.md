---
Title: EvGPT Web App (by "Group 6")
Date: 2023-04-30 12:12
Category: Progress Report
Tags: Group 6 EvGPT
---

*Author: Fung Ho Kit*
*Co-Author: Li Xinran, Yang Fan, Zhu Jiarui*

It is actually really important to build a webapp. Development of codes usually are limited locally and the usage also be limited no matter how great your idea is. Therefore, it is necessary to think about how to get your work populated and circulated. There are several ways to implement that, which I have explore for one year or so.

1. Learning Full Stack Techniques: Being full stack means that you can both handle frontend and backend development. The requirements may be a bit hard since you need to acquire the knowledge of HTML/CSS for frontend template setting and JS/Flask/Node.js for backend function. Although I can have the experience in such workflow of development, it is rather difficult considering the short period of semester and potential workload from other courses. There are several benefits for such kind of development. Firstly, it can be more flexible in the content and functions, while the alternative in second methods will have some preset templates. Also, you will have more authority since you are the one hosting the websites.

2. Utilizing some libraries for building lightweight websites. I have multiple exploration on these tools such as Django and streamlit. I think one of the benefits is that these allow you to build a impress website within a short period since they have some templates so that you can use fewer lines to achieve the expected function. Also, they have a easy deployment choice. For example, streamlit have a web server. Therefore, I can deploy my model by just click several buttons. The real process to deploy a self-hosting website require more steps and more expenses on the web domain. There is one dilemma we face before today's final presentation is that our website fail due to the error "Resources out of limit". It seems that they only allow for 1 GB data storage unless you have upgrades on the plans. Therefore, it is better for demonstrating ideas and not for long-term development of websites.


Our current project is also using streamlit to put the code online (https://justinfungi-fina4350-nlp-part4-evegptwebapp-9vlrp3.streamlit.app/). It may not be a very perfect one but should be enough for us to deliver the ideas in our project. it causes me a lot of time to think about how to incorporate the AI model into the WebApp. Thanksfully, things worked out. And in the WebApp, user can choose the company in our portfolio to see the prediction. It is a limitation of our project that the webscraping and Sentiment scores calculation cannot be automated yet. We can only predict the stock price of the days with the text data from webscraping. If the webscraping process can be automated, our model can be more useful considering that it can update the prediction on a daily basis based on the newly scraped text and the stock price
