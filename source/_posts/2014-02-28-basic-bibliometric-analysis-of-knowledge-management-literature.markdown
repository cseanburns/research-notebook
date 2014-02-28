---
layout: post
title: "Basic Bibliometric Analysis of Knowledge Management Literature"
date: 2014-02-28 17:54
comments: true
categories: [knowledge management, bibliometrics, lis658]
---

## Basic Bibliometric Analysis of Knowledge Management Literature listed in Web of Science

**Basic data collection process:**

* Search for term "knowledge management" in Web of Science
* Refine *Publication Years* to 2009-2013
* Refine *Research Domains* to *Social Sciences*
* Refine *Document Type* to *Article*
* Refine *Languages* to *English*
* Result includes 1,380 records
* Sort by *Times Cited -- highest to lowest*
* Click on *Create Citation Report*
* Click on *Save to Excel File*
* Save in three batches (max export is 500 records)
* Save as a new file for working (keep original separate)
* Remove unnecessary columns; retaining the data points listed
  below 
* Save as a comma separated value (CSV) file using: tabs as the
  field delimiter; and quotations as the text delimiter
* Import CSV file into R Studio

**Data / variables saved**

* Article title
* Authors
* Source title
* Publication year
* Beginning page number
* Ending page number
* Total citations
* Average citations per year
* Citations for each year: 2009 - 2013

---

Sample size is 1,380 records.

See the word clouds for graphical representations, but the
following list includes the most frequent words in the article
titles:

word             | freq
-----------------|-----------------
knowledge        | 908
management       | 580
organizational   | 200
innovation       | 138
development      | 124
performance      | 109
learning         | 104
studies          | 99
share            | 95
systems          | 95
strategic        | 89
effects          | 82
model            | 82
practice         | 82
technology       | 81
case             | 80
use              | 78
social           | 77
relationship     | 76
firms            | 72  

---

There are 383 unique journal titles in the sample, but only
fourteen of those journals publish at least or close to 1.00% of
the articles on knowledge management and two journals dominated the
area:

Journal | Percentage
--------| -----------
JOURNAL OF KNOWLEDGE MANAGEMENT | 17.39%
KNOWLEDGE MANAGEMENT RESEARCH &amp; PRACTICE | 9.71%
AFRICAN JOURNAL OF BUSINESS MANAGEMENT | 2.97%
INTERNATIONAL JOURNAL OF TECHNOLOGY MANAGEMENT | 2.75%
MANAGEMENT DECISION | 2.03%
JOURNAL OF BUSINESS RESEARCH | 1.45%
TOTAL QUALITY MANAGEMENT &amp; BUSINESS EXCELLENCE | 1.23%
INDUSTRIAL MARKETING MANAGEMENT | 1.16%
SERVICE INDUSTRIES JOURNAL | 1.09%
INFORMATION &amp; MANAGEMENT | 1.09%
COMPUTERS IN HUMAN BEHAVIOR | 1.09%
INTERNATIONAL JOURNAL OF HUMAN RESOURCE MANAGEMENT | 1.01%
SYSTEMS RESEARCH &amp; BEHAVIORAL SCIENCE | 0.94%
ORGANIZATIONAL SCIENCE | 0.94%

---

There are 3,545 authors in the sample and there are an average of
about 2 authors per article (mdn = 2.00, m = 2.57). However, one
article has about 64 authors.

And just in case you were curious, each article is about 14 pages
long.

**Article title word clouds:**

The first word cloud is based on a minimum word frequency of 2
words.

