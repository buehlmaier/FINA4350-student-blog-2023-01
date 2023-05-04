---
Title: NLP Project's Blog Post, Part II (by "Group TSS")
Date: 2023-04-30
Category: Progress Report
Tags: TSS
---

By Group "TSS"

#Model Training: Challenges Encountered	

###1. Web Scraping

As stated before, we used the App store and the Google Play store to obtain the online apps’ user comments data. However, we faced some issues with the app_store_scraper, the web scrapper platform for the App store; it could only fetch 400 entries or users’ comments even though there were millions of user’s comments in the App store for Microsoft’s online apps used. Although we tried to solve this technical issue, it was still giving the same output. Thus, we decided to move forward with using the Google Play store only for the online apps’ reviews. We do not think that this had any negative impact on our model since Google Play store is more widely used than the App store and it contained a greater volume of reviews than the App store. Below is a sample code showing the volume of reviews obtained from the Google Play store.

```
[{'name': 'outlook',
  'google_id': 'com.microsoft.office.outlook',
  'continuation_token_google': <google_play_scraper.features.reviews._ContinuationToken at 0x290aeaa59f0>,
  'reviews_google':                                                   content  score  Year Month
  0                Use for job related info very important!      5  2019    Q3
  1        Not sure if i can unsubscribe to unwanted emails      4  2019    Q3
  2       updated on july 27 and it does not work anymor...      1  2019    Q3
  3                                           very good app      4  2019    Q3
  4       very good though it needs a swifter system to ...      4  2019    Q3
  ...                                                   ...    ...   ...   ...
  449995                                         Works fine      5  2023    Q2
  449996                                 Excellent platform      4  2023    Q2
  449997                                      My favorite!!      5  2023    Q2
  449998                                            Perfect      5  2023    Q2
  449999  Microsoft Expectation is an amazing applicatio...      4  2023    Q2
  ```

  
###2. Sales and Scores: Data Correlation

  Our two data inputs for our NLP model were the quarterly sale figures from Microsoft for the last three years, and the online apps’ reviews for the most popular Microsoft’s online apps (Outlook, Authenticator, Microsoft’s Teams, and Microsoft’s 365). Our end goal was to correlate the scores obtained from the sentiment analysis model applied to the online app’s reviews with the quarterly sales figures, but as we moved forward in our model training, we realized that 1) the consumer sentiment on the online apps did not have such a strong effect on the sales projections, and 2) the sales figures did not match the frequency of the users’ comments (there were many instances of user’s comments and only 12 actual values for the sales figures). 

Related to issue 1, it is important to mention that Microsoft’s online apps still had some correlation to sales figures, but not as much as we expected. What we did to address that was to analyze each online app individually to determine which one had the biggest correlation with sales. After doing that, we determined that Outlook online app was the one with the biggest correlation coefficient, which was 0.11. Below is some sample code of the Outlook correlation analysis.


```
# Outlook's score
# Heatmap
corr_matrix = df_merge_product[df_merge_product['product'] == 'outlook'][['sales','Year', 'score', 'score_vader', 
                                'score_bow_0.2', 'score_bow_0.4', 
                                'score_bow_0.6','score_bow_0.8']].corr()

# Create the heatmap
sns.heatmap(corr_matrix, annot=True)
```

Related to issue 2, the number of user’s comments for a quarter did not match the sales values. So, we made the values match by using the same sales amount for each quarter; thus, for all comments in one quarter, the same sales figure was assigned to each comment for comparison during that quarter. 

Sample code of sales figures and scores values:

```
 "text/plain": [
       "                                             content  score  Year Quarter  \\\n",
       "0                                           easy use      5  2020      Q1   \n",
       "1  fining & reaing important email easier (thanks...      4  2020      Q1   \n",
       "2                                 excellent hany usr      5  2020      Q1   \n",
       "3                                                lov      4  2020      Q1   \n",
       "4                           awesome easy use...... 😊      5  2020      Q1   \n",
       "\n",
       "   score_vader  score_bow_0.2  score_bow_0.4  score_bow_0.6  score_bow_0.8  \\\n",
       "0            4              5              5              5              5   \n",
       "1            5              1              4              4              4   \n",
       "2            4              5              5              5              5   \n",
       "3            2              5              5              5              5   \n",
       "4            5              5              5              5              5   \n",
       "\n",
       "   product  sales  \n",
       "0  outlook  35021  \n",
       "1  outlook  35021  \n",
       "2  outlook  35021  \n",
       "3  outlook  35021  \n",
       "4  outlook  35021  "
      ]
     },
     "execution_count": 49,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "merged_df = df_merge_product.merge(df_sales, on=['Year', 'Quarter'], how='left')\n",
    "\n",
    "df_merge_product['sales'] = merged_df['Revenue_product']\n",
    "\n",
    "df_merge_product.head()"
```

###Final Remarks
We believe that the model could be greatly improved with future work that could address the issues we encountered with the data. For instance, a way our model could be more accurate is finding a dataset more continuous than quarterly sale figures to be able to match the frequency of user’s comments. Moreover, for our consumer sentiment analysis, we could look at other products offered by Microsoft and see if they have a stronger correlation to sales. Nonetheless, even though our model was not completely accurate in providing our sales prediction, it functioned as we trained it and it provided us with the sales growth projection for Q1 2023, which was our project’s end goal.

