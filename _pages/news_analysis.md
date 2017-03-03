---
permalink: "/news_categorized"
layout: single
type: pages
author_profile: true
---
# The News of Our Times
<kbd>
<img src ="https://lukearmbruster.github.io/_pages/cover.png" style="width: 1000px">
</kbd>

## Note
<p style='text-align: justify;'>The following article is currently being revised. Please refer to the <a href="https://github.com/lukearmbruster/GA-DSI-4-Capstone_Project/blob/master/DSI_Capstone_Technical_Report.ipynb">technical report</a> or the <a href="https://docs.google.com/presentation/d/1ZGF9nfiR4TT_wyTAbSi9_-NnaK7VUG8auo9ITJDpiJg/edit?usp=sharing">presentation</a> I gave recently in the meantime.</p>

Thanks!

Luke

### Jump to Section
[Introduction](#introduction),
[Project Objective](#project_objective),
[Data Sources](#data_sources),
[Assumptions](#assumptions),
[Importing and Cleaning, and Merging the Data](#importing),
[Exploratory Data Analysis (EDA)](#eda),
[Figure 1](#figure_1),
[Figure 2](#figure_2),
[Figure 3](#figure_3),
[Figure 4](#figure_4),
[Figure 5](#figure_5),
[Figure 6](#figure_6),
[Figure 7](#figure_7),
[Figure 8](#figure_8),
[Figure 9](#figure_9),
[Figure 10](#figure_10),
[Figure 11](#figure_11),
[Figure 12](#figure_12),
[Figure 13](#figure_13),
[EDA Summary](#eda_summary),
[Model Building](#model_building),
[Model Results and Evaluation](#model_evaluation),
[Figure 14](#figure_14),
[Figure 15](#figure_15),
[Figure 16](#figure_16),
[Figure 17](#figure_17),
[Conclusion](#conclusion),
[Future Work](#future_work)

<a name="introduction"></a>
## Introduction
Fake news is a very hot topic these days after the 2016 U.S. Presidential election. Facebook and Google are responding with their own actions to reduce the spread of fake news, including down-ranking in search results and pulling advertisements funds[[1](http://www.reuters.com/article/us-alphabet-advertising-idUSKBN1392MM),[2](https://techcrunch.com/2016/12/15/facebook-now-flags-and-down-ranks-fake-news-with-help-from-outside-fact-checkers/)]. The technological challenge of automatically tagging fake news without thoroughly cross-checking statements-a task only a human can accomplish well at this time, is a serious obstacle to preventing the spread of fake news [[3](https://qz.com/843110/can-artificial-intelligence-solve-facebooks-fake-news-problem/)]. Excited by the opportunity to address this unique and challenging technological challenge, I found the inspiration to tackle the following project.

<a name="project_objective"></a>
## Project Objective
This project analyzes posts from a period spanning 73 days before and after the U.S. Presidential election (from August 26, 2016, to January 20, 2017) on Facebook news pages via the Facebook Graph API to explore the affect of the election on posting and engagement by type of news as well as to meet the following objectives:

- Identify what factors uniquely identify whether a shared Facebook news posting is from a mainstream, fake, conspiracy, or satire source. I suspect that the differences between fake, conspiracy, and satire sources are subtle, and, therefore, the potential minute differences between them would need to be inspected more closely than merely an inspection between mainstream and fake news. For this reason, a multiclass inspection over a binary inspection (i.e. fake and not fake news) is preferred.

- Leverage differences between types of news to build a multiclass logistic regression model that tags a shared Facebook news post by type using important monograms and bigrams from post messages, patterns in engagement activities, type of post attachment (e.g. image, video, none, etc.) and various factors of time (i.e. day of week, hour of day, and timing of election [i.e. before, same day, or after]). A regression model is appropriate for the purposes of this project to identify the most important model features via model coefficients. The value of an appropriately calibrated model could help Facebook identify what posts to flag for taking actions to disincentivize the dissemination of fake news or for use by computational journalists to track the issues related to fake news to meet future reporting responsibilities.

<a name="data_sources"></a>
## Data Sources
This project generates the project dataset by taking the following steps:
1. A list of mainstream news sites is generated using a [Pew Research study published on October 21, 2014, Political Polarization & Media Habits: From Fox News to Facebook, How Liberals and Conservatives Keep Up with Politics](http://www.journalism.org/2014/10/21/political-polarization-media-habits/). Comprising the list are well-known sources more trusted than not by the general public as identified in the study.
2. Two sources are used to generate a list of fake, conspiracy, and satire news sites. A [list of fake, conspiracy, and satire news sources compiled by Dr. Melissa Zimdars of Merimack College](https://github.com/BigMcLargeHuge/opensources) is cross-checked against against a list of news sources by the corresponding target types and unknown types from a [fake news Kaggle dataset](https://www.kaggle.com/mrisdal/fake-news) compiled in October and November of 2016 using the [BS ("bullshit") Detector Chrome Extension from Daniel Sieradski](https://www.producthunt.com/posts/b-s-detector). All news sources listed as fake, conspiracy, and satire in Dr. Zimdars list are inspected for Facebook pages unless the news label explicitly conflicts with the news label of the sources in the Kaggle dataset. To focus on U.S. media, various known foreign pages are excluded.
3. Compile all news postings from the Facebook pages of mainstream, fake, conspiracy, and satire sources via the [Facebook Graph API](https://developers.facebook.com/docs/graph-api)

<a name="assumptions"></a>
## Assumptions
- The list compiled by Melissa Zimdars have Facebook posts consistent with the category assigned when examined as a whole. A mainstream news post contains accurate information with the intent of informing the reader about a current event. A fake news post contains inaccurate information with the intent of misleading the reader. Conspiracy does not contain information that can be verified with an unknown or unapparent intent. Satire may contains inaccurate information and/or perhaps unverified information with the intent of humoring the reader.
- Proxies for thoroughly cross-checking and verifying the validity of statements/accounts, i.e. good journalism, and inferring intent can predict the type of news.
- A model built on a skewed sample can be used to accurately categorize the news in the real world when the ratios of exposure to the type of news is changing and dependent on previous engagement with information on Facebook.
- Each Facebook news post can be adequately, appropriately assigned to a unique category.
- The logit of the probabilities of the types of news are linear with respect to the model parameters.

<a name="importing"></a>
## Importing, Cleaning, and Merging the Data
The following steps are taken to download and clean the data before performing exploratory data analysis (EDA):
1. Post information is compiled to a local drive via Facebook's Graph API by making modifications to a Python code provided by another [programmer](https://drive.google.com/file/d/0Bw1LIIbSl0xuRTNCZElUa3U1b1U/view). Data extracted from Facebook include post message, title of link, type of link, post date, and individual counts of comments, shares, likes, loves, wows, hahas, sads, and angrys.
2. Additional columns were generated from the existing dataset or merged with known existing information to make a more detailed final dataset showing Facebook page id, type of news page (i.e. mainstream, fake, conspiracy, or satire), day of week, hour, timing of election, and count of all user engagement activities (i.e. comments, shares, likes, loves, wows, hahas, sads, and angrys).
3. All text is converted from unicode to string format. (year, month, day?).
4. 99 of the posts are identified as redundant (.0.04% of the total), so these are removed.
5. All urls are removed from post messages to avoid selecting when identifying important monograms and bigrams from post messages.

<a name="eda"></a>
## Exploratory Data Analysis (EDA)
The following section explores characteristics of the dataset to better understand how the predictors perform in the final model.

The final dataset includes over 274 thousand posts and over 900 million engagement activities from 129 Facebook pages. Though the number of mainstream sites are least represented in the dataset, mainstream posts are the second most common type of post.

<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_1.png" style="width: 1000px">
</kbd>
##### Figure 1
<br>
<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_2.png" style="width: 1000px">
</kbd>
##### Figure 2
<br>
Only a handful of pages are posting and receiving the majority of the engagement, because the distribution of posts and level of engagement on pages shows a clear positive skew regardless of the type of news. I would expect a model built on the existing skewed dataset would perform disproportionately better for the sites with the most posts and the highest levels of engagement.

<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_3.png" style="width: 1300px">
</kbd>
##### Figure 3
<br>
Now specific characteristics of model predictors are explored with respect to posts and engagement activities. If differences are observed in the patterns of posts among the different types of news with respect to the various predictors, they may be reflected in the final model as relatively high model coefficients in terms of absolute value. First, the frequency of the different post attachments and engagement level by the type of news is investigated. Upon inspection, all types of news have posts with links more frequently than any other type of attachment, and all but conspiracy also show links receiving the highest percent of engagement compared with all other types of attachment. However, with respect to the second most frequent type of attachment, mainstream pages post videos more frequently than all other types of news. Therefore, I would expect the model to show a relatively high coefficient for mainstream videos relative to the coefficient for videos from all other types of news.

<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_4.png" style="width: 1000px">
</kbd>
##### Figure 4
<br>
Next the frequency of posts and level of engagement are investigated with respect to the U.S. Presidential Election. The most drastic change in post volume occurred in the form of a reduction in fake news after the election, in large part due to a reduction in post volume among the top three most prolific sources. ([Figure 5](#figure_5) and [Figure 6](#figure_6)) Nevertheless, engagement activities associated with fake news increased most drastically second only to the spike in engagement among mainstream news-in part due to an increase in post volume. ([Figure 5](#figure_5)) The spike in engagement to mainstream news occurred largely due to relatively high engagement with posts from CNN, ABC News, and USA Today. ([Figure 7](#figure_7)) Due to the noticeable reduction in the percentage of posts from fake news after the election, the final model may have a relatively high pre-Presidential Election fake news model coefficient compared to the post-Presidential Election fake news model coefficient.

<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_5.png" style="width: 1000px">
</kbd>
##### Figure 5
<br>
<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_6.png" style="width: 500px">
</kbd>
##### Figure 6
<br>
<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_7.png" style="width: 500px">
</kbd>
##### Figure 7
<br>
An inspection of the frequency of posts and level of engagement over the days of the week reveals a noticeable dip in the volume of posts and engagement activities occurs on Saturday and Sunday relative to other days of the week  regardless of the type of news. ([Figure 8](#figure_8)) Due to the low variation of post frequency among different types of news throughout the week, I would expect the model not to show relatively large coefficients with respect to this predictor.

<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_8.png" style="width: 1000px">
</kbd>
##### Figure 8
<br>
With respect to the hours of the day, posts peak during working hours when considering the four time zones spanning the lower 48 U.S. states, regardless of the type of news. Differences in the posting time is relatively small between the different types of news with the exception of satire news, which shows a considerable peak between 6 am and 12 pm (Pacific Standard Time). Interestingly, engagement levels appear relatively uniform over the hours of a day in comparison to post hours, indicating widespread engagement among users around the world and/or late night users. Relative to other types of news, engagement levels among conspiracy sites appear to be most uniform across all times, while engagement levels among satire pages spike at a time when posts peak, i.e. between 6 am and 7 am. ([Figure 9](#figure_9)) With little difference in the pattern of posts over the hours of the day between different types of news with the exception of satire news, model coefficients for hours of the day for mainstream, fake and conspiracy news are expected to be relatively low in comparison to the model coefficient for hours of the day for satire news.

<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_9.png" style="width: 1000px">
</kbd>
##### Figure 9
<br>
The model also includes predictors for each type of engagement activity. To compare differences in the pattern of engagement activities among the different types of news, the value for such a predictor is the proportion that a specific engagement activity occurs out of the total engagement activity in response to a post. An investigation of engagement activities by type of news and among the most highly engaged pages follows.

Like followed by share is the most common engagement action in response to a post. High variability in engagement actions is observed for all types of news relative to the mean value. Interestingly, satire shows the second highest mean engagement activity but has the lowest total engagement among all types of news. Perhaps unsurprisingly, satire has a noticeably higher mean volume of haha compared with the mean volume of haha for other types of news. ([Figure 10](#figure_10)) Furthermore, haha is the second or third most common engagement response among individual satire pages. ([Figure 11](#figure_11)) Therefore, I would expect the final model to show haha model coefficient for satire to be relatively higher than the haha coefficients for all other types of news. Nonetheless, more satire posts covering a longer period of time and including more satire sources are needed to better understand and compare with other types of engagement, because the project dataset contains far fewer satire posts relative to all other types of news. ([Figure 10](#figure_10))

Upon further inspection, other patterns also emerge. Share and comment are the second or third most common engagement actions from satire news pages or highly engaged mainstream news pages, i.e. Fox News. and cnn, but relatively less common as a proportion of other engagement actions among remaining mainstream sources. Among fake and conspiracy news, share again is the second most common engagement type (sometimes most common), and the proportion of share in response to fake and conspiracy posts appears to be greater relative to the proportion of shares among mainstream and satire news. ([Figure 11](#figure_11)) Based on these observations, I would expect the final model to have higher share coefficients for fake and conspiracy pages relative to the share coefficients for mainstream and satire pages.

<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_10.png" style="width: 1000px">
</kbd>
##### Figure 10
<br>
<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_11M.png" style="width: 1000px">
</kbd>
<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_11F.png" style="width: 1000px">
</kbd>
<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_11C.png" style="width: 1000px">
</kbd>
<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_11S.png" style="width: 1000px">
</kbd>
##### Figure 11
<br>
Correlations of the main quantitative predictors are examined to determine the appropriate setting for regularization in the final model. When examining correlation heat maps, no strong correlations appear between engagement activities and/or between time periods (i.e. hour or day of week regardless of the type of news. ([Figure 12](#figure_12)) Therefore, regularization with lasso appears to be a suitable setting.

<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_12.png" style="width: 700px">
</kbd>
##### Figure 12
<br>
The remaining predictors that are not investigated above are important monograms and bigrams selected from post messages for each type of news. These n-grams are selected such that the maximum number of distinct words used in post messages is extracted as well as the most commonly used words that are unique among each type of news. In general, the inclusion of monograms and bigrams in the model risks adding a considerable amount of noise, given that the training set is considerably smaller than the number of posts in the training dataset. Only 2% (mainstream) to 29% (satire) of all posts actually contain message text depending on the type of news. Therefore, the coefficients associated important monograms and bigrams in the final model must be looked at with a higher degree of skepticism than with the other model predictors.

In detail, three steps are followed to generate the list of important monograms and bigrams from post messages. First, four lists of the most frequent 6,000 words for each type of news is generated using a count vectorizer. Next the term frequency inverse document frequency (TF-IDF) is calculated, and the 6,000 words with the highest TF-IDF score are selected if the word is identified in only one of the four lists. The resulting list contained only 793 words with TF-IDF scores. The list was further reduced to 505 words in order to remove meaningless words or ones that were closely associated with an individual page (e.g. the name of the page or a host of a particular news program).

When evaluating correlations for each type of news, mainstream news has relatively high correlations between haha and comment (positive correlation [+]), angry and comment (+), wow and share (+), love and like (+), like and comment (negative correlation [-]), like and share (-), wow and like (-), haha and like (-), sad and like (-), and angry and like (-). Fake news has relatively high correlations between angry and comment (+), and like and share (-). Also, satire news has relatively high correlations like and share (-). ([Figure 13](#figure_13))

<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_13.png" style="width: 1000px">
</kbd>
##### Figure 13
<br>
<a name="eda_summary"></a>
## EDA Summary
- Only a handful of pages are posting and receiving the majority of the engagement regardless of the type of news. ([Figure 3](#figure_3))
- With respect to the second most frequent type of attachment, mainstream pages post videos more frequently than all other types of news.  ([Figure 4](#figure_4))
- The most drastic change in posting volume occurred in the form of a reduction in fake news after the election, in large part due to a reduction in post volume among the top three most prolific sources. ([Figure 5](#figure_5) and [Figure 6](#figure_6))
- Regardless of the type of news, a noticeable dip in volume of posts and engagement reactions occurs on Saturday and Sunday relative to other days of the week. ([Figure 8](#figure_8))
- With respect to the hours of the day, posts peak during working hours when considering the four time zones spanning the lower 48 U.S. states, regardless of the type of news. ([Figure 9](#figure_9))
- Perhaps unsurprisingly, satire has a noticeably higher mean volume of haha compared with the mean volume of haha for other types of news. ([Figure 10](#figure_10))
- The proportion of share in response to fake and conspiracy posts appears to be greater relative to the proportion of shares among mainstream and satire news. ([Figure 11](#figure_11))
- When examining correlation heat maps, no strong correlations appear between engagement activities and/or between time periods (i.e. hour or day of week regardless of the type of news. ([Figure 12](#figure_12))

<a name="model_building"></a>
## Model Building
A logistic regression model is appropriate in this project in order to categorize the news by type using categorical and numeric predictors. Once the model is built, model coefficients indicate relative importance of predictors that can provide insight into distinct characteristics of posts for each type of news.

The predictors of the final model include important monograms and bigrams from post messages, patterns in engagement activities, type of post attachment (e.g. image, video, none, etc.) and various factors of time (i.e. day of week, hour of day, and timing of election [i.e. before, same day, or after]). All default settings for the logistic regression model in Scikit-Learn are maintained except for the specification of lasso regularization as justified above and an increase in the maximum number of iterations allowed.

<a name="model_evaluation"></a>
## Model Results and Evaluation
A 57% mean accuracy is achieved on the test dataset with a test-train split of 70%-30%, which translates to an increase of 16% above baseline. The areas under the receiver operating characteristic (ROC) curves range from 74% (fake and conspiracy) to 86% (satire) indicating a well-performing model on the test dataset. ([Figure 14](#figure_14)) Therefore, a quantifiable degree of success is achieved when relying on the source list compiled by Dr. Melissa Zimdars to predict the type of news of a Facebook post from specific post features.

<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_14.png" style="width: 800px">
</kbd>
##### Figure 14
<br>
Nonetheless the model has apparent weaknesses. Precision and recall ranges between 54% (fake and satire) and 70% (conspiracy) and between 8% (conspiracy) and 77% (fake), respectively. Low recall values result from the model predicting the majority of the conspiracy and satire posts as mainstream and fake. ([Figure 15](#figure_15)) On a source level, this is confirmed, as nearly all high and low volume conspiracy and satire pages incorrectly predict several times the count of correct predictions. Fprnradio (conspiracy) and NewsThump (satire) are an exception to this case, with incorrect and correct counts relatively close. Elmundotoday (satire) and theunrealpage (satire) are also exceptions with the correct prediction count at least double the incorrect count. ([Figure 16](#figure_16))

<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_15.png" style="width: 600px">
</kbd>
##### Figure 15
<br>
<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_16M.png" style="width: 1000px">
</kbd>
<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_16F.png" style="width: 1000px">
</kbd>
<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_16C.png" style="width: 1000px">
</kbd>
<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_16S.png" style="width: 1000px">
</kbd>
##### Figure 16
<br>
Although all types of news have only a few sources that post a large portion of the total volume and receive much of the engagement ([Figure 3](#figure_3)), the model is not overfitting to the characteristics of the most prolific or engaged pages. Using the test dataset, a review of the classified and misclassified posts by page confirms this observation. Both high and low volume fake and mainstream news pages are classified as their true class more often than misclassified, i.e. for 70% and 80% posts, respectively. Also, nearly all the posts for high and low volume conspiracy and satire sources are misclassified several times more than than the correctly classified posts. ([Figure 16](#figure_16))

Overall, the final model shows the highest model coefficients for engagement actions and n-grams. The highest model coefficients for mainstream news are associated with the following predictors: likes (positive [+]), comments (+), sads (+), neverhillary (negative [-]), angrys (+). The highest model coefficients for fake news among predictors include the following: follow american (+), follow deplorable (+), stop cheering (+), share expose (+), and sads (-). Also, the highest model coefficients for conspiracy news are associated with the following predictors: comments (-), follow american (-), tour ticket (+), follow deplorable (-), hahas (-). Finally, the highest model coefficients for satire news include: likes (+), loves (-), hahas (+), angrys (-), neverhillary (-). ([Figure 17](#figure_17))

<kbd>
<img src ="https://lukearmbruster.github.io/_pages/Figure_17.png" style="width: 1000px">
</kbd>
##### Figure 17
<br>
Values of model coefficients are evaluated against observations noted in the above exploratory data analysis to further assess the performance of the model. These evaluations are summarized in the list below:
- As discussed above, mainstream pages post videos more frequently than all other types of news. ([Figure 4](#figure_4)) Upon inspection of the model coefficients, this observation is reflected in a relatively high model coefficient for mainstream videos compared with video coefficients for other types of news. ([Figure 17](#figure_17))

- A noticeable reduction in the percentage of posts from fake news is observed after the election. ([Figure 5](#figure_5)) This observation is reflected in a relatively high pre-Presidential Election coefficient for fake news compared with the post-Presidential Election coefficient for fake news.

- As expected, due to the low variation of post frequency among different types of news throughout the week ([Figure 8](#figure_8)), model coefficients for days of the week vary very little (i.e. between -0.08 to 0.08) among the different types of news.

- Also as expected, with little difference in the pattern of posts over the hours of the day between different types of news with the exception of satire news ([Figure 9](#figure_9)), model coefficients for hours of the day for mainstream, fake and conspiracy news are relatively low in comparison to the model coefficient for hours of the day for satire news.

- Furthermore the model confirms perhaps the least surprising observation. i.e. satire has a noticeably higher mean volume of haha compared with the mean volume of haha for other types of news ([Figure 10](#figure_10)). This observation is reflected in the high haha model coefficient for satire relative to the haha coefficients for all other types of news. ([Figure 17](#figure_17)),

- For reasons to be determined in future work, share coefficients for fake and conspiracy news are actually lower than share coefficients for mainstream and satire news. This result is contrary to expectations from observing a greater proportion of shares in response to highly engaged fake and conspiracy posts relative to the proportion of shares in response to highly engaged mainstream and satire posts. ([Figure 11](#figure_11))

<a name="conclusion"></a>
## Conclusion
- A quantifiable degree of success is achieved when relying on the source list compiled by Dr. Melissa Zimdars to predict the type of news of a Facebook post from specific post features. A 57% mean accuracy is achieved on the test dataset with a test-train split of 70%-30%, which translates to an increase of 16% above baseline, and the areas under the ROC curves indicate a well-performing model on the test dataset. ([Figure 14](#figure_14))

- Low recall values on the test dataset are produced by the model systemically predicting the majority of the conspiracy and satire posts as mainstream and fake posts ([Figure 15](#figure_15) and [Figure 16](#figure_16));

- Although all types of news only have a few sources that post a large portion of the total volume and receive much of the engagement ([Figure 3](#figure_3)), the model is not overfitting to the characteristics of the most prolific or engaged pages as confirmed in a review of the classified and misclassified posts by page on the test dataset. ([Figure 16](#figure_16))

- Overall, the final model shows the highest model coefficients for engagement actions and n-grams. ([Figure 17](#figure_17))

- The values of model coefficients confirm many of the observations described in the EDA, including the more frequent posting of videos by mainstream news ([Figure 4](#figure_4)), changes in the posting of pre- and post_presidential Election fake news ([Figure 5](#figure_5)), the low variation in posting among different types of news throughout the week ([Figure 8](#figure_8)), and the unique posting pattern over the hours of the day and the high mean volume of haha for satire news compared to other types of news. ([Figure 9](#figure_9) and [Figure 10](#figure_10))

<a name="future_work"></a>
## Future Work
- Calibrate and verify the model using more posts, messages, and news sources with a dataset more evenly sampled among types of news than the current project dataset. Do proxies stand? More satire posts over a longer period of time are needed to understand broad patterns among message text and engagement activities, because the existing project dataset contains fewer satire posts relative to all other types of news. ([Figure 10](#figure_10))
- Vary number and types of words from message text used in the model. Possibly include n-grams of link names.
- Examine the content of the pre-categorized posts in the dataset and, if necessary, correct the label.
- Evaluate other feature engineering options to compare against the  performance of Lasso regularization.
- Use GridSearch to vary the C parameter.
- To resolve the discrepancy between the results from the EDA and the examination of model coefficients as discussed above, explain why share coefficients for fake and conspiracy news are lower than share coefficients for mainstream and satire news.
