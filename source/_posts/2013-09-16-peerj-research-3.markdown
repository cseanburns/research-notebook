---
layout: post
title: "PeerJ Research 3"
date: 2013-09-16 15:03
comments: true
categories: [peerj, research]
---

This is the third set of notes on a research project about peer
review. To access the collection of notes and log entries, see:

[Category: PeerJ](/blog/categories/peerj)

---

## File Handling and Conversion

The downloaded and retrieved PeerJ files existed in four formats:
txt, doc, docx, and pdf. This collection of sampled files are
archived so that I will have access to the originals in case
needed. The files are copied to a new directory and converted to
.txt files for analysis.

I will import these files into R separately, but I want to take a
quick look at the whole thing. So I create one file out of all
those text files (a total of 203 files) and do so by outputing to
a single file in a separate temporary directory:

    for i in *txt ; do cat $i >> $HOME/tmp/oneFile.txt ; done

Since the files are numbered 01 through 107, this saves the files
out of order (e.g., 99 comes after 107 in the file directory), but
since this is just a quick peek, it is not a problem at the
moment.

## Importing and Peeking

There are a lot of text mining and R examples on the web. This is
not the point of my project, but I using one nice
[example](http://www.exegetic.biz/blog/2013/09/text-mining-the-complete-works-of-william-shakespeare/)
to take a quick look at what I have:

    pj <- readLines("$HOME/tmp/oneFile.txt")
    head(pj)
    tail(pj)
    library(tm)
    library(SnowballC)
    doc.vec <- VectorSource(pj)
    doc.corpus <- Corpus(doc.vec)
    summary(doc.corpus)
    doc.corpus <- tm_map(doc.corpus, tolower)
    doc.corpus <- tm_map(doc.corpus, removePunctuation)
    doc.corpus <- tm_map(doc.corpus, removeNumbers)
    doc.corpus <- tm_map(doc.corpus, removeWords,
    stopwords("english"))
    doc.corpus <- tm_map(doc.corpus, stemDocument)
    doc.corpus <- tm_map(doc.corpus, stripWhitespace)
    inspect(doc.corpus[1])
    inspect(doc.corpus[2])
    TDM <- TermDocumentMatrix(doc.corpus)
    inspect(TDM[1:10,1:10])
    DTM <- DocumentTermMatrix(doc.corpus)
    inspect(DTM[1:10,1:10])
    findFreqTerms(TDM, 100)
    findAssocs(TDM, "discuss", 0.2)
    findAssocs(TDM, "cite", 0.2)
