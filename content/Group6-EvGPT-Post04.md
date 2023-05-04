---
Title: Dara processing (by "Group 6")
Date: 2023-04-25 12:12
Category: Progress Report
Tags: Group 6 EvGPT
---


*Author: Li Xinran*
*Co-Author: Fung Ho Kit, Yang Fan, Zhu Jiarui*

After scraping the text data from various platforms, we need to process it before feeding it into the model. FinBERT supports many common English punctuations and can split a paragraph into sentences by itself. It saved us a lot of work and what we did in the data processing part was mainly cleaning. During cleaning, the main difficulty we ran into was dealing with invalid characters.

At first glance, we noticed that there are some non-English letters (e.g., Ä) in the texts, and we thought: maybe those text entries are in other languages and we should just drop those observations. Once we took a closer look at the data, we found that:

1) Those non-English letters are actually the only "non-English composition" of the text entries where they appear, apart from them, the rest of the text is more or less a valid English paragraph.

2) The appearances of the non-English characters in different text entries show some pattern: certain combinations of those characters appear repetitively (e.g., Äô).

We then guess that those non-English characters were actually punctuations that got messy in the process of scraping. This was proved to be true when we opened the webpage where one of those texts was from.

In this case, we should not simply drop (by filtering out rows with non-Ascii characters) those observations containing invalid characters, because it would be a waste of data. What we did was to look through enough data, and try to find the mappings (e.g., ‚Äô is actually ') for, if not all, most of the "messy punctuations". Only after replacing them with their original form, we use the "Ascii filter" to drop the rows containing invalid characters (e.g., the full stop from the Chinese input method), which our model cannot process.
