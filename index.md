# Non-Technical Report

Steam, being one of the largest platforms of which games are sold on, creates a lot of revenue each year just from game sales. The reviews of a video-game very directly affect it's overall success: With more reviews it gets, the more representative the overall rating is surrounding the video-game. People see advertisements for games whenever they go to the store page, based on the overall summation of it's reviews and how users are reacting to the game play experience it definitely has an effect on how many people see the video-game and make a judgement as to whether or not they want to buy the video-game, based on those reviews. Games that receive better ratios of reviews recommending to not recommending also appear higher on the priority list of what people see when operating the Steam client.

Recently, Steam has changed their policy regarding how reviews are processed. Before you could give game keys to players following your game, however this was causing dilemma as the developers were giving incentive to the users they gave keys to write good reviews about the game, this resulted in inflation of a video-game's user reviews, and thus, allowing them to receive more publicity overall, because they gave their players reason to positively rate their game. Now, any accounts given keys by a game owner/developer **(while the game is in beta)** can no longer write reviews about that game without actually purchasing it.

A lot of things affect reviews, not only the experience the reviewer has but also whether or not the reviewer is familiar with the developers or franchise prior to reviewing, though not explored in my analysis, I believe this would have predictive power as someone who has played predecessor games to another game they're reviewing might be more or less lenient about what kind of rating they give to the game.

Predicting this success from features in the reviews would be helpful not only to investors thinking of helping independent developers expand their horizons, but also for the developers themselves in determining where they need to make improvements as there are multiple forms of analysis that can be performed on reviews that indicate different things on how the reviewer is reacting to or has reacted to the game up till posting the review.

