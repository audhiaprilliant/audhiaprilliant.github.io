---
layout: post
title:  "Twitter Data Visualization (case study: Indonesia General Election 2019)"
author: audhi
categories: [ data visualization,rprogramming ]
tags: [data mining]
image: assets/images/13-0.jpg
---

### Overview
For this project, we use primary data of Twitter by crawling on 28th - 29th of May 2019. Further, the data is in csv format (comma delimited). It refers into two topics, the data of Joko Widodo which contains the keyword "Joko Widodo" and the other one is the data of Prabowo Subianto that has the keyword "Prabowo Subianto". These include several variables and information in order to determine user sentiment. Actually, the data has 16 variables or attributes and more than 1000 observations (for both data). Some variables are listed in Table 1.
<p align="center">
  <img src="{{ site.baseurl }}/assets/images/13-13.jpg" width="750" />
</p>

### Data Exploration
Data exploration aims to get any information and insight from Twitter data. It should be noted that the data had been conducted text pre-processing. Exploration is carried out for variables which are considered quite interesting to be discussed. For instance, the variable of `Created`
<p align="center">
  <img src="{{ site.baseurl }}/assets/images/13-1.png" width="350" />
  <img src="{{ site.baseurl }}/assets/images/13-2.png" width="350" />
  <div class="caption" style="align: left; text-align:center;">Figure 1</div>
</p>

Based on Figure 1, we can conclude that the number of tweets by data crawling (for keyword "Jokow Widodo" and "Praabowo Subianto") is not similar, even though in the same date. For instance on Figure 1 (left), visually it refers that for tweets with the keyword "Joko Widodo" only obtained on 28th of May 2019 in the period of 03:00 - 17:00 WIB. While, on the Figure 1 (right), we conclude that tweets with keyword "Prabowo Subianto" obtained on 28th - 29th of May 2019 in the period of 12:00 - 23:59 WIB (28th of May 2019) and 00:00 - 15:00 WIB (29th of May 2019).
<p align="center">
  <img src="{{ site.baseurl }}/assets/images/13-3.png" width="350" />
  <img src="{{ site.baseurl }}/assets/images/13-4.png" width="350" />
  <div class="caption" style="align: left; text-align:center;">Figure 2</div>
</p>

Based on Figure 2, we get the significant differences between users who tweet using keyword "Joko Widodo" and "Prabowo Subianto". Tweets with the keyword "Joko Widodo" tend to be so intense talking about Joko Widodo at certain time (07:00 - 09:00 WIB) with the most number of tweets at 08:00 WIB. It has 348 tweets. Whereas tweets with the keyword "Prabowo Subianto" tend to be constantly talking about Prabowo Subianto during the date of 28th - 29th of May 2019. The average of tweets with the keyword "Prabowo Subianto" per hour uploaded on 28th - 29th of May 2019 is 36 tweets.
<p align="center">
  <img src="{{ site.baseurl }}/assets/images/13-5.png" width="350" />
  <img src="{{ site.baseurl }}/assets/images/13-6.png" width="350" />
  <div class="caption" style="align: left; text-align:center;">Figure 3</div>
</p>

Figure 3 is the barplot of number of tweets with keyword "Joko Widodo" and "Prabowo Subianto" on 28th - 29th of May 2019. Based on Figure 3 (left), it can be concluded that the average of tweets per hour uploaded by users which are talking about Prabowo Subianto is less than the total of tweets in two days of 30 tweets per hour. Twitter users are less active talking about Prabowo Subianto at 19:00 - 23:59 WIB. It is caused by the breaktime of Indonesian. However, the tweets with its topic always update at midnight because of users who live abroad nor users who are still active. Then, the users starts the activities at 04:00 WIB to its peak at 07:00 WIB, then drops until it rises again at 12:00 WIB.
<p align="center">
  <img src="{{ site.baseurl }}/assets/images/13-7.png" width="350" />
  <img src="{{ site.baseurl }}/assets/images/13-8.png" width="350" />
  <div class="caption" style="align: left; text-align:center;">Figure 4</div>
</p>

