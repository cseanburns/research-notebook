---
layout: post
title: "Twitter Data and Social Networking Analysis"
date: 2016-05-12 15:27:33 -0400
comments: true
categories: [sna, social network analysis, twitter, R]
---

I'm new to social network analysis, and like anyone with a lot to learn,
I've been reading papers and example applied scenarios and code. Although
there are a number of these examples, and many of these use Twitter as a
data source, most Twitter examples seem to analyze the relations between
terms rather than actors (or Twitter users), for example, see
[http://www.rdatamining.com/examples/social-network-analysis][1]. Or if they
do look at actors, the examples are incomplete in some way and are thus
difficult to apply (well, maybe for a newbie like me), especially to the
kind of data that we are able to extract from the Twitter API using the
popular [twitteR package][2]. This data does provide account information,
such as the screen name of the tweeter as well who that tweeter is replying
to (see *replyToSN* variable in Twitter source data), if they are replying,
but it only records the first Twitter account mentioned in the tweet and not
all the accounts the tweet is replying to, if the tweet is replying to
multiple accounts. For example, the following #rstats tweet by @jaimedash is
a reply to @sharon000 and @calli993 but the Twitter variable *replyToSN*
only shows that it's a reply to @sharon000 (this tweet was picked randomly).

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/sharon000">@sharon000</a> <a href="https://twitter.com/calli993">@calli993</a> another option to upgrade <a href="https://twitter.com/hashtag/rstats?src=hash">#rstats</a> easily is installr <a href="https://t.co/Ua9JSxnDDj">https://t.co/Ua9JSxnDDj</a></p>&mdash; Jaime Ashander (@jaimedash) <a href="https://twitter.com/jaimedash/status/730497939143974913">May 11, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Thus, it was a nice surprise to read about the new [tidytext][3] package,
authored by Gabriela De Queiroz, [David Robinson][6], and [Julia Silge][7],
and demonstrated on [Silge's blog][4]. When I read about this package, my
immediate thought was that it would make it much easier to parse the text of
the tweets for all the account information. That is, I could use it to
tokenize each tweet and associate each token to the respective account name.
To proceed, I just need to analyze the data that contains account
information for both columns (tokens that equal source accounts). Overall,
the steps include:

1. Retrieve Twitter data based on a search query using the twitteR package
1. Convert the data to a data frame
1. Create a keep words list (opposed to a list of stop words) based on the screen name variable
1. Save tweets and screen names in a separate data frame
1. Tokenize each tweet, one token per row and associate each token with the tweeter's account name
1. Filter token list by the keep words list (this means we keep only those tokens that are screen names and remove all other tokens, i.e., the content of the tweets)
1. Plot the data

I still have a lot to learn, generally, about SNA. Plus, I'm not sure yet if
the process here captures the entire network. I also need to better
understand the igraph package (it'll help if I work with more standard
data). Later, I'll tackle the [ggraph package][5] for creating nicer looking
plots, but so far, here's what I've been able to do. The following code
analyzes recent tweets containing the hashtag **#rstats**:

    library(twitteR)
    library(dplyr)
    library(igraph)
    library(tidytext)

    consumer_key     <- "[enter consumer_key here]"
    consumer_secret  <- "[enter consumer_secret here]"
    access_token     <- "[enter access_token here]"
    access_secret    <- "[enter access_secret here]"
    twitCred         <- setup_twitter_oauth(consumer_key,
                                            consumer_secret,
                                            access_token,
                                            access_secret)

    # Search Twitter for #rstats for the last three days
    rtalk <- searchTwitter("#rstats",
                           n = 1500,
                           since='2016-05-09',
                           until='2016-05-12')

    # Convert results to data frame
    rtalk <- twListToDF(rtalk)

    # Maintain a list of screen names
    keepWords <- rtalk$screenName

    # Save tweets and screen names
    rtalk.ts <- data.frame(rtalk$text, rtalk$screenName)

    # Rename variables
    names(rtalk.ts) <- c("text", "screenName")

    # Convert tweets to character class
    rtalk.ts$text <- as.character(rtalk.ts$text)

    # for each user's tweet, split into component tokens and save in new df
    rtalk.at <- rtalk.ts %>% unnest_tokens(accounts, text)

    rtalk.at$screenName<- as.character(rtalk.at$screenName)

    # keep only tokens (parts of tweets) that were screen names
    rtalk.at <- filter(rtalk.at, accounts %in% keepWords)

    # Plot social network
    set.seed(1234)
    plot(graph_from_data_frame(rtalk.at))

![alt text](https://dl.dropboxusercontent.com/u/55752964/octopress/sna-rstats-crowded.png "Crowded SNA Plot of #rstat tweets")

It seems to work but the plot is overfilled and thus difficult to read. We
can fix the crowdedness, and have a more accurate picture by removing those
tweets that are simply retweets (likely just noise, depending on the
question asked of the data), and then recreating the plot:

    # Since the network is too crowded, start over and remove all retweets
    rtalk2 <- filter(rtalk, isRetweet == FALSE)

    rtalk.ts2 <- data.frame(rtalk2$text, rtalk2$screenName)

    # Rename variables
    names(rtalk.ts2) <- c("text", "screenName")

    # Convert tweets to character class
    rtalk.ts2$text <- as.character(rtalk.ts2$text)

    # for each user's tweet, split  into component tokens and save in new df
    rtalk.at2 <- rtalk.ts2 %>% unnest_tokens(accounts, text)

    rtalk.at2$screenName<- as.character(rtalk.at2$screenName)

    # keep only tokens (parts of tweets) that were screen names
    rtalk.at2 <- filter(rtalk.at2, accounts %in% keepWords)

    # Plot social network
    set.seed(1234)
    plot(graph_from_data_frame(rtalk.at2))

![alt
text](https://dl.dropboxusercontent.com/u/55752964/octopress/sna-rstats-clean.png
"Cleaner SNA Plot of #rstat tweets")

Here the plot is much clearer and patterns emerge. This makes sense given
the centrality of, e.g., @hadleywickham. Still, there's more to learn and do
to make sure this is a valid approach. Any comments welcome.

[1]: http://www.rdatamining.com/examples/social-network-analysis
[2]: https://cran.r-project.org/web/packages/twitteR/README.html
[3]: https://cran.r-project.org/web/packages/tidytext/index.html 
[4]: http://juliasilge.com/blog/Life-Changing-Magic/
[5]: https://github.com/thomasp85/ggraph
[6]: https://twitter.com/drob
[7]: https://twitter.com/juliasilge