![alt text](https://dl.dropboxusercontent.com/u/55752964/octopress/wp2.png "Word Cloud, KM, Freq 2")

The second word cloud is based on a minimum word frequency of 5
words.

![alt text](https://dl.dropboxusercontent.com/u/55752964/octopress/wp5.png "Word Cloud, KM, Freq 5")

The third word cloud is based on a minimum word frequency of 10
words.

![alt text](https://dl.dropboxusercontent.com/u/55752964/octopress/wp10.png "Word Cloud, KM, Freq 10")

The fourth word cloud is based on a minimum word frequency of 15
words.

![alt text](https://dl.dropboxusercontent.com/u/55752964/octopress/wp15.png "Word Cloud, KM, Freq 15")

Citations by year. Note how 2009 and 2010 are about the same,
except for the one highly cited article in 2009.

![alt text](https://dl.dropboxusercontent.com/u/55752964/octopress/citationsByYear.png "Citations by Year, 2009 - 2013")

---

## Code:

    # Read file
    wokRecKM <- read.delim("wokRecKM.csv")

    ## Load libraries

    library(tm)
    library(Snowball)
    library(RWeka)
    library(rJava)
    library(RWekajars)
    library(wordcloud)

    # Prepare word cloud
    myStopWords <- stopwords('english') kmArt <- Corpus(VectorSource(wokRecKM$Title))
    kmArt <- tm_map(kmArt, tolower)
    kmArt <- tm_map(kmArt, removeWords, myStopWords)
    kmArt <- tm_map(kmArt, removePunctuation)
    kmArt <- tm_map(kmArt, removeNumbers)

    dictCorpus <- kmArt

    kmArt <- tm_map(kmArt, stemDocument)
    kmArt <- tm_map(kmArt, stemCompletion, dictionary = dictCorpus)

    kmTDM <- TermDocumentMatrix(kmArt,
                                control = list(minWordLength = 1))

    mKM <- as.matrix(kmTDM)
    vKM <- sort(rowSums(mKM), decreasing = TRUE)

    namesKM <- names(vKM)

    dKM <- data.frame(word = namesKM,
                      freq = vKM)

    # Generate word clouds based on different minimum frequencies
    wordcloud(dKM$word, dKM$freq, min.freq = 2)
    wordcloud(dKM$word, dKM$freq, min.freq = 5)
    wordcloud(dKM$word, dKM$freq, min.freq = 10)
    wordcloud(dKM$word, dKM$freq, min.freq = 15)

    # Table of the 20 most frequent words in the article titles

    dKM[1:20,]

    # Count number of authors per article
    # Data imported automatically as factors;
    # convert to character first

    wokRecKM$Authors <- as.character(wokRecKM$Authors)

    # Each author's name is separated by a comma and is ordered as
    # last name, first name. Splitting each field by a comma creates
    # n strings + 1. So splitting and then subtracting by one
    # produces a count of the authors in the field, assuming data
    # in each field is regular.

    sum(sapply(strsplit(wokRecKM$Authors, ","), length) - 1)
    summary(sapply(strsplit(wokRecKM$Authors, ","), length) - 1)
   
    # Unique number of titles in the sample
    length(unique(wokRecKM$Source.Title))

    # Top Journals with at least 1% of knowlege management coverage:
    round(sort(table(wokRecKM$Source.Title) /
                            length(wokRecKM$Source.Title)) * 100, 2)

    # Page numbers; They imported as factors. So first convert
    # to character and then convert to numeric in order to retain
    # literal string
    wokRecKM$Beginning.Page <- as.character(wokRecKM$Beginning.Page)
    wokRecKM$Beginning.Page <- as.numeric(wokRecKM$Beginning.Page)
    wokRecKM$Ending.Page <- as.character(wokRecKM$Ending.Page)
    wokRecKM$Ending.Page <- as.numeric(wokRecKM$Ending.Page)

    # Descriptive statistics of page numbers
    summary(wokRecKM$Ending.Page - wokRecKM$Beginning.Page)

    # Citations Plots
    p <- ggplot(wokRecKM, aes(Publication.Year,
                              Total.Citations))
    p + geom_point() +
      xlab("Publication Year") +
      ylab("Total Citations")

    p <- ggplot(wokRecKM, aes(seq(1:1380), Total.Citations))
    p + geom_jitter() +
      xlab("Index") +
      ylab("Total Citations")