Figure 4 is the density plot of sentiment score that contains the keyword of "Joko Widodo" and "Prabowo Subianto". The score for tweets are obtained by the average of score for root words making up the tweets. So, its score is given for each root words and has value between -10 to 10. If the score is smaller, then the more negative sentiment for words in the tweets and vice versa. Based on Figure 4 (left), it can be concluded that the negative sentiment for tweets containing the keyword "Joko Widodo" ranges from -10 to -1 with a median score of -4. It also applies to the positive sentiment (with positive score, of course). Based on the density plot on Figure 4 (left), it is found that the scores for positive sentiment has quite small variance. So, we conclude that the positive sentiment for tweets containing keyword "Joko Widodo" is not too diverse.

Figure 4 (right) shows the density plot of sentiment score which contains keyword "Prabowo Subianto". It is different to the Figure 4 (left) because the negative sentiment on Figure 4 (right) ranges from -8 to -1. It implies that the tweets don't have too much negative sentiment (tweets have negative sentiment, but not high enough). in addition, the distribution of negative sentiment scores has two peaks in the range of 4 and 1. Whereas, positive sentiment ranges from 1 to 10. In contrast to Figure 4 (left), the positive sentiment on Figure 4 (right) has high variance and has two peaks in the range of 3 and 10. This shows that the tweets containing the keyword "Prabowo Subianto" have the high of positive sentiment.
<p align="center">
  <img src="{{ site.baseurl }}/assets/images/13-9.png" width="350" />
  <img src="{{ site.baseurl }}/assets/images/13-10.png" width="350" />
  <div class="caption" style="align: left; text-align:center;">Figure 5</div>
</p>

Figure 5 is the summary of sentiment score for tweets which have been categorized into negative, neutral, and positive sentiment. The negative sentiment is a sentiment that has the score less than zero. The neutral is the sentiment that has the score equals to zero. Wheres, the positive sentiment has the score greater than zero. According to the Figure 5, it is obtained that the tweets with keyword "Joko Widodo" have the percentage of negative sentiment less than tweets with keyword "Prabowo Subianto". It has the differences of percentage of 6.3%. It is also found that the tweets containing the keyword "Joko Widodo" have a greater percentage of neutral sentiment and positive sentiment compared to the tweets with keyword "Prabowo Subianto". Exploration through piechart found that tweets with the keyword "Joko Widodo" tend to have aa greater percentage of positive sentiment compared to tweets with the keyword "Prabowo Subianto". But through the density plot, it is found that the distribution of positive and negative sentiment score shows that tweets containing keyword "Prabowo Subianto" tends to have a greater sentiment score compared to "Joko Widodo". It must conduct further analysis.
<p align="center">
  <img src="{{ site.baseurl }}/assets/images/13-11.png" width="350" />
  <img src="{{ site.baseurl }}/assets/images/13-12.png" width="350" />
  <div class="caption" style="align: left; text-align:center;">Figure 6</div>
</p>

Figure 6 shows the terms or words in tweets (keyword "Joko Widodo" and "Prabowo Subianto") that are often uploaded by users on 28th - 29th of May 2019. Through this WordCloud visualization, it can be found hot topics that are subject of discussion for keywords. For tweets containing the keyword "Joko Widodo", it is found that the terms "tuang", "petisi", "negara", "aman", and "nusantara" are top five terms with the highest number of occurrences in each tweets. Whereas, the tweets containing keyword "Joko Widodo" found that the term "Prabowo", "Subianto", "kriminalisasi", "selamat", and "dubai" are the top five terms with the highest number of occurrences in each tweets. This indirectly shows the pattern of tweets uploaded with the keyword "Prabowo Subianto" ie: every tweet uploaded almost certainly contains the name "Prabowo Subianto" directly, not through mention (@). This is because in text pre-processing, mention (@) has been removed.

### Sources
<a target="_blank" href="https://haikawanku.wordpress.com/2019/11/08/analisis-sentimen-pasca-pemilihan-umum-indonesia-tahun-2019-melalui-twitter-dengan-metode-naive-bayes/" class="btn btn-danger">Personal Blog</a> <a target="_blank" href="https://github.com/audhiaprilliant/Indonesia-Public-Election-Twitter-Sentiment-Analysis" class="btn btn-warning">Personal GitHub</a>
