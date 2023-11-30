---
layout: page
title: How Factors Influencing Crowd Engagement with News Differ Around the Globe
description: CSCD25 Final Project
img: assets/img/project_img/missing.jpg
importance: 7
category:
---

## Abstract
Social media has become a medium to facilitate crowd engagement with the news. Large fractions of the public now get their morning headlines by simply reading stories reposted by others on their social media feed. Thus, it is important to understand the mechanism by which news is popularized on social media in hopes that these platforms can adjust their algorithms and policies to limit the spread of misinformation. To gain insight into how news is popularized, we will investigate the impacts of various factors on crowd engagement with Reddit posts that share news articles. We will focus on factors such as changing the header when sharing an article, the sentiment of the shared article and the time of day at which the post was made. We will analyze how these relations differ between news subreddits from around the world to better understand if culture plays a role in news engagement.

## Hypothesis
**Changing the title of an article when sharing it on Reddit, the sentiment of the news article shared on Reddit and the time of day at which a news article is shared on Reddit are correlated with increased activity on the post.**

## Research Questions
1. When posting a link to a news article on Reddit, does making the title of the post different from the original article title impact the activity on the post? If there is an influence, how does this influence differ from region-to-region? 
2. Is the sentiment of a news article shared on Reddit correlated with the activity on the post in subreddits around the world? If so, how does this relationship differ across subreddits?
3. Is the time of day at which a news article was shared on Reddit correlated with the activity on the posts?  

