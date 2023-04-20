---
layout: post
title:  "Visualizing Twitter Data: A Case Study of the 2019 Indonesian General Election"
author: audhi
categories: [ data visualization,rprogramming ]
tags: [data mining]
image: assets/images/13-0.jpg
---

### Overview
In this project, primary data was collected from Twitter by crawling on 28th - 29th of May 2019. The data is stored in csv format (comma delimited) and consists of two topics - Joko Widodo, which contains the keyword "Joko Widodo" and Prabowo Subianto, which has the keyword "Prabowo Subianto". The data includes various variables and information to determine user sentiment. In total, there are 16 variables or attributes and over 1000 observations (for both datasets). Some of the variables are listed in Table 1.
<p align="center">
  <img src="{{ site.baseurl }}/assets/images/13-13.jpg" width="750" />
</p>

### Data Exploration
The objective of data exploration is to gain insights and information from the Twitter data. It is worth noting that the data underwent text pre-processing prior to the exploration process. The exploration is focused on variables that are deemed relevant and interesting for analysis. For instance, the variable of `Created`
<p align="center">
  <img src="{{ site.baseurl }}/assets/images/13-1.png" width="350" />
  <img src="{{ site.baseurl }}/assets/images/13-2.png" width="350" />
  <div class="caption" style="align: left; text-align:center;">Figure 1</div>
</p>

Based on the analysis presented in Figure 1, it can be inferred that the number of tweets obtained through data crawling with the keywords "Joko Widodo" and "Prabowo Subianto" is not similar, despite being conducted on the same date. The data shows that tweets containing the keyword "Joko Widodo" were only obtained on May 28th, 2019, during the period of 03:00 - 17:00 WIB, as depicted in Figure 1 (left). In contrast, tweets with the keyword "Prabowo Subianto" were obtained over a longer period, from May 28th to May 29th, 2019, during the period of 12:00 - 23:59 WIB (May 28th, 2019) and 00:00 - 15:00 WIB (May 29th, 2019), as shown in Figure 1 (right).
<p align="center">
  <img src="{{ site.baseurl }}/assets/images/13-3.png" width="350" />
  <img src="{{ site.baseurl }}/assets/images/13-4.png" width="350" />
  <div class="caption" style="align: left; text-align:center;">Figure 2</div>
</p>

Based on Figure 2, a significant difference can be observed between users who tweeted using the keyword "Joko Widodo" and "Prabowo Subianto". Tweets with the keyword "Joko Widodo" tend to be more intense during a certain time period, particularly from 07:00 - 09:00 WIB, with the highest number of tweets at 08:00 WIB, which was 348 tweets. On the other hand, tweets with the keyword "Prabowo Subianto" tend to be more constant throughout the day during the period of 28th - 29th of May 2019, with an average of 36 tweets per hour.
<p align="center">
  <img src="{{ site.baseurl }}/assets/images/13-5.png" width="350" />
  <img src="{{ site.baseurl }}/assets/images/13-6.png" width="350" />
  <div class="caption" style="align: left; text-align:center;">Figure 3</div>
</p>

Based on Figure 3, the barplot illustrates the number of tweets with the keywords "Joko Widodo" and "Prabowo Subianto" on 28th - 29th of May 2019. As depicted in Figure 3 (left), the average number of tweets per hour uploaded by users discussing Prabowo Subianto is lower than the total number of tweets in two days, with only 30 tweets per hour. Twitter users are less active in discussing Prabowo Subianto from 19:00 - 23:59 WIB, which can be attributed to the Indonesian breaktime. However, the tweets related to this topic are continuously updated at midnight due to the activity of users living abroad or still being active. The users' activity starts to increase at 04:00 WIB, peaks at 07:00 WIB, then drops until it rises again at 12:00 WIB.
<p align="center">
  <img src="{{ site.baseurl }}/assets/images/13-7.png" width="350" />
  <img src="{{ site.baseurl }}/assets/images/13-8.png" width="350" />
  <div class="caption" style="align: left; text-align:center;">Figure 4</div>
