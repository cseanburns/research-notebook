---
layout: post
title: "Basic Bibliometric Analysis of Knowledge Management Literature Part II"
date: 2015-02-18 15:46:54 -0500
comments: true
categories: [bibliometrics, knowledge management, lis658]
---

It is nice to have a general lay of the land when doing literature searches.
To help with this, what follows is a basic overview of the knowledge
management literature. This is a non-robust (based on a simple search using
the keywords "knowledge management" in *Web of Science*) overview, and
covers only a five year publication window (2010-2014). See [last year's
post on this topic for a basic sketch of the retrieval process and the code
used for the basic analysis][1]

Sample size is 1,862 records.

## Basic Text Mining

Here are the top 20 most frequent words that appear in the article titles.
Note that neither the terms *knowledge* nor *management* appear in all the
article titles (otherwise *Freq* would equal the sample size of 1,862). Also
note that less than half might be helpful either as key terms in database
searches or in helping build an intuition about the field. This is because
most of the terms are generically related to words commonly found in
research studies: e.g., model, research, development, analysis, study, etc.
Those terms may be helpful in narrowing down empirical search, however.

| Word           | Freq  |
| :--------------|------:|
| knowledge      | 1,058 |
| management     | 587   |
| performance    | 153   |
| organizational | 145   |
| learning       | 137   |
| sharing        | 127   |
| information    | 121   |
| innovation     | 118   |
| social         | 118   |
| analysis       | 97    |
| development    | 97    |
| systems        | 96    |
| role           | 89    |
| research       | 86    |
| model          | 83    |
| perspective    | 83    |
| approach       | 79    |
| study          | 77    |
| practice       | 69    |
| case           | 62    |

## Journal titles

There are 460 unique journal titles in the sample (77 more titles than
appeared [last year][1]). Last year, two journal titles accounted for over
26% of the coverage, but this year the coverage is a bit more diffuse. Here
are the journal titles that published at least 1% of the material on
knowledge management:

| Journal Title                                                          | Percentage |
| :----------------------------------------------------------------------|-----------:|
| Journal of Knowledge Management                                        | 11.87%     |
| Knowledge Management Research &amp; Practice                           | 6.61%      |
| Expert Systems with Applications                                       | 2.31%      |
| African Journal of Business Management                                 | 2.15%      |
| International Journal of Information Management                        | 1.88%      |
| Management Decision                                                    | 1.45%      |
| Journal of Business Research                                           | 1.24%      |
| Industrial Management &amp; Data Systems                               | 1.24%      |
| Computers in Human Behavior                                            | 1.24%      |
| International Journal of Technology Management                         | 1.18%      |
| Decision Support Systems                                               | 1.18%      |
| International Journal of Project Management                            | 1.13%      |
| Behavior &amp; Information Technology                                  | 1.02%      |
| Journal of the American Society for Information Science and Technology | 1.02%      |

## Authorship

Based on the database search, in the last five years 4,984 authors have
published 1,862 papers related to knowledge management. This represents a
mean of 2.7 authors per paper and a median of 2.0 authors per paper. Most
papers have been authored by four or fewer authors:

* 337 papers have been authored by one person
* 598 papers have been authored by two people
* 563 papers have been authored by three people
* 241 papers have been authored by four people

The most prolific authors in this sample (arbitrarily set to those authors
that have published more than 5 articles on knowledge management) follow.
Citation counts are limited to the sample and are not total citation counts
for all articles or works the author has published:

| Article Count | Authors                  | Total Citations     |
| :------------ |--------------------------|--------------------:|
| 10            | Bontis, Nick             | 67                  |
| 10            | Serenko, Alexander       | 71                  |
| 8             | Palacios-Marques, Daniel | 12                  | 
| 7             | Cheung, C. F.            | 6                   |
| 7             | Colomo-Palacios, Ricardo | 36                  |
| 7             | Lin, Binshan             | 23                  |
| 7             | Middleton, Blackford     | 67                  |
| 6             | Jafari, Mostafa          | 42                  |
| 6             | Lin, Chinho              | 17                  |
| 6             | Lin, Hsiu-Fen            | 21                  |
| 6             | Ooi, Keng-Boon           | 17                  |
| 6             | Wright, Adam             | 64                  |
| 6             | Yang, Jie                | 22                  |


The ten most cited authors in the sample. Interestingly, this list hardly
agrees with [last year's list][1], which means the shift in publication time
frame had a substantial effect.

| Article Count | Authors            | Total Citations |
|:--------------|--------------------|----------------:|
| 2             | Noe, Raymond A.    | 119             |
| 2             | Wang, Sheng        | 119             |
| 4             | Tseng, Ming-Lang   | 112             |
| 1             | Foss, Nicolai J.   | 102             |
| 1             | Lyles, Marjorie A. | 102             |
| 1             | Volberda, Heng W.  | 102             |
| 3             | Leonardi, Paul M.  | 86              |
| 1             | Xu, Li Da          | 84              |
| 2             | Lavie, Dovev       | 79              |
| 1             | Stettner, Uriel    | 72              |

## Publication Pattern

Based on the same search, there is a difference in the publication patterns
between [last year's list][1] and this year's list. This likely reflects
differences in the *Web of Science* database between last year and this year
(these kinds of changes really screw with the validity of bibliometric
research, btw).

* 2010: 375 articles
* 2011: 409 articles
* 2012: 384 articles
* 2013: 350 articles
* 2014: 344 articles

## Word cloud based on article titles

Unlike last year, I'm only including one word cloud in this post. The
frequency list at the top of this post is much more revealing, but this word
cloud is based on a minimum of 15 words.

![alt text](https://dl.dropboxusercontent.com/u/55752964/octopress/km-wc-15words-min.png "Word Cloud, KM, Freq 15")

## Citations

Citations by year. The citation counts are lower than [last year's][1],
which is to be expected. The same expected pattern from last year holds true
for this year.

![alt text](https://dl.dropboxusercontent.com/u/55752964/octopress/citationsByYear2015.png "Citations by Year, 2010 - 2014")

Also as expected, there are fewer higher impact articles and lots of lower
impact articles, as measured by citation counts.

![alt text](https://dl.dropboxusercontent.com/u/55752964/octopress/citation-long-tail-2015.png "Citation Long Tail, 2010 - 2014")

---

Code and data collection process used here is the same as last year's. See
[that post][1].

[1]: /blog/2014/02/28/basic-bibliometric-analysis-of-knowledge-management-literature/
