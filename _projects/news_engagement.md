---
layout: page
title: How Factors Influencing Crowd Engagement with News Differ Around the Globe
description: CSCD25 (Advanced Data Analytics) Final Project
img: assets/img/news_eng/title_cos_violin.png
importance: 7
category:
---

## Abstract
Social media has become a medium to facilitate crowd engagement with the news. Large fractions of the public now get their morning headlines by simply reading stories reposted by others on their social media feed (using platforms like Reddit). It is important to understand the mechanism by which news is popularized on social media in hopes that these platforms can adjust their algorithms and policies to limit the spread of misinformation. To gain insight into how news is popularized on Reddit, we investigated whether various factors (such as changing the header when sharing an article, negative sentiment in the shared article and the time of day at which the post was made) are correlated with increased engagement with the posts that share news articles. We analyzed how these trends differ between news subreddits from around the world to better understand if local culture may play a role in news engagement.

## Hypothesis
**Changing the title** of an article when sharing it on Reddit, **negative sentiment** in the news article being shared and the **time of day** at which a news article is posted on Reddit are all correlated with increased activity on the post.

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
- `title_cos_similarity`: the cosine distance between embeddings of `title` and `article_title` (titles were vectorized using SentenceTransformer's `all-MiniLM-L6-v2` pre-trained model)
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

Using this distinction, we sought to answer our three research questions.

### 1) Analyzing the Impact of Changing News Article Headline on Crowd Engagment

A [study by Benjamin D. Horne and Sibel Adali in 2017](https://arxiv.org/abs/1703.10570) conducted on r/worldnews subreddit posts in 2012 and 2013 discovered that posts with high activity tend to have a lower similarity between post and article titles (i.e., cosine similarity is closer to 0 in posts with higher activity). In other words, their results suggested that changing the headline when posting an article on Reddit could positively influence the activity on the post. To gauge whether the same holds true in 2023 on the regional news subreddits, we first ran a T-test which compared the mean title cosine similarity in high activity posts to that in low activity posts (results below).

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
      <td>-0.589091</td>
      <td>0.556424</td>
    </tr>
    <tr>
      <th>1</th>
      <td>usanews</td>
      <td>-0.589091</td>
      <td>0.363316</td>
    </tr>
    <tr>
      <th>2</th>
      <td>EUnews</td>
      <td>-0.008271</td>
      <td>0.993405</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ausnews</td>
      <td>-0.008271</td>
      <td>0.993405</td>
    </tr>
  </tbody>
</table>

<br />

Based on the p-values above, none of the subreddits have a significant difference in mean title similarity between high and low activity posts. Thus, we cannot make any conclusions regarding the difference in title similarity between the two groups. To further study the distribution of in title similarity, we constructed violin plots (as shown below).

{% include figure.html path="assets/img/news_eng/title_cos_violin.png" title="title_cos_violin" class="center" width="600"%}

We observe that across all the subreddits, the median title cosine similarity of the high activity posts are approximately equal or higher than that of lower activity posts. This contradicts Horne and Adali's findings. This may be due to changes in the way users consume the news over the past decade (since their study was conducted on data in 2012-2013).Additionally, their study was conducted only on the r/worldnews subreddit. The discrepancy in results between r/worldnews and could also be due to some psychological underpinning in the way we consume local news compared to the way we consume global news. To determine whether these explanations are valid, we constructed the same plots on recent r/worldnews posts (shown below).

{% include figure.html path="assets/img/news_eng/title_cos_violin_worldnews.png" title="title_cos_violin_worldnews" class="center" width="600"%}

With the updated r/worldnews dataset, we saw a similar pattern as with the regional subreddits. This means that there is likely no difference in the way that local and global news are interpreted. It is more likely that the way users consume news in general has changed since the study. Observational studies will be required to validate this potential reason.

<br />

### 2) Analyzing the Impact of Article Sentiment on Crowd Engagement

In the social sciences, a well-studied phenomenon is that in general, popular stories tend to be more negative in sentiment than less popular stories ([Harcup and O'Neill 2001](https://www.tandfonline.com/doi/abs/10.1080/14616700118449)). However, these studies primarily focus on the spread of news through newspapers, rather than social media platforms. Although both are conduits for spreading the news, the nature of news engagement may differ online. We attempted to test this hypothesis by first comparing the mean sentiments of the articles linked in posts with high activity to those with low activity using T-tests (results below).

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

<br />

We observed that there was a statistically significant difference (p < 0.05) in the mean sentiment between the two groups within r/EUnews and r/ausnews. On these two subreddits, the higher activity posts link to substantially more negative articles than lower activity posts. This supports Harcup and O'Neill's findings. 

To better understand the data (especially in r/canadanews and r/usanews), we constructed violin plots and compared the distribution of article sentiment between posts with high engagement to posts with low engagement.

{% include figure.html path="assets/img/news_eng/sentiment_violin.png" title="sentiment_violin" class="center" width="600"%}

Across all subreddits, we see that high activity posts seen to unanimously link to negative (or neutral) articles. Meanwhile, low activity posts link to articles of a broad range of sentiments (both positive and negative). Given the differences in ranges, we decided to check if there is a statistically significant difference in the variances between the groups using the Bartlett test (results below).

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Subreddit</th>
      <th>Bartlett Test Statistic</th>
      <th>p-value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>canadanews</td>
      <td>0.034849</td>
      <td>0.851912</td>
    </tr>
    <tr>
      <th>1</th>
      <td>usanews</td>
      <td>0.034849</td>
      <td>0.186271</td>
    </tr>
    <tr>
      <th>2</th>
      <td>EUnews</td>
      <td>0.590379</td>
      <td>0.442273</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ausnews</td>
      <td>0.590379</td>
      <td>0.442273</td>
    </tr>
  </tbody>
</table>

<br />

Based on the p-values above, there is no significant difference between the "high" and "low" groups in their variances either. Thus, we cannot make any conclusions about the differences in the distribution of sentiments between high and low activity posts on r/canadanews and r/usnews.

Based on the samples studied, we can comment that posts with high activity tend to have strictly negative or neutral sentiments and only in r/EUnews and r/ausnews is there a significant difference in article sentiments among high and low activity posts.

### 3) Analyzing the Impact of Posting Time on Crowd Engagement
In each dataset constructed, there is a column named `created` which holds the date/time stamp (in the UTC time zone, represented in Unix time) at which each post was made. To make these values interpretable, we used the `fromtimestamp` function implemented in Python's `datetime` library which converted the Unix time to a readable date/time format (also in the UTC time zone).

As the regions being studied are all around the globe, the local time zones may introduce bias into the results if solely looking at the UTC time. To address this potential issue, we converted the UTC times to the local time in the region the posts are made. Here, we made the assumption that all users on the subreddits live in the same region as its focus (e.g., all users posting and interacting on r/canadanews are based out of Canada). This assumption may not be entirely true all the time; however, given the constraints of the data at hand, this was the best we were able to do. All of the regions being studied have multiple time zones. So, we converted to the mean time zone in the region for all subreddits.

We used the following local time zones for our analysis (as indicated in the map below):
- Canada: `Canada/Central`
- USA: `US/Central`
- EU: `Europe/Berlin`
- Australia: `Australia/Darwin`

{% include figure.html path="assets/img/news_eng/time_zones.png" title="time_zones" class="center" width="600"%}

To visualize the activity throughout the day, we plotted the mean activity on posts posted each hour of the day (both in the UTC and local time zones), as follows:

{% include figure.html path="assets/img/news_eng/time_activity_raw.png" title="time_activity_raw" class="center" width="600"%}

By observing the raw activity values throughout the day, it is difficult to draw any conclusions since the magnitudes between subreddits are so different (e.g., Europe and Australia is much less active than the US, so their plots appear to be around 0). To make the plots more interpretable, we normalized the mean activities for each subreddit (i.e., the highest value for each subreddit was set to 1 and the lowest was set to 0). The normalization produced the following plot:

{% include figure.html path="assets/img/news_eng/time_activity_normalized.png" title="time_activity_normalized" class="center" width="600"%}

The normalized activity values are much easier to interpret. When comparing the plots based on hour of posting in UTC, there does not seem to be a very clear pattern between the regions. This is likely because the local times differ so greatly across the regions studied that the times at which users are typically active on Reddit do not coincide across the regions.

When analyzing the plots based on local time, a loose pattern emerges. Since the patterns since the mean time zone was used for local times, the plotted times may be up to 2-3 hours off from the actual hour of posting. Despite this inaccuracy, we observe that in all the regions, there is increased activity on posts posted around 18:00 (6 PM) and 21:00 (11 PM). This may be because users tend to get off from work/school during these times and have some leisurely time to surf Reddit. In Australia, there are also spikes at around 9 AM and 2 PM local time. As these are recorded in the mean time zone, it is possible that these spikes coincide with morning commutes and lunch breaks. These spikes are not as prominent in the other regions which may be due to differences in working culture. It is important to note that without thorough observational studies, these potential reasons behind the patterns cannot be confirmed.

## Conclusion
Through this study, we made several interesting discoveries. Contrary to our hypothesis (which had been accepted in a paper using data from a decade ago), there is no correlation between changing the title when linking an article on Reddit and activity levels on the post. This finding also underscores the importance of repeating such experiments over time. With the fast changes in the ways information is spread due to technology, the behaviours of users are also shifting. By repeating experiments, we can stay up-to-date on the current state of human behaviour as it relates to engagement with the news. We also discovered that in Europe and Australia, posts with higher activity link to articles with more negative sentiment than articles linked to by lower activity posts (which aligns with our hypothesis). This trend was not observed in r/canadanews and r/usnews, suggesting differences in the factors influencing news popularity among regions. Lastly, our study uncovered patterns which potentially imply that the local time at which posts are made have an influence on the level of activity on the post. 

In future studies, in order to verify if these results generalize to a broader scope, similar methodologies should be applied to more news subreddits as well as other social media platforms on which news is shared.