</p>

In Figure 4, we present the density plot of sentiment score for tweets containing the keywords "Joko Widodo" and "Prabowo Subianto". The sentiment score, which ranges from -10 to 10, is computed as the average score of the root words in each tweet. The negative score indicates negative sentiment in the tweet, while the positive score indicates positive sentiment.

Based on the density plot on Figure 4 (left), we observe that tweets containing the keyword "Joko Widodo" have a negative sentiment score ranging from -10 to -1, with a median score of -4. The positive sentiment score also ranges from 1 to 10. We conclude that the positive sentiment for tweets containing the keyword "Joko Widodo" is not highly diverse, as indicated by the small variance in the density plot.

On the other hand, Figure 4 (right) displays the density plot of sentiment score for tweets containing the keyword "Prabowo Subianto". The negative sentiment score ranges from -8 to -1, indicating that the tweets do not have highly negative sentiment. Moreover, the distribution of negative sentiment scores has two peaks in the range of 4 and 1. The positive sentiment score ranges from 1 to 10, with high variance and two peaks in the range of 3 and 10. These findings suggest that tweets containing the keyword "Prabowo Subianto" have a high level of positive sentiment.
<p align="center">
  <img src="{{ site.baseurl }}/assets/images/13-9.png" width="350" />
  <img src="{{ site.baseurl }}/assets/images/13-10.png" width="350" />
  <div class="caption" style="align: left; text-align:center;">Figure 5</div>
</p>

Based on Figure 5, it can be observed that tweets with the keyword "Joko Widodo" exhibit a lower percentage of negative sentiment than tweets with the keyword "Prabowo Subianto". The difference in the percentage of negative sentiment between the two categories is 6.3%. Additionally, tweets containing the keyword "Joko Widodo" have a higher percentage of neutral sentiment and positive sentiment as compared to tweets with the keyword "Prabowo Subianto". A pie chart analysis further revealed that tweets with the keyword "Joko Widodo" have a greater proportion of positive sentiment than tweets with the keyword "Prabowo Subianto". However, based on the density plot, it is observed that the distribution of positive and negative sentiment scores indicates that tweets containing the keyword "Prabowo Subianto" tend to have a higher sentiment score compared to those containing the keyword "Joko Widodo". Further analysis is required to investigate this observation.
<p align="center">
  <img src="{{ site.baseurl }}/assets/images/13-11.png" width="350" />
  <img src="{{ site.baseurl }}/assets/images/13-12.png" width="350" />
  <div class="caption" style="align: left; text-align:center;">Figure 6</div>
</p>

Figure 6 presents a WordCloud visualization of frequently used terms in tweets containing the keywords "Joko Widodo" and "Prabowo Subianto" on 28th - 29th May 2019. The visualization provides insights into the popular topics of discussion related to the respective keywords. For tweets containing the keyword "Joko Widodo", the top five frequently used terms include "tuang", "petisi", "negara", "aman", and "nusantara". On the other hand, for tweets containing the keyword "Prabowo Subianto", the top five frequently used terms include "Prabowo", "Subianto", "kriminalisasi", "selamat", and "dubai". It is worth noting that the pattern of tweets containing the keyword "Prabowo Subianto" reveals that the tweets often mention the name "Prabowo Subianto" directly, without the use of a mention (@) due to the removal of mentions during the text pre-processing.

### Sources
<a target="_blank" href="https://haikawanku.wordpress.com/2019/11/08/analisis-sentimen-pasca-pemilihan-umum-indonesia-tahun-2019-melalui-twitter-dengan-metode-naive-bayes/" class="btn btn-danger">Personal Blog</a> <a target="_blank" href="https://github.com/audhiaprilliant/Indonesia-Public-Election-Twitter-Sentiment-Analysis" class="btn btn-warning">Personal GitHub</a>
