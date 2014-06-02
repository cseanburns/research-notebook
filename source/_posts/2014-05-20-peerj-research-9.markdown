---
layout: post
title: "PeerJ Research 9"
date: 2014-05-20 11:56:51 -0400
comments: true
categories: [peerj, research]
---

This is the ninth set of notes on a research project about peer
review. To access the collection of notes and log entries, see:

[Category: PeerJ](/blog/categories/peerj)

---

On [August 20, 2013](/blog/2013/08/20/peerj-research-2/) I
collected a round of data based on a sample of random numbers
generated on [July 20, 2013](/blog/2013/07/20/peerj-research/).

On [November 20, 2013](/blog/2013/11/20/peerj-research-7/) I
collected a follow up round of data plus some additional data
points.

On [February 20, 2014](/blog/2013/11/20/peerj-research-8/) , I
collected an additional round of data and added some additional
data points.

Today, on May 20, 2014, I collected the final round of data. One
additional data point was added.

Currently, the data is stored in a LibreOffice Calc spreadsheet
file to be exported to a CSV file and imported into R Studio.

I have the following variables:

### Document Variables

* **RandomNum**: The random number originally generated.
  Corresponds to the DOI assigned to *PeerJ*.
* **doi**: The DOI assigned to *PeerJ*. Corresponds to the random
  number. *PeerJ*'s DOIs are easily identified as sequential.
* **RecdDate**: Recorded date the manuscript was received by
  *PeerJ*.
* **AcceptDate**: Recorded date the manuscript was accepted by
  *PeerJ*.
* **PubDate**: Recorded date of publication.
* **PeerRevAvail**: Dichotomous yes / no variable indicating
  whether the peer review history the article in the sample is
  publicly available.
* **PubMed**: Dichotomous yes / no variable indicating whether the
  manuscript is indexed in PubMed.
* **AuthCount**: Count of authors of a manuscript.
* **MultiNational**: Dichotomous yes / no variable indicating
  whether authors are from more than one nation, as indicated by
  their recorded address on the published article.

### Review Variables

* **NoReviewers**: Number of reviewers per article. For those
  articles that have a publicly available peer review history, the
  value for the variable is numerical. For those articles that do
  not have a publicly available peer review history, the value is
  *na*.
* **PeerAnon**: Categorical variable with values from 0 to 2. A
  value of 0 indicates all referee identities are anonymous. A
  value of 1 indicates all referee identities are public. A value
  of 2 indicates a mix of anonymous and public referee identities.
  A value of *na* indicates no information available and is
  assigned to those articles without a publicly available peer
  review history.
* **NoRevisions**: Numerical variable indicating the number of
  revisions the manuscript went through before being accepted. The
  higher the number, the more revisions. The range is *0-2*.
* **RevStanding-1**: Categorical variable indicating whether the
  first revision was accepted after no revisions, minor revisions,
  or major revisions. Values include: None, Minor, Major, and
  *na*.
* **RevStanding-2**: Same as RevStanding-1 except the values
  indicate category of acceptance after the second revision of the
  manuscript. The range of data is either Minor or *na*.

The following two variables are held in separate CSV files.

* **R1.1**: Word count of the first review by the first referee
* **R2.1**: Word count of the first review by the second referee

### Use Variables

* **UniqueVisitors-Aug**: The count of unique visitors to an
  article's page, as provided by *PeerJ*, and as recorded on
  August 20, 2013.
* **UniqueVisitors-Nov**: Same as above and as recorded on
  November 20, 2013.
* **UniqueVisitors-Feb**: Same as above and as recorded on
  February 20, 2014.
* **UniqueVisitors-May**: Same as above and as recorded on
  May 20, 2014.

---

* **Pageviews-Aug**: The count of unique page views to an
  article's page, as provided by *PeerJ*, and as recorded on
  August 20, 2013.
* **Pageviews-Nov**: Same as above and as recorded on November 20,
  2013.
* **Pageviews-Feb**: Same as above and as recorded on February 20,
  2014.
* **Pageviews-May**: Same as above and as recorded on May 20,
  2014.

---

* **SocRefUniq-Aug**: The unique count of social referrals to an
  article's page, as provided by *PeerJ*, and as recorded on
  August 20, 2013.
* **SocRefUniq-Nov**: Same as above and as recorded on November
  20, 2013.
* **SocRefUniq-Feb**: Same as above and as recorded on February
  20, 2014.
* **SocRefUniq-May**: Same as above and as recorded on May 20,
  2014.

---

* **SocRefTotal-Aug**: The total number of referrals by all social
  referrals to an article's page, as provided by *PeerJ*, and as
  recored on August 20, 2013.
* **SocRefTotal-Nov**: Same as above and as recorded on November
  20, 2013.
* **SocRefTotal-Feb**: Same as above and as recorded on February
  20, 2014.
* **SocRefTotal-May**: Same as above and as recorded on May 20,
  2014.

---


* **TopRefUniq-Aug**: The unique count of web referrals to an
  article's page, as provided by *PeerJ*, and as recorded on
  August 20, 2013.
* **TopRefUniq-Nov**: Same as above and as recorded on November
  20, 2013.
* **TopRefUniq-Feb**: Same as above and as recorded on February
  20, 2014.
* **TopRefUniq-May**: Same as above and as recorded on May 20,
  2014.

---

* **TopRefTotal-Aug**: The total number of referrals by all web
  referrals to an article's page, as provided by *PeerJ*, and as
  recorded on August 20, 2013.
* **TopRefTotal-Nov**: Same as above and as recorded on November
  20, 2013.
* **TopRefTotal-Feb**: Same as above and as recorded on February
  20, 2014.
* **TopRefTotal-May**: Same as above and as recorded on May 20,
  2014.

---

Data points added on February 20, 2014:

* **Promoted**: Dichotomous variable indicating whether the
  article was promoted as part of the [PeerJ Picks 2014
  Collection][1] or, as of May 20, 2013, was promoted as part of
  the [The Line Islands Collection][2].
* **Correction-Minor**: Categorical variable indicating whether
  there is a post-publication correction to the article. At this
  point, the categories are: **minor**.
* **Scopus_Feb**: Citation counts from Scopus.
* **GS_Feb**: Citation counts from Google Scholar. Note that
  article 10.7717/peerj.22 was not discoverable from Google
  Scholar and required a title search in Google Web in order to
  retrieve the citation count.

---

Data points added on May 20, 2014:

* **Scopus_May**: Citation counts from Scopus.
* **GS_May**: Citation counts from Google Scholar.
* **Downloads-May**: PeerJ now displays on article pages the
  number of times article was downloaded. This new data point was
  added.

Wayback Machine confirms article download information was added in
the last three months. E.g.:

[http://web.archive.org/web/20131026113907/https://peerj.com/articles/107/](http://web.archive.org/web/20131026113907/https://peerj.com/articles/107/)

[1]: https://peerj.com/collections/5-peerjpicks/
[2]: https://peerj.com/collections/1-line-islands/
