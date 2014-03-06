---
layout: post
title: "Basic Bibliometric Analysis of Knowledge Management Literature"
date: 2014-02-28 17:54
comments: true
categories: [knowledge management, bibliometrics, LIS658]
---

**Basic data collection process:**

Why basic? Because at the very least, in investigating any field
we'd want to generate data on something other than a simple
keyword search for a term, such as the search for *knowledge
management* in this example. Thus, there is likely a lot of hit
and miss in this search, such that a number of the articles may
have nothing to do with knowledge management as we study it in our
course. That said, the following might help build some intuition
about publication patterns in the field.

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

The R code and summarized output follow. First, load the CSV file.

    # Read file
    wokRecKM <- read.delim("wokRecKM.csv")

    names(wokRecKM) # List variable names (output not shown)
    length(wokRecKM) # List length of observations (sample size)

Sample size is 1,380 records.

Next, load the libraries needed for constructing a term document
matrix of the words in the article titles. We can then list the
most frequent words in the article titles and display the list
graphically. See the bottom of this post for the word clouds. Code
first and then some tabular results:

## Load text mining / word cloud libraries

    library(tm)
    library(Snowball)
    library(RWeka)
    library(rJava)
    library(RWekajars)
    library(wordcloud)

### Prepare term document matrix and word cloud

    # Prepare word cloud
    myStopWords <- stopwords('english')
    kmArt <- Corpus(VectorSource(wokRecKM$Title))
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
  
The top 20 most frequent words in the article titles. Note that
neither the terms *knowledge* nor *management* appear in all the
article titles.

| word             | freq |
| :----------------|------:|
| knowledge        | 908 |
| management       | 580 |
| organizational   | 200 |
| innovation       | 138 |
| development      | 124 |
| performance      | 109 |
| learning         | 104 |
| studies          | 99 |
| share            | 95 |
| systems          | 95 |
| strategic        | 89 |
| effects          | 82 |
| model            | 82 |
| practice         | 82 |
| technology       | 81 |
| case             | 80 |
| use              | 78 |
| social           | 77 |
| relationship     | 76 |
| firms            | 72 |

---

## Journal titles

    # Unique number of titles in the sample
    length(unique(wokRecKM$Source.Title))
    
    # Top Journals with at least 1% of knowlege management coverage:
    round(sort(table(wokRecKM$Source.Title) /
                            length(wokRecKM$Source.Title)) * 100, 2)


There are 383 unique journal titles in the sample, and fourteen of
those journals publish at least or close to 1.00% of the articles
on knowledge management. Two journals dominate the area:

| Journal | Percentage |
| :-------| ----------- :|
| JOURNAL OF KNOWLEDGE MANAGEMENT | 17.39% |
| KNOWLEDGE MANAGEMENT RESEARCH &amp; PRACTICE | 9.71% |
| AFRICAN JOURNAL OF BUSINESS MANAGEMENT | 2.97% | 
| INTERNATIONAL JOURNAL OF TECHNOLOGY MANAGEMENT | 2.75% |
| MANAGEMENT DECISION | 2.03% | 
| JOURNAL OF BUSINESS RESEARCH | 1.45% | 
| TOTAL QUALITY MANAGEMENT &amp; BUSINESS EXCELLENCE | 1.23% |
| INDUSTRIAL MARKETING MANAGEMENT | 1.16% |
| SERVICE INDUSTRIES JOURNAL | 1.09% | 
| INFORMATION &amp; MANAGEMENT | 1.09% |
| COMPUTERS IN HUMAN BEHAVIOR | 1.09% |
| INTERNATIONAL JOURNAL OF HUMAN RESOURCE MANAGEMENT | 1.01% |
| SYSTEMS RESEARCH &amp; BEHAVIORAL SCIENCE | 0.94% |
| ORGANIZATIONAL SCIENCE | 0.94% |

## Authorship

    # Count number of authors per article
    # Data imported automatically as factors;
    # convert to character first
    wokRecKM$Authors <- as.character(wokRecKM$Authors)
  
    # Each author's name is separated by a semicolon.
    # Splitting each field by a semicolon creates
    # exactly the number of fields as the number of authors. Where
    # there is only one author, and thus no semicolon, the string
    # split generates a count of one. See the documentation on
    # **strsplit** for an explanation.
    
    sum(sapply(strsplit(wokRecKM$Authors, ";"), length))
    summary(sapply(strsplit(wokRecKM$Authors, ";"), length))
    
    # Save Author count as its own variable
    wokRecKM$AuthorsCount <- sapply(strsplit(wokRecKM$Authors, ";"), length)
    
    # Produce table of Authors Count
    table(wokRecKM$AuthorsCount)

Most documents have less than three authors. In this table, for
example, 284 articles have one author, 461 articles have two
authors, and so forth.

