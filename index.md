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

<style>
em {
  font-style: italic
}
</style>

## The motivation

* [Lawmakers Who Upheld NSA Phone Spying Received Double the Defense Industry Cash](http://www.wired.com/2013/07/money-nsa-vote/).

---

## The working example

*  On July 24, 2013, shortly after the first wave of Snowden revelations, US Representative Justin Amash (R, MI) introduced the "Defund the NSA" amendment.
* It failed 217 to 205, in line with the wishes of:
 - the President of the United States (on record  [here](http://www.businessinsider.com/amash-amendment-nsa-white-house-obama-veto-2013-7))
 - security and defense industry interests (the surveillance state is a customer)
 - US citizens who don't think that the NSA is doing anything wrong, and whose representatives carried the day (this is just how the system should work)

--- .class #id 

## Some questions you might ask

1. So what if No votes collected more from defense interests than Yes votes did? Do US representatives really respond to funding from special interests?
1. If they do, is there a way to get an idea of how much might do the job?
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

* The first assumption mirrors the sentiment of every third-party candidate and disengaged citizen, that the two main parties are interchangeable; you may be sympathetic, but if the second assumption fits the data better, that's evidence that you're wrong.
* If under either assumption the response is flat over a feasible range of funding, this can be read in two ways:
 - special interests are wasting their time; when the vote does go their way, it's simply that what they want matches what the majority wants
 - special interests aren't buying retail; instead, they get the votes they want *by keeping representatives from voting across party lines*; this simple model of pricing individual representatives does not fit that use case.
* In other words, if you see no increase in the probability of voting your way as you increase the funding, don't bother trying to find the right price: either the politicians are not for sale, or you'll have to buy the whole party.

---

## The quantity of interest

* We will run a logit under each assumption above, but won't report the parameter estimates. They're not what's of interest.
* Instead, we will plot the *expected probability of a No vote over a reasonable range of funding levels*, by party, and simulate the uncertainty surrounding it.
* This will immediately tell an interested party how much they should expect to pay for a vote in their favor, with the probabillity of such a vote shown within a chosen confidence band -- 80%, 95%, etc.
* This is easy to do with [Zelig](http://zeligproject.org/); the mechanics were explained [here](http://gking.harvard.edu/files/abs/making-abs.shtml) in 2001(!)

---

## The proof of concept

<iframe src=https://ghuiber.shinyapps.io/Amash/></iframe>

---

## Don't read too much into it

* The Amash amendment was a great candidate for a simple proof-of-concept. 
 - It failed narrowly; this produced balanced classes, which helps the logistic regression classifier fit response curves within narrow probability ranges. 
 - But it did not fail along party lines; this left enough of the variation in `Vote` to be explained by `Amount`, rather than `Party`.
 - The Yes side got no funding. This made things a little easier to reason about.
 - This was an ideologically polarizing issue, but not a partisan one. 
 - This amendment was not likely to pass. If that's how it was perceived going into the voting, it was more like a photo opp where individual representatives would be happy to cross party lines and score easy points for integrity, on either side.

--- 

## A more interesting use case
 
* For an issue with money on both sides, no ideological baggage, and no partisan positions staked out, this app can use MapLight data to price both Yes and No outcomes. It's an easy tweak. 
* This would be the ideal use case, where finding the correct level of funding could actually change the result of the vote, not just look like it might.
* This is work in progress. MapLight data is easy to scrape with [`rvest`](http://cran.r-project.org/web/packages/rvest/index.html). If I turn up a handful of examples that fit these requirements, that may be enough for the seed round.

---

## What might break the app

* For an issue with money on both sides but very stark partisan differences, this app will show voting probabilities by party on either side of the 50% line and staying far from it at all levels of funding. 
 - If this is what you see, it's worth asking whether the piles of money being spent on this particular issue are wasted, or you're seeing endogeneity at work, like with [Milton Friedman's thermostat](http://themonkeycage.org/2012/07/31/milton-friedmans-thermostat/): massive funding that looks like it's being wasted is actually what *keeps* individual representatives from crossing party lines.
 - The app cannot answer this question. It will return the trivial result that you shouldn't bother spending anything at all.
 - [H.R. 37 - Promoting Job Creation and Reducing Small Business Burdens Act](http://maplight.org/us-congress/bill/114-hr-37/6586030/total-contributions) might be the perfect example: Republicans loved it, Democrats hated it, special interests funded generously both sides, and the bill passed almost strictly along party lines.

---

## A battle too big for this app

<iframe src=https://ghuiber.shinyapps.io/hr37/></iframe>

---



## How could you have told, before running the app?


* Amash votes crossed party lines:

```
##    
##     Yes  No
##   D 111  83
##   R  94 134
```

* HR 37 votes, not so much:

```
##    
##     Yes  No
##   D  29 153
##   R 242   1
```

---

## Thank you

The code is on [GitHub](https://github.com/ghuiber/Amash)