## Data Used for Analysis
We use the [Python Reddit API Wrapper (PRAW)](https://praw.readthedocs.io/en/stable/index.html) which will scrapes posts from any subreddit. This API can fetch up to 1000 of the top posts from a given subreddit.

By passing the URLs through the `extract` function implemented in the [Goose3 library](https://goose3.readthedocs.io/en/latest/), we extract necessary information about the article linked (including the article title and content). Any posts whose linked article is behind a paywall cannot be accessed by Goose3 and has thus been excluded from our dataset. Hence, the loaded dataset for each subreddit studied will has at most 1000 rows and have 8 columns (`title`, `subreddit`, `score`, `id`, `url`, `comms_num`, `created` and `body`).

In addition to the features from PRAW, we constructed a few more columns to supplement our analysis, generating the complete feature set below:

- `title`: the title of the post
- `subreddit`: the subreddit the post belongs to
- `score`: the number of upvotes minus the number of downvotes received by the post 
- `id`: a unique identifier for the post 
- `url`: the URL linked in the post
- `comms_num`: the number of comments on the post
- `created`: the date/time at which the post was made (in UTC time zone, represented in Unix time) 
- `body`: the body of the Reddit post
- `activity`: the total activity of each article, defined by `score` + `comms_num`
- `article_title`: the original title of each article obtained using the `extract` function in the [Python Goose3 library](https://goose3.readthedocs.io/en/latest/).
- `headline_changed`: a boolean variable which is True if the `title` is the same as the `article_title`, and False otherwise
- `title_cos_similarity`: the cosine distance between vectorized embeddings of `title` and `article_title` (computed using the [python-string-similarity library](https://github.com/luozhouyang/python-string-similarity)) 
- `sentiment`: the sentiment score assigned to each article using the [Python-SentiStrength](https://github.com/hykilpikonna/PySenti) library

We use PRAW to scrape the [r/canadanews](https://reddit.com/r/canadanews), [r/usanews](https://reddit.com/r/usanews), [r/EUnews](https://reddit.com/r/EUnews) and [r/ausnews](https://reddit.com/r/ausnews) subreddits.

Below are a sample of rows from the dataset generated from r/canadanews:

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>subreddit</th>
      <th>score</th>
      <th>id</th>
      <th>url</th>
      <th>comms_num</th>
      <th>created</th>
      <th>body</th>
      <th>activity</th>
      <th>article_title</th>
      <th>headline_changed</th>
      <th>title_cos_similarity</th>
      <th>sentiment</th>
      <th>utc_hour_created</th>
      <th>local_hour_created</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>King Charles to be featured on new coins circulated by Royal Canadian Mint</td>
      <td>canadanews</td>
      <td>0</td>
      <td>17uzow8</td>
      <td>https://www.cbc.ca/news/canada/manitoba/king-charles-mint-coin-design-circulation-1.7027298</td>
      <td>1</td>
      <td>1699958959</td>
      <td>NaN</td>
      <td>1</td>
      <td>King Charles to be featured on new coins circulated by Royal Canadian Mint | News</td>
      <td>True</td>
      <td>0.033532</td>
      <td>0</td>
      <td>5</td>
      <td>23</td>
    </tr>
    <tr>
      <th>1</th>
      <td>'Vile antisemitic attack:' Police investigating graffiti targeting Indigo CEO outside downtown store</td>
      <td>canadanews</td>
      <td>6</td>
      <td>17su8nc</td>
      <td>https://www.cp24.com/news/vile-antisemitic-attack-police-investigating-graffiti-targeting-indigo-ceo-outside-downtown-store-1.6639708</td>
      <td>12</td>
      <td>1699708826</td>
      <td>NaN</td>
      <td>18</td>
      <td>'Vile antisemitic attack:' Police investigating graffiti targeting Indigo CEO outside downtown store</td>
      <td>False</td>
      <td>0.000000</td>
      <td>-2</td>
      <td>8</td>
      <td>2</td>
    </tr>
  </tbody>
</table>

<br />

## Addressing Research Questions
As the size of the four subreddits study differ greatly (e.g., r/usanews is a much larger community than r/ausnews), we could not use raw activity values to carry out our analysis. Instead, we defined two groups of posts for each subreddit dataset:
- **high**: posts with activity values at or above the 99th percentile
- **low**: posts with activity values at or below the 50th percentile

### 1) Analyzing Impact of Changing News Article Headline on Crowd Engagment

<!-- A [study by Benjamin D. Horne and Sibel Adali in 2017](https://arxiv.org/abs/1703.10570) was the first to  -->

### 2) Analyzing Impact of Article Sentiment on Crowd Engagement

In the social sciences, a well-studied phenomenon is that in general, more negative news stories end up becoming more popular than articles with neutral or positive sentiment ([Harcup and O'Neill 2001](https://www.tandfonline.com/doi/abs/10.1080/14616700118449)). However, these studies primarily focus on the spread of news through newspapers, rather than social media platforms. Although both are conduits for spreading the news, the nature of news engagement may differ online. We attempted to test this hypothesis by first comparing the mean sentiments of the articles linked in posts with high activity to those with low activity using T-tests (results below).

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Subreddit</th>
      <th>T-test Statistic</th>
      <th>p-value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>canadanews</td>
      <td>-1.203407</td>
      <td>0.230155</td>
    </tr>
    <tr>
      <th>1</th>
      <td>usanews</td>
      <td>-1.203407</td>
      <td>0.566143</td>
    </tr>
    <tr>
      <th>2</th>
      <td>EUnews</td>
      <td>-2.230939</td>
      <td>0.026349</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ausnews</td>
      <td>-2.230939</td>
      <td>0.026349</td>
    </tr>
  </tbody>
</table>

We observed that there was no statistically significant in the mean sentiment between the two groups for any of the subreddits analyzed. To better understand the data, let us plot violinplots to compare the distribution of article sentiment between posts with high engagement to posts with low engagement.

{% include figure.html path="assets/img/news_eng/sentiment_violin.png" title="title_cos_canada" class="center" width="600"%}

Across all subreddits, we see that high activity posts seen to unanimously link to negative (or neutral) articles. Meanwhile, 

### 3) Analyzing Impact of Time of Post on Crowd Engagement
In each dataset constructed, there is a column named `created` which holds the date/time stamp (in the UTC timezone, represented in Unix time) at which each post was made. I will use the `fromtimestamp` function implemented in Python's `datetime` library to convert the Unix time to a readable datetime format. I will then convert these UTC times to the local time zones of each subreddit analyzed. For Canada and the US (where there are multiple time zones), I will convert to the mean time zone which is Central Standard Time (CST). To visualize the data, I will construct a set of scatter plots where the x-axis will be the time and the y-axis will represent the engagement on the posts.