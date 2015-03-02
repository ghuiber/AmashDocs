---
title       : An app for pricing US Representatives
subtitle    : don't hire an expensive lobbyist if a competent bag man will do
author      : Gabi Huiber
job         : consultant
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [shiny,mathjax] # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides

--- 

## The motivation

* [Lawmakers Who Upheld NSA Phone Spying Received Double the Defense Industry Cash](http://www.wired.com/2013/07/money-nsa-vote/).

---

## The working example

* In response to the first wave of Snowden revelations, US Representative Justin Amash (R, Mich.) introduced the "Defund the NSA" amendment on July 24, 2013.
* The amendment failed 217 No to 205 Yes, in line with the wishes of:
 - the President of the United States (on record  [here](http://www.businessinsider.com/amash-amendment-nsa-white-house-obama-veto-2013-7))
 - security and defense industry interests (the surveillance state is a customer)
 - US citizens who don't think that the NSA is doing anything wrong, and whose representatives carried the day (this is just how the system should work)

--- .class #id 

## Some questions you might ask

1. OK, so No votes collected more from defense interests than Yes votes did. So what? Do US representatives really respond to funding from special interests?
1. If it turns out that they do, could you get an idea of how much might do the job?
3. Could you use the recipe for pricing votes on other issues where interested parties might be curious?

---

## The model

* With data from [here](http://maplight.org/us-congress/bill/113-hr-2397/1742215/contributions-by-vote?sort=asc&order=$%20From%20Interest%20Groups%3Cbr%20/%3EThat%20Opposed&party[D]=D&party[R]=R&party[I]=I&vote[AYE]=AYE&vote[NOE]=NOE&vote[NV]=NV&voted_with[with]=with&voted_with[not-with]=not-with&state=&custom_from=01/01/2011&custom_to=12/31/2012&all_pols=1&uid=44999&interests-support=&interests-oppose=D2000-D3000-D5000-D9000-D4000-D0000-D6000&from=01-01-2011&to=12-31-2012&source=pacs-nonpacs&campaign=congressional) and logistic regression, you can estimate the probability of a No vote as a function of party affiliation and industry funding.
* You can do this under two assumptions: 
 - there's only a baseline difference but representatives from either party respond to funding the same way: 
 $$
 P(No) \sim f(Party + Amount)
 $$
 - there's a difference both at baseline and in how representatives from each party respond to funding: 
 $$
 P(No) \sim f(Party * Amount)
 $$


--- 

## Interpreting the model

* The first assumption mirrors the sentiment of every third-party candidate, whether Green, Socialist, or Libertarian, and of every disengaged citizen, that the two main parties are interchangeable; you may be sympathetic, but if the second assumption fits the data better, that's evidence that you're wrong.
* If under either assumption the response is flat over some plausible range of funding, that is evidence that special interests are wasting their time: when the vote does go their way, it's simply that what they want matches what the majority wants. 
* In other words, if you see no increase in the probability of voting your way as you increase the funding, don't bother trying to find the right price: the politicians are not for sale.

---

## The quantity of interest

* We will run a logit under each assumption above, but won't report the parameter estimates. They're not what's of interest.
* Instead, we will plot the *expected probability of a No vote over a reasonable range of funding levels*, by party, and simulate the uncertainty surrounding it.
* This will immediately tell an interested party how much they should expect to pay for a vote in their favor, with the probabillity of such a vote shown within a chosen confidence band -- 80%, 95%, etc.
* This is easy to do with [Zelig](http://zeligproject.org/); the mechanics were explained [here](http://gking.harvard.edu/files/abs/making-abs.shtml) in 2001(!)

---

## The app

<iframe src=https://ghuiber.shinyapps.io/Amash/></iframe>

---

## Some disclaimers

* The Amash amendment was a great candidate for this exercise. 
 - It failed quite narrowly, so you had balanced classes, which helps the logistic regression classifier, so you get these fairly narrow probability ranges with quite high confidence. 
 - The Yes side got no funding from anywhere. In a typical issue you have money punted on both sides, so the equation has to take that into account.
 - This was an ideologically polarizing issue, pitting law-and-order Republicans and blue dog Democrats on one side against old-school hipppie Democrats and libertarian-leaning Republicans on the other. This is the cool kind of fight that will produce a visible baseline difference. 
* So, a great follow-up would be to pick some more evenly-fought issue, with money on both sides and ideological differences less stark, where there's voting and MapLight data available, and repeat the exercise with the model expanded accordingly. [Here](http://maplight.org/us-congress/bill/114-hr-37/6586030/total-contributions) is one example.
