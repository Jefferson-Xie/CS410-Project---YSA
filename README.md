# CS410-Project---YSA Documentation

Author: Jefferson Xie, jjxie3
Video Presentation: https://drive.google.com/file/d/17PHx3kL1fPkjQhyZnHcJ1xZVoc4KIOQ6/view?usp=sharing or https://youtu.be/FqCM7XG626o

## Overview
Ever get curious on how other people feel about a YouTube video? While YouTube still has their famous like and dislike buttons, as viewers we can no longer see the exact number and ratio anymore. This led to the idea of performing sentiment analysis (SA) on the YouTube videos' comment section in order to determine the overall 'likability' of the video. Adding my own interest in this, I decided to perform SA for the skins of different champions from the online game, League of Legends.

![](https://www.mobafire.com/images/champion/skins/portrait/alistar-unchained.jpg)  ![](https://www.mobafire.com/images/champion/skins/portrait/alistar-moo-cow.jpg)
> How well does a skin that turns Alistar blue compare to him wearing a Cow Costume?

----
## Installation
The easiest way to install would be to simply copy all the relevant files and drop them into a google drive and run the notebook through Google Collab. Make sure the files are all in the same folder. Next you will need to create a Strawpoll API key from here to run the cells related to fetching data from Strawpoll (or don't since I included an alternative CSV file) and a Google API key to fetch YouTube video comments (this one you must have).

## Methodology
One of the most important aspects of testing is verification, so beginning the project I knew I had to grab some kind of survey data for the different skins in order to compare my final SA results to.

In this case I pulled by survey results from a survey conducted in 2022 from reddit which can be found [here](https://www.reddit.com/r/leagueoflegends/comments/124kb99/best_skins_per_champ_2022/ "here").

From there I got all of the links to the different champion surveys and stored them to be used by the [strawpoll API.](https://strawpoll.com/docs/api/ "strawpoll API.")

Because the strawpoll API is a bit slow, I also included a csv file which contains all the Champion/Skins/Votes that can be imported to the notebook.

I then included an interactive cell which allows the user to select a champion of interest from a dropdown menu and displays their strawpoll vote results.

![](https://github.com/Jefferson-Xie/CS410-Project---YSA/blob/main/images/SelectChamp.png)

Next I created my YoutubeInfo class which holds all my relevant Youtube API calls. It is used to both search for videos on a [specific YouTube channel](https://www.youtube.com/@SkinSpotlights "specific YouTube channel") that covers all new skin releases.

Next there are some cells to help verify our dataframes are good to go before grabbing all the comments for the different skins of a specified champion and store them in a different dataframe.

Following next is the SentimentAnalyzer class which handles all the tasks related to the [VADER SA](https://www.nltk.org/_modules/nltk/sentiment/vader.html "VADER SA") such as determining the compound score for a comment and indentify if the comment is positive, negative, or neutral comments. The class also tracks the mean compound sentiment for all the comments of a skin before passing it back to be stored.

We then create a new data frame to display the results.

![](https://github.com/Jefferson-Xie/CS410-Project---YSA/blob/main/images/SentimentAdded.png)

## Results and Evaluation
While working on this project, I have come to learn how powerful NLTK's VADER sentiment analysis tool is. Originally, I wanted to test my comments data before and after preprocessing to verify the impact preprocessing can have on NLP tasks. This includes tasks such as converting all chars to lower chase, removing special characters and emojis, [lemmatization](https://www.datacamp.com/tutorial/stemming-lemmatization-python "lemmatization"), etc.

However upon closer inspection of the VADER SA API, I quickly learned that a lot of these preprocessing tasks are already built into VADER and it would be redundant for me to do it twice. VADER's text preprocessing is extrememly powerful as it handles all of the standard preprocessing steps as well as some extra capabilities such having an extensive lexicon that can handle "Internet slang" terms such as "wtf" or "GZGZ".

Another thing of note is that while the SA scores for the Lux skins lined up nicely (Elementalist Lux having both highest compound score as well as having the greatest number of votes), there is a major issue when evaluating skins (and ranking favorability in general) like this. The issue is the "apples-to-oranges" argument.

These cosmetic skins do not come out all at the same time, which means a lot of the data from comments for older skins may be more positive simply because they came out earlier and people didn't have as high a standard at the time, resulting in score that would be much higher than if the comments were made with today's visual standards.

While VADER SA is a powerful tool in determining the overall sentiment for different comments, it cannot be used on its own in evaluating how different objects compare with each other. While it can easily determine if someone “likes crispy apples” or “enjoys juicy oranges”, it can be difficult to determine if that person prefers apples to oranges.
