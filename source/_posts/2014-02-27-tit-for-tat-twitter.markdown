---
layout: post
title: "Tit for Tat Twitter"
date: 2014-02-27 22:12
comments: true
categories: [Twitter, R Programming]
---

I'm not really a Twitter pro, but after a few years with an
account and periodic activity (I was and am still gunning for
[Diaspora](https://joindiaspora.com/)), I had followed enough
accounts to make my Twitter stream a little unwieldy, and this
meant that I was missing out on tweets by people I'd actually like
to pay attention to. 

So, with a little time on my hands this evening (my wife and son
are out of town), I thought I'd play with the awesome
[twitteR](http://cran.r-project.org/web/packages/twitteR/index.html)
package for the [R Programming](http://cran.r-project.org/)
language. What I wanted to do, especially because the Twitter web
interface is no good with this task, was list all those accounts
that I follow but that do not follow me and all those that follow
me and that I do not (yet) follow. Let's call this initial effort
**Tit for Tat Twit**. And this really is just an initial run at
the task. It can do for some optimizing --- right now just working
out the logic and getting more familiar with the *twitteR*
package and Twitter's API.

    library(twitteR)

    # Set up Twitter authorization variables
    # Requires creating a Twitter app: https://dev.twitter.com/
    requestURL      <- "https://api.twitter.com/oauth/request_token"
    accessURL       <- "https://api.twitter.com/oauth/access_token"
    authURL         <- "https://api.twitter.com/oauth/authorize"
    consumerKey     <- "keyHere"
    consumerSecret  <- "secretHere"
    twitCred        <- OAuthFactory$new(consumerKey = consumerKey,
                                        consumerSecret = consumerSecret,
                                        requestURL = requestURL,
                                        accessURL = accessURL,
                                        authURL = authURL)

    # Initiate contact with Twitter
    twitCred$handshake()
    registerTwitterOAuth(twitCred)

    # Get my account info; insert your handle in the quotes
    tuser <- getUser("twitterHandleHere")

    # Examine 'environment'
    ls(tuser)

    # Examine 'fields' in 'tuser' environment; just to test
    tuser$url
    tuser$name

    # Convert reference class 'lastStatus' into data frame
    # with fields; only will return a data frame with one
    # observation; just to test
    lsCsbStat <- as.data.frame(tuser$lastStatus)

    # Examples of methods available; look at environment to
    # see more
    following <- tuser$getFriends()
    followers <- tuser$getFollowers()
    followers <- twListToDF(followers)
    following <- twListToDF(following)

    ## Part 1: Tit4Tat_Twit.R

    ### List my followers that I do not follow
    #### And save as a data frame
    followers$Ifollow <- followers$screenName %in%
        following$screenName
    followers$screenName[followers$Ifollow == "FALSE"]
    noIfol <- as.data.frame(cbind(
        followers$screenName[followers$Ifollow == "FALSE"],
        followers$name[followers$Ifollow == "FALSE"]))
    names(noIfol) <- c("Screen Name", "Name")

    ### List following that do not follow me
    #### Save as a data frame
    following$noFollowMe <- following$screenName %in%
        followers$screenName
    following$screenName[following$noFollowMe == "FALSE"]
    noFollMe <- as.data.frame(cbind(
        following$screenName[following$noFollowMe == "FALSE"],
        following$name[following$noFollowMe == "FALSE"]))
    names(noFollMe) <- c("Screen Name", "Name")

    ## Part 2: Tit4Tat_Twit.R
    ## Same thing as first part above, just written
    ## differently

    ### I'm following them but they are not following me.
    following$FollowMe <- following$screenName %in%
        followers$screenName
    meNotThem <- subset(cbind(following$screenName,
                              following$name),
                        following$FollowMe == FALSE)

    ### They're following me but I'm not following them.
    followers$FollowMe <- followers$screenName %in%
        following$screenName
    themNotMe <- subset(cbind(followers$screenName,
                              followers$name),
                        followers$FollowMe == FALSE)

    ### Intersection; Not thoroughly tested yet.
    meNotThem[,1] %in% themNotMe[,1] ||
        themNotMe[,1] %in% meNotThem[,1]

I used the resulting info to follow some folks that had followed
me but for some reason I had missed them. I had followed a lot of
news and other such types of accounts and I unfollowed those but
kept them in lists so I can check on them every once in a while.
