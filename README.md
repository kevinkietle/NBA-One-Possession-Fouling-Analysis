NBA One Possession Fouling Project
==================================

In this project, I had the goal of researching take fouls in one possession games. The focus was take fouls from the losing team when down by one possession. Specifically, I only looked at situations in which there were more than 24 seconds remaining, implying that the team losing by one possession actually had a decision to make on whether or not to foul. I performed my research using Python, Pandas, Matplotlib, and Seaborn and data from [Schmadamco on Kaggle](https://www.kaggle.com/datasets/schmadam97/nba-playbyplay-data-20182019/code).

After cleaning the data, I also decided I wanted to do a quick dive into teams that committed a take foul while winning to avoid giving up a three or to exercise their foul to give. The cleaning is not tailored to that question, however.

Summary
-------

This project is broken down into two parts, in which you can find the links to below for full code and project. The rest of this readme will summarize key activities and findings from each section.

-   [1\. Data Cleaning](https://github.com/kevinkietle/NBA-One-Possession-Fouling-Analysis/blob/main/1.%20NBA%20One%20Possession%20Fouling%20Project%20-%20Cleaning.ipynb)

-   [2\. EDA](https://github.com/kevinkietle/NBA-One-Possession-Fouling-Analysis/blob/main/2.%20NBA%20One%20Possession%20Fouling%20Project%20-%20Analysis.ipynb)

**Main takeaways:**

-   For both fouling when up or when down, 5 seconds seems to be the accepted time from coaches that is needed for a team to have an effective chance of scoring.

-   Being down 3 greatly increases the odds that the trailing team will foul to extend the game. This is supported by about a 2% chance of winning when teams trail by 3 and do not foul. The dataset of teams fouling was not large enough to show whether fouling would increase or decrease that 2% chance, although it seems like the correct move.

-   When not down by 3, waiting seems to be the right play, especially if you have close to 5 seconds or more. If you can get a stop you have a chance to score. The reason it is tough when down 3 is because the winning team can milk the clock and foul you right back eliminating your chance to tie.

-   As for fouling when up, if you can commit the foul with five or less seconds remaining, this is a strong strategy.

-   Fouling with less than five seconds when up 3 essentially ends the game barring crazy circumstances.


Data Cleaning
-------

In this project, I pulled data that was scraped from Basketball Reference by another contributor on Kaggle. The link to that can be found here: <https://www.kaggle.com/datasets/schmadam97/nba-playbyplay-data-20182019/code>

The data from this set involves every play from every game from the 2015-2021 seasons as separate rows. Each season is its own csv that I combine together. This contributor pulled the data midway through the 2021 season so that dataframe is not fully complete. I decided to use this instead of scraping my own data for the sake of time and complexity given that there shouldn't be a huge factor of the results being outdated in just two years.

In my initial cleaning, I removed all data from the first three quarters as that does not affect this analysis. I then also removed all data from games that went to overtime as it would have required more complex code to see if the take foul strategy worked. In four quarter games, the WinningTeam column of the data should reveal if the strategy worked. However, in overtime games, it is possible for a team to successfully execute the strategy before losing in overtime. I also only kept plays from games beginning with less than 34 seconds left in the game. Anything beyond this and a team would not be intentionally fouling only down one possession.

Once that initial filtering was done, I wanted to clean further until the games remaining were teams exactly in the situation I outlined: being one possession down with the dilemma of whether to foul or not. To do this, I had these main goals with the data:

-   Filter down games to which the first play after 34 seconds remaining is an empty value or a rebound for the winning play

-   Make sure the first play was over 24 seconds

By doing this, I can ensure that the winning team has the ball and has the opportunity to foul. An empty value for the winning team play means the losing team play had a value, likely a made or missed shot. Additionally, I check that there are at least 24 seconds or else the losing team would have no choice.

In order to achieve this main logic, I had to remove a few edge case games. Each of the following edge cases were analyzed by the first row before the entire game's plays were removed if the criteria was met:

-   The losing team play in the first row being Offensive Rebound: This would mean the winning team play is empty but the losing team still has the ball and thus would not foul.

-   The winning team committing a foul: With how the play by play is tracked, fouls are logged in the column of the team drawing it. Therefore, a foul committed by the winning team would show an empty value for winning team play but the losing team still has the ball.

-   The losing team calling a timeout: This would again show an empty value in the winning team's column despite the losing team having the ball.

-   Either team executing substitutions of calling for a replay: This left the game ambiguous of who had the ball without manually reading the play by play.


EDA and Insights
-------

**Take Fouls When Down One Possession**

-   Teams have identified the 29 second mark as the cutoff in which they may need to foul to get the ball back. 5 seconds seems to be prevailing number as the amount of time accepted that is needed to score, as shown in the take fouls while up as well.

![photo 1a](https://user-images.githubusercontent.com/82183590/216804751-fbdb3678-3b66-422b-aed4-e908e0d1a9ec.JPG)

-   As expected when there is more than 30 seconds, more teams are willing to just wait and play out defense.

![photo 1b](https://user-images.githubusercontent.com/82183590/216804758-9193ffae-667f-4994-b772-675723acb1b6.JPG)

-   The win percentage seems to rise at about 30 seconds for teams that do not foul. This would lend itself to why teams mostly viewed this 29 second mark as when they fouled.

![photo 2](https://user-images.githubusercontent.com/82183590/216804766-26734177-7c7c-4c98-bc91-243f67061115.jpg)

-   Most teams that choose the take foul when they have the choice do so when they are down 3. This could be because they feel more desperate. If they are only down by 1 and give up a basket, they could still have a chance to tie. Down 2 or 3 and giving up a basket as well as having the majority of the remaining time milked leaves no chance to win the game.

![photo 3](https://user-images.githubusercontent.com/82183590/216804779-7802ca50-c944-46d0-95ae-f613292afefb.jpg)

-   This is contrasted by the distribution of teams not fouling. Much more evenly distributed with more instances of no fouls when down 1. 

![photo 3b](https://user-images.githubusercontent.com/82183590/216804784-c9424861-1f39-48fa-8c87-add9a1df1336.JPG)

-   Teams are much more likely to win when trailing by only 1 as opposed to 2 or 3. This is intuitive. But it also suggests the situation is not dire enough down 1 to foul. This is using rhe dataframe of teams not fouling as none of the teams that fouled actually won the game.

![photo 3c](https://user-images.githubusercontent.com/82183590/216804789-90122810-59a4-47de-a637-2530f86450d2.JPG)

**Take Fouls When Winning Late**

-   Teams are mostly fouling up when there is less than five seconds left and they are up at least 3. Situations in which a team fouls when only up 1 or 2 is when they have a foul to give. Any more than five seconds and the effect is minimized as opposing teams can still run a play. Or in the case of a foul up 3, an opposing team can still foul back with enough time to get the ball and shoot again.

![photo 4](https://user-images.githubusercontent.com/82183590/216804793-84ee813e-f19c-4b7d-830c-9cd1b41b29ab.jpg)

-   Seeing that this is the inverse of the dataset of fouling when down, it makes sense that a large majority of losses come when the opposing team is only down 1. Of course, the sample size of teams that did foul when up 1 with a foul to give is small, but this relatively large losing percentage would suggest that using the foul to give under 5 seconds is a strong strategy. Additionally, if a team is able to foul with under 5 seconds left while up by 3 that also seems to be a smart strategy. While the majority of teams still win without fouling, it seems like a semi-guaranteed win so long as your players are capable of hitting both free throws, which would put you back in the same situation but with less time.

![photo 5](https://user-images.githubusercontent.com/82183590/216804796-39b7675c-66d6-4c21-b907-4a70b9b6d47f.JPG)

-   I was just curious of this small sample size of who the teams were that did do the take foul while up. This strategy has been almost accepted as the correct strategy based on analytics, so it was interesting to see that some of the teams here are the ones we know to have strong investment in analytics. Of course Houston is one of the pioneers of the analytics movement in the last decade, and Atlanta and the Clippers are around the top third in terms of analytics department size.

![photo 6](https://user-images.githubusercontent.com/82183590/216804800-37fd6bec-2a04-4e47-9a43-5ddef0ae403e.JPG)

Caveats
-------

There are a few notes I would like to include about this project:

-   I was very stringent in removing edge cases so that there were no data points included in which the one possession fouling while down option was not there for the trailing team. As a result of the extensiveness of the cleaning, I probably removed many other instances in which a team did foul down one possession.

-   The analysis of the take foul while winning is even more imperfect as I used the same cleaned data from the previous goal as opposed to going through tailoring a cleaning process for a different question.

-   Because of the issue in the previous bullet I did a little cleaning in Excel to remove games in which the situation did not match what I was analyzing. I was able to dive into the play by play to determine this because the dataset of teams that employ the fouling while up strategy was small.

-   Removing all overtime games from the datasets made the analysis easier but probably removed instances in which the strategy was successful. If a team down fouls and is able to get the game to overtime, that would be considered a success in the strategy regardless of whether they win in OT or not. Likewise, for fouling while up, a team that allows the trailing team to force overtime would be considered a failure regardless of whether they win in OT or not.

-   One angle of analysis on both questions that I did not get to address because of its lack of data in the set is timeouts remaining. Of course, if a team has a timeout to advance the ball the strategy (and likely results) changes drastically.

Hope you enjoyed reading this project!

Feel free to contact me with any questions or inquiries.

LinkedIn - <https://www.linkedin.com/in/kevinkietle/>

Email - <kevinkietle@gmail.com>