![](https://rawgit.com/DanielEMoreno/DSI-SF-2-DanielEMoreno/master/code/projects/capstone/Plots/review_counts.png)

Above, the counts of reviews for each game, the games are denoted by their specific 'App ID' in the Steam program.

Given the problem and only steam review data, defining a metric of success was not an easy task. With columns encompassing multiple different metrics about the review itself, it is only a representation of the reviewer's attitude toward the videogame at that point in time. To create a metric of success given the data, I take the amount of hours the user has played thus far (as of 9/24/216) compared to the recorded total time played at the time of their review being posted, it potentially could provide for a relationship between the valence of the review and the amount of time the reviewer plays the videogame following the review.

The task of cleaning/munging the data was clearly defined as I had a column of potential user profile ID numbers of which I could feed into the steam web API in order to retrieve the amount of hours the user has played. However some items in the dataset were not the actual user ID, some were the user alias. To combat this I used a third party class to retrieve the user ID from the link provided. The only data cleaning that was actually required was conversion of the timestamp column that indicated when the user had posted the review, and when I had to convert the time I fetched for each reviewer; when I fetched the time played it was in minutes.


![](https://rawgit.com/DanielEMoreno/DSI-SF-2-DanielEMoreno/master/code/projects/capstone/Plots/class_imbalence.png)

When coming to a decision on a model to use for depicting a linear relationship it was clear to me that the quickest, and easiest model to use was a Linear Regression. It would be able to tell me right away whether or not there was a relationship between my predictor and the target. There was also a very noticeable problem with the distribution of classes I had available. For the case of linear regression it wouldn't be an issue as the sentiment rating isn't based on whether or not the review recommends the game or not.

Below, the distribution of difference in hours played across all of the videogames. The observation to highlight is that most of the games have a distribution that ranges between 0-1000, but some of the games have a more spread out distribution of additional  hours logged, the two that are greater are the yellow and red band that span greatly, DotA2, and Counter Strike: Global Offensive

![](https://rawgit.com/DanielEMoreno/DSI-SF-2-DanielEMoreno/master/code/projects/capstone/Plots/download%20(7).png)

To define the valence of my reviews I use the Textblob sentiment function. This is subjective, as is any sentiment rating, it is even more subjective however because the different words and phrases passed around between the different communities of the games. Without a class imbalance I prospectively would be able to get common words between reviews that don't recommend the game and reviews that do recommend the game it's reviewing. With this I could determine common topics discussed for games that have certain ratios of reviews recommending/not recommending the game, and thus be able to predict a video-game being taken in by a community and thriving by what people think and say about the game.

![](https://rawgit.com/DanielEMoreno/DSI-SF-2-DanielEMoreno/master/code/projects/capstone/Plots/regplot_regression.png)

For all the datasets observed, the regression line and distribution of values is pretty identical to the one above, resulting in an r^2 value between -0.00~ and 0.00~, both showing little to no relationship between the features what-so-ever. However this is very open to interpretation as the Textblob sentiment definition may rate the sentiment incorrectly due to the different language used by the communities of the video-game. It is clear in the regression-plot that there are users on both ends of the spectrum: both with negative scores of valence and enormous amounts of difference in hours played, and with positive scores but minuscule amounts of difference in time played.

## Event Specific Reviews

After ruling out the relationship between sentiment and the amount of additional hours a reviewer has played the game they reviewed, there must be a better reason for why there is such an imbalance of classes. Investigating this with a Bernoulli Naive Bayes classifier pretty much a perfect fit. The classifier is fast to use, only requiring one pass over the data. In some cases it is not choice to use Naive Bayes as it assumes independence of all observations. This is not an issue when it comes to classifying the video game reviews as they are independent of one another -- It seems highly unlikely that someone would post a review solely because of another person's review, there is definitely influence however that comes from the reviewer reading other reviews since someone may highlight something that the reviewer hadn't thought of previously.

The following is a set of features and their likelihoods of appearing in a review that recommends the game, and that doesn't recommend the game for Skyrim

| Not Recommend   |      Recommend      |  Feature |
|----------|:-------------:|------:|
| 0.697143 |  0.252535 | mods |
| 0.486857 |    0.621198   |   game |
| 0.425143 | 0.035023 |    paid |
| 0.331429 | 0.027650 | paid mods |
| 0.289143 | 0.013825 | valve |
| 0.212571 | 0.062673 | bethesda |
| 0.202286 | 0.114286 | mod |
| 0.184000 | 0.014747 | pay |
| 0.147429 | 0.321659 | skyrim |
| 0.145143 | 0.074654 | community |

From observing the liklihoods we can see more clear distinctions like the likelihood of 'mods' appearing in reviews that don't recommend the game versus how likely it is to appear in the latter, also weith features 'paid mods', 'paid' and 'pay' also with a higher likelihood of being in reviews not recommending the game.

After seeing this it was necessary to do more exterior research about Skyrim and it's situation with mods.

![](https://rawgit.com/DanielEMoreno/DSI-SF-2-DanielEMoreno/master/code/projects/capstone/Plots/GRAPH%20WITH%20LABELS%20JUAN.png)

"Just a few days after launching paid mods on Steam through the Skyrim Workshop, Valve is removing the payment option, citing customer feedback. Valve says it will refund Steam users who purchased a Skyrim mod on Steam" (Michael McWhertor - Polygon)

With initiating paid mods on steam, for Skyrim specifically, it's home in the Steam store was ravaged by a bunch of angry consumers who had bought the game previously and felt entitled to modifying their game, at no expense.

| Not Recommend   |      Recommend      |  Feature |
|----------|:-------------:|------:|
| 0.666667 |  0.562715 | game |
| 0.399038 |    0.128877   |   rockstar |
| 0.259615 | 0.017574 |    sale |
| 0.248397 | 0.023432 | price |
| 0.240385 | 0.085803 | people |
| 0.231571 | 0.232943 | gta |
| 0.223558 | 0.199173 | online |
| 0.219551 | 0.135768 | get |
| 0.196314 | 0.139904 | like |
| 0.188301 | 0.062715 | money |

Again, with the set of Grand Theft Auto reviews we see terms that have similarities appearing within one class of review: In not recommended there is 'sale', 'price', and at the bottom of the list, 'money'. Having seen the results of the previous game I was certain I would find something relating to this game and why people who don't recommend the game talk frequently about the price, a sale, and money.

![](https://rawgit.com/DanielEMoreno/DSI-SF-2-DanielEMoreno/master/code/projects/capstone/Plots/GRAPH%20WITH%20LABELS%20DOSE.png)


Above we can see the intervals that encourage more and more people to write reviews, these intervals show the community specific events that encourage players to write reviews, updating the video-game's stature.

"At a glance, it looked like the price of the game had been jacked up and then discounted by 25%, making it just shy of the regular price. But clicking on the deal revealed something even more disappointing. GTA V had actually been removed from the store, and replaced with a bundle that included some of GTA Online’s in-game currency, and that’s what had been discounted. " - (Fraser Brown PCGamesN)

We see the same fashion of reviews as before, the company made a mistake and the community responded to that mistake through writing buyer's-remorse reviews.

### The International

To further assess these 'event specific' reviews, I thought it would be interesting to try and classify differences between the reviews surrounding DotA's yearly tournament, The International

It's a tournament that happens usually during the summer each year which has an enormous prize pool contributed to by the players themselves through monetized purchases they make in-game. Its very important as valve keeps 75% of the revenue collected through contributions. The base item you must purchase costs $10, so of that they keep $7.50, $2.50 going to the prize pool itself. With the 2016 prize pool totaling $20,770,460, that's a lot of revenue that Valve keeps for them.

To do this, with the data given, I was only able to create a relevant model of one of the tournaments. This opened my horizon of data to the reviews I did not use previously to predict the amount of hours a user played related to the sentiment of their review. Different from what I did previously, I used a Multinomial Naive Bayes classifier to check the occurrence of words between the time periods instead of checking the existence of words between the two classes.

 |   Predicted is Tourneytime      |  Predicted is not Tourneytime |
|----------|:-------------:|:------:|
| is Tourneytime (True) |  483 |   70   |
| is not Tourneytime (True) |    314   |   108   |

![](https://rawgit.com/DanielEMoreno/DSI-SF-2-DanielEMoreno/master/code/projects/capstone/Plots/roc.png)

This ROC curve shows the TPR (True positive rate) on the Y axis and the FPR (False Positive Rate) on the X axis. The AUC is more or less a measure of how good the model is, however it is not completely conclusive on how good the model is, this only measures the rates of true positives and false positives, not also encompassing true negatives and false negatives. The AUC has varying relevance depending on the problem at hand.

In classifying reviews for being posted around the time of the International the Multinomial Bayes model performed about 3% better than if I had made a judgement call myself.

![](https://rawgit.com/DanielEMoreno/DSI-SF-2-DanielEMoreno/master/code/projects/capstone/Plots/download%20(6).png)

At the different intervals, we can see there is no real indication of users reviewing the game following different milestones before, during, and after The International, except for following the cosmetic update. 

## Conclusions:

In Summation, using the sentiment of a review isn't very useful in determining how much more a user will play a game. Perhaps if there was a deeper rating that was numerical, not black and white, there would be a better representation of how much a reviewer actually liked the video-game.

As for event specific reviews, it seems as though people are more inclined to review video-games following an  event when that event isn't received well by the community surrounding the video-game.

Being able to classify the possibility of competition for a videogame would be super profitable in the case of independently developed video-games, however this brings to light another issue with the video game industry which is definition of what it means to be 'in beta' and 'in alpha'. However it is becoming increasingly popular to put a version of a game out in beta, allow interested players to try the game out and give feedback, and then change their objectives accordingly. There have been many games that have gone into beta for a duration and come out on the other side very successful, the popular example of this is Minecraft. DotA 2 was in beta, however this is different than the case of Minecraft as it is the successor to DotA1. 

With sites like Kickstarter offering video-game sections it creates a big opportunity for investors to look at each video-game and judge whether or not it would be a sound investment. A lot of developers face issues with funding, and if we were able to determine whether a video-game was a worthwhile investment based on it's reviews in beta, a lot of the video-game developers that require funding for things like anti-cheat, servers, etc, could finally get the funding they need.