| No. of Authors | Count of Documents |
| :------------| --------------------:|
| 1 | 284 |
| 2 | 461 |
| 3 | 421 |
| 4 | 143 |
| 5 | 46 |
| 6 | 8 |
| 7 | 7 |
| 8 | 3 |
| 9 | 1 |
| 11 | 2 |
| 20 | 1 |
| 31 | 1 |
| 32 | 1 |
| 64 | 1 |

    # Look at authors independently
    authors <- strsplit(wokRecKM$Authors, ";")
    authors <- unlist(authors)
    authors <- sub("^[[:space:]]", "", authors)
    authorsSorted <- sort(table(authors), decreasing = TRUE)
    library("arrayhelpers")
    authorsSorted <- array2df(authorsSorted, matrix = FALSE)
    names(authorsSorted) <- c("Count", "Authors")
    anyDuplicated(authorsSorted$Authors)
    
    # Note: It looks like there is at least one author that
    uses full first name under one article and initials for first
    part of name under the second article. See Adeline de Toit and
    A. S. A. de Toit. Will find a way to deal with this.
    
    # Associating authors with citation counts
    # Example of single author search
    wokRecKM$Total.Citations[grep("Bontis, Nick", wokRecKM$Authors)]
    sum(wokRecKM$Total.Citations[grep("Bontis", wokRecKM$Authors)])
    
    # Automating citation association with authors
    library("iterators")
    library("foreach")
    authorsSorted$Authors <- as.character(authorsSorted$Authors)
    il <- iter(authorsSorted$Authors)
    sum(wokRecKM$Total.Citations[grep(nextElem(il), wokRecKM$Authors)])
    # Save citations of individual authors in data set
    # First create function:
    auth <- function(auth) {
      sum(wokRecKM$Total.Citations[grep(nextElem(il), wokRecKM$Authors)])
    }
    # Then apply the function to each element in the author list
    authorsSorted$Citations <- sapply(authorsSorted$Authors,
                                      FUN = auth)
    rm(il) # Clear iteration list


There are 3,570 author entities attached to the articles in the
sample (there are less unique authors). There are an average of
about 2 authors per article (Mdn = 2.00, M = 2.59). See the table
above for details. 

There are 3,102 unique authors in the sample. The average
publication per author is about 1, but there are some prolific
authors.

Top ten most prolific authors, such that in this example, Nick
Bontis has published 10 articles and received 85 citations to
those articles.

    head(authorsSorted, n = 10)

| Count of articles | Authors | Total Citations |
| :-----------------|---------|----------------:|
| 10 | Bontis, Nick | 85 |
| 9 | Serenko, Alexander | 70 |
| 6 | Palacios-Marques, Daniel | 6 |
| 5 | Kianto, Aino | 15 |
| 5 | Lee, Gwo-Guang | 14 |
| 5 | Ordonez de Pablos, Patricia | 17 |
| 4 | Andreeva, Tatiana | 8 |
| 4 | Chai, Kah-Hin | 0 | 
| 4 | Chua, Alton Y. K. | 8 |
| 4 | Fang, Shih-Chieh | 8 |

Top ten most cited authors, such that, in this example, Stephen P.
Borgatti has published 1 article on knowledge management and has
received 268 citations for that one article.

    head(authorsSorted[order(-authorsSorted$Citations),], n = 10)

| Count of articles | Authors | Total Citations |
| :-----------------|---------|----------------:|
| 1 | Borgatti, Stephen P. | 268 |
| 1 | Brass, Daniel J. | 268 |
| 1 | Labianca, Giuseppe | 268 |
| 1 | Mehra, Ajay | 268 |
| 2 | Lichtenthaler, Eckhard | 90 |
| 2 | Lichtenthaler, Ulrich | 90 |
| 10 | Bontis, Nick | 85 |
| 3 | Huang, Jing-Wen | 76 |
| 1 | Noe, Raymond A. | 72 |
| 1 | Wang, Sheng | 72 |

## Publication pattern

There is some fluctuation in the number of KM articles published
per year, but there is no evidence to accept or reject the idea of
a trend. There could be a number of explanations for these
differences.

    table(wokRecKM$Publication.Year)

2009: 271 articles  
2010: 272 articles  
2011: 319 articles  
2012: 278 articles  
2013: 240 articles  

## Word clouds based on article titles

The first word cloud is based on a minimum word frequency of 2
words. (The term 'management' doesn't appear in the graph because
it would not fit.)

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

## Citations

Citations by year. Note how 2009 and 2010 are about the same,
except for the one highly cited article in 2009.

    # Citations Plots
    library("ggplot2")
    p <- ggplot(wokRecKM, aes(Publication.Year,
                              Total.Citations))
    p + geom_point() +
      xlab("Publication Year") +
      ylab("Total Citations")

![alt text](https://dl.dropboxusercontent.com/u/55752964/octopress/citationsByYear.png "Citations by Year, 2009 - 2013")
