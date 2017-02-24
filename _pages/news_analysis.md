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

<a name="introduction"></a>
## Introduction
Fake news is a very hot topic these days after the 2016 U.S. Presidential election. Facebook and Google are responding with their own actions to reduce the spread of fake news, including down-ranking in search results and pulling advertisements funds[[1](http://www.reuters.com/article/us-alphabet-advertising-idUSKBN1392MM),[2](https://techcrunch.com/2016/12/15/facebook-now-flags-and-down-ranks-fake-news-with-help-from-outside-fact-checkers/)]. The technological challenge of automatically tagging fake news without thoroughly cross-checking statements-a task only a human can accomplish well at this time, is a serious obstacle to preventing the spread of fake news [[3](https://qz.com/843110/can-artificial-intelligence-solve-facebooks-fake-news-problem/)]. Excited by the opportunity to address this unique and challenging technological challenge, I found the inspiration to tackle the following project.  

<a name="project_objective"></a>
## Project Objective
This project analyzes posts from a period of 73 days before and after the U.S. Presidential election (from August 26, 2016, to January 20, 2017) on Facebook news pages via the Facebook Graph API. The purpose of using
this information is to meet the following objectives:

- Identify what factors uniquely identify whether a shared Facebook news posting is from a mainstream, fake, conspiracy, or satire source. I suspect that the differences between fake, conspiracy, and satire sources are subtle, and, therefore, the potential minute differences betwen them would need to be inspected more closely than between mainstream and fake news. For these reasons, a multiclass inspection over a binary inspection (i.e. fake and not fake news) seems more approrpiate.

- Leverage differences between news site to build a multiclass logistic regression model that tags a shared Facebook news post by type using post status text and user engagement. A logistic regression model is appropriate in this project in order to evaluate the affects of predictors on the target probabilities. The value of an appropriately calibrated model could help Facebook identify what posts to flag for taking actions to disincentivize the dissemination of fake news or for use by computational journalists to track the issues related to fake news to meet future reporting responsbilities.

<a name="#data_sources"></a>
## Data Sources   
This project uses the following data sources:
- A list of mainstream news sites, defined as well-known sources more trusted than not by the general public, is generated using a [Pew Research study published on October 21, 2014, Political Polarization & Media Habits: From Fox News to Facebook, How Liberals and Conservatives Keep Up with Politics](http://www.journalism.org/2014/10/21/political-polarization-media-habits/).
- Two sources are used to generate a list of fake, conspiracy, and satire news sites. A [list of fake, conspiracy, and satire news sources compiled by Dr. Melissa Zimdars of Merimack College](https://github.com/BigMcLargeHuge/opensources) is checked against against a list of news sources by the corresponding target types and unknown types from a [fake news Kaggle dataset](https://www.kaggle.com/mrisdal/fake-news) compiled in October and November of 2016 using the [BS ("bullshit") Detector Chrome Extention from Daniel Sieradski](https://www.producthunt.com/posts/b-s-detector). All news sources listed as fake, conspiracy, and satire in Dr. Zimdars list are inspected for Facebook pages in the project unless their type designation conflicted with the type designation for the sources in the Kaggle dataset. Also some known foreign pages are exluded to focus mostly on U.S. media.
- Compiled all news postings from the Facebook pages of mainstream, fake, conspiracy, and satire sources via the [Facebook Graph API](https://developers.facebook.com/docs/graph-api)

<a name="assumptions"></a>
## Assumptions
- The list compiled by Melissa Zimdars have Facebook posts consistent with the category assigned. For example, I assume a fake news post does not contain accurate information as a whole.
- Proxies for thorough cross-checking and verifying the validity of statements/accounts, i.e. good journalism, can consistently and accurately predict type of news.
- A model built on a skewed sample can be used to accurately categorize news in the real world when the ratios of exposure to the type of news is changing and dependent on user preferences.
- The logit of the probabilities of the types of news are linear with respect to the model parameters.

<a name="eda_summary"></a>
### EDA Summary
- Although the number of diffierent mainstream news sources analyzed in the project is lowest among all other types of news, mainstream still makes up the second highest post volume. ([Figure 1](#figure_1) and [Figure 2](#figure_2))
- A few mainstream, fake, conspiracy, and satire news (last type not shown in left panel of Figure 3) post much of the total volume and received much of the engagement within their respective type of news. ([Figure 3](#figure_3))
- Links are the most popular attachment in posts among all types of news. Links also receive the highest engagement level among types of attachments in posts, except for conspiracy, for which photos receive the highest engagement level although more links are attached than photos. ([Figure 4](#figure_4))
- The most drastic change in posting volume occured in the form of a reduction in fake news after the election, in large part due to a reduction in post volume among the top three most prolific sources. ([Figure 5](#figure_5) and [Figure 6](#figure_6)) Nevertheless, engagement activities associated with fake news increased most drastically second only to the spike in engagement among mainstream news-in part due to an increase in post volume. ([Figure 5](#figure_5)) The spike in engagement among mainstream news occured largely due to relatively high engagement among posts from CNN, ABC News, and USA Today. ([Figure 7](#figure_7))
- Regardless of the type of news, a noticeable dip in volume of posts and engagement reactions occurs on Saturday and Sunday relative to other days of the week. ([Figure 8](#figure_8))
- Considering the four time zones spanning the lower 48 U.S. states, posts peak regardless of the type of news during working hours. Interestingly, engagement levels appear relatively uniform over the hours of a day in comparison to post hours, indicating widespread engagement among users around the world and/or late night users. Also interesting, engagement levels among conspiracy sites appear to be most uniform across all times, while engagement levels among satire  sites spike at a time when posts peak, i.e. between 6 am and 7 am. ([Figure 9](#figure_9))
- Like followed by share is the most common engagement reaction in response to a post. High variability in engagement reactions is observed for all types of news relative to the mean value. Interestingly, engagement activity among satire sites is second highest relative to engagement activity among mainstream sites. Perhaps unsurprisingly, a noticeably higher mean volume of haha is observed with a relatively low level of variability among all other types of news. ([Figure 10](#figure_10)) Furthermore, haha is the second or third most common engagement response among satire sites. ([Figure 11](#figure_11)) Nonetheless, more satire posts covering a longer period of time and including more satire sources are needed to understand broad patterns among engagement reactions, because the project dataset contains far fewer satire posts relative to all other types of news. ([Figure 10](#figure_10))
- Share and comment are the second and third most common engagement reactions to high volume posts from mainstream news outlets, i.e. Fox News. and cnn, but relatively less common as a proportion of other engagement reactions among remaining mainstream sources. Among fake and conspiracy news, share again is the second most common engagement type (sometimes most common), and the proportion of share in response to fake and conspiracy posts appears to be greater relative to the proportion of shares among mainstream news. ([Figure 11](#figure_11))
- No strong correlations exist among reaction types time periods (i.e. hour or day of week)regardless of the type of news. ([Figure 12](#figure_12)) When evaluating correlations for each type of news, mainstream news has relatively high correlations betwen haha and comment (positive correlation [+]), angry and comment (+), wow and share (+), love and like (+), like and comment (negative correlation [-]), like and share (-), wow and like (-), haha and like (-), sad and like (-), and angry and like (-). Fake news has relatively high correlations betwen angry and comment (+), and like and share (-). Also, satire news has relatively high correlations like and share (-).
([Figure 13](#figure_13))
- Between 2% (mainstream) to 29% (satire) of posts contain text depending on the type of news.

<a name="model_summary"></a>
### Model Summary
- A 57% mean accuracy is achieved on the test dataset with a test-train split of 70%-30%, which translates to an increase of 16% above baseline. Precision and recall ranges are between 53% (satire) and 71% (conspiracy) and between 8% (conspiracy) and 78% (fake), respectively.

- The area under the ROC curves ranges between 74% (fake and conspiracy) and 86% (satire). ([Figure 14](#figure_14))

- Using the test dataset, mainstream and fake news posts are classified as their true class much more often than misclassified, i.e. 0.7 and 0.8, respectively. The model misclassified much more satire as mainstream and fake (80% combined) compared to the true class. Conspiracy is misclassified more as mainstream and fake (90% combined) than the true class (0.1). ([Figure 15](#figure_15)) On a source level, this is confirmed, as nearly all high and low volume conspiracy and satire source incorrectly predict several times the count of correct predictions. Fprnradio (fake) and NewsThump (satire) are an exception to this case, with incorrect and correct counts relatively close. Elmundotoday (satire) is also an exception with over double the correct prediction relative to the incorrect count. ([Figure 16](#figure_16))

- The model generates relatively high mainstream coefficients for the following parameters: like (positive [+]), comment (+), sad (+), neverhillary (negative [-]). Relatively high fake coefficients are generated for the following parameters: follow american (+), follow deplorable (+), stop cheering (+), share expose (+), video type (-), and sad (-). Also, relatively high conspiracy coefficients are generated for the following parameters: comment (-), tour ticket (+). The highest satire coefficients are as follows: like (+), love (-), haha (+), angry (-), neverhillary (-).
([Figure 18](#figure_18))

<a name="conclusion"></a>  
## Conclusion
- Overall, predicting the four types of news is difficult due to the high variability in engagement reactions  relative to mean values. ([Figure 10](#figure_10))
- A quantifiable degree of success is achieved in relying on the source list compiled by Dr. Melissa Zimdards to predict the type of news of a Facebook post. Model score accuracy  on the test dataset is 16% above baseline
- Like is among the most common engagement reaction regardless of type of news, so it’s unclear why the fake and conspiracy coefficients for like are relatively small compared to the mainstream and satire coefficient for like. ([Figure 10](#figure_10)) Also suprprisingly, fake and conspiracy coefficients for the share reaction are relatively small. The proportion of share reactions in response to fake and conspiracy posts appears to be greater relative to the proportion of share reactions in response to mainstream news. ([Figure 11](#figure_11))
- A relatively high mainstream coefficient for the comment reaction may have been generated due the comment being the third most commong engagement reaction among high volume mainstream news pages, i.e. Fox News and CNN. ([Figure 11](#figure_11))
- Not surprisingly, haha is the second or third most common engagement response to satire news. ([Figure 10](#figure_10) and [Figure 11](#figure_11))
- Although all types of news have only a few sources that post a large portion of the total volume and receive much of the engagement ([Figure 3](#figure_3)), the model is not overfitting to the characteristics of the most prolific or engaged pages. Using the test datset, a review of the classified and misclassified posts by page confirms this observation. Both high and low volume fake and mainstream news pages are classified as their true class more often than misclassified, i.e. for 70% and 80% posts, respectively. Also, nearly all the posts for high and low volume conspiracy and satire sources are misclassified several times more than than the correctly classified posts. ([Figure 16](#figure_16))

<a name="further_work"></a>
## Further Work
- Calibrate and verify the model using more posts, messages, and news sources with a datset more evenly sampled among types of news than the current project dataset. Do proxies stand? More satire posts over a longer period of time are needed to understand broad patterns among message text and engagement activities, because the project dataset contains fewer satire posts relative to all other types of news. ([Figure 10](#figure_10))
- Vary number and types of words from message text used in the model. Possibly include n-grams of link names.
- Examine the content of the pre-categorized posts in the dataset and, if necessary, correct the label .
- Evaluate other feature engineering options to compare against the  performance of Lasso.
- Use GridSearch to vary the C parameter.
