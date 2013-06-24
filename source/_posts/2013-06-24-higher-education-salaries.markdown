---
layout: post
title: "Higher Education Salaries"
date: 2013-06-24 07:56
comments: true
categories: [higher education, scholarly communication]
---

## Salaries: Administration versus Faculty

Outline of this post:

1. Extracting data from a PDF file
2. Cleaning data
3. Quick overview of data

*Note*: Code beginning with a dollar sign ($) signifies that the
command was carried out in a Linux terminal / shell (BASH).
Otherwise, code was carried out in the text editor [Vim][1].

[1]: http://www.vim.org/

Purpose: The purpose of this post is to begin outlining how to
examine higher education salaries --- especially salaries paid to
administration, faculty, and adjuncts. The eventual goal is to
compare these salaries against each other (in a later analysis
related to scholarly communication issues). Since this post
outlines how to do this with salaries at the University of
Missouri and is based on the University of Missouri personnel
information provided by the Missouri Secretary of State's office,
the code and process below cannot necessarily be generalized to
other universities. Each state most likely provides its public
information in different formats, although I haven't examined
this. This post primarily serves as a note to self. Most of what
is done in Vim below may eventually be automated with sed and / or
awk. But this is an exploration and Vim provides a nice interface
for this. 

Start by downloading the PDF document with University of Missouri
personnel information for the 2011 - 2012 year, the latest
available data at the time of this writing.

    $ wget http://www.sos.mo.gov/BlueBook/2011-2012/10_Personnel.pdf#personnel

This document contains name, position, and salary information.
Although it is organized as Last Name, First Name, Position, and
Salary, the fields are not so neat. Some persons have more than
one position or title and some individual names are more than two
words long. So this file will require some cleaning before it can
be analyzed. Also, I will eventually remove the first and last
names from the file as they are not needed.

First, convert the downloaded PDF to a text file. In this file,
the University of Missouri information begins on page 87 and ends
on page 151, and the Linux command *pdftotext* can cut that
specific part out:

    $ pdftotext -f 87 -l 151 10_Personnel.pdf personnel.txt

The MU information begins somewhere around the middle of the left
column on the first page and ends partway through the last page. I
delete the personnel information from the other institutions that
are carried over on these two pages so just MU information is
contained in this file. Also, I delete the section titles that
mark the beginning of the MU information section and are now at
the top of the document (i.e., the text is: University of
Missouri, Columbia 65211).

The pdftotext command makes a good attempt at making a 1 to 1 copy
of the PDF file (but fortunately, it collapses the multi-column
PDF into a single column text file) and so the converted text file
has page numbers and page headers in it. These are on their own
lines. The page numbers begin with a ^L (control L character). In
Vim, remove these control characters and page numbers with the
following code. Note that I start the substitution with a caret ^
key, which indicates the beginning of a line, and then press
*CONTROL-V CONTROL-L* in order to enter the *^L* character in the
substitution string.

    :%s/^^V^L[0-9][0-9][0-9]//g

Salaries are in the last column, but some of these salaries are
carried over to the next line and sit by themselves. They all
begin with a dollar sign. Search for these, by escaping the dollar
sign, and back them up on the line they belong to. There may be a
way to automate this, but right now, as I develop an intuition
about this file and its structure, I do this manually. The Vim
search string for locating these lines is:

    /^\$

The header lines from the PDF need to be removed. There are two of
them and the Vim code to remove them is:

    :%s/^UNIVERSITY OF MISSOURI//g
    :%s/^OFFICIAL MANUAL//g

Next I remove all empty lines:

    :g/^$/d

Some lines are still carried over and they all seem to begin with
a lower case letter. Search for all lines beginning with a lower
case letter. This search and replace has to be done manually
because I'm not yet sure if there's any regularity to this. For
example, some last names begin with a lower case letter, such as
those beginning with *van*. But I'm not sure if all these lines
indicate a last name that begins with a lower case letter. Nor do
I know how to write the kind of code that could automate *backing
up a line*. In any case, the Vim code below, the *^\l* searches
for all lines that begin with a lower case letter.

    /^\l

The file is now cleaned. I save it as a master copy and work with
a new copy that I save as a comma separated value file (CSV).

    $ cp personnel.txt personnel.csv

There's still some cleaning up to do. The end game is to have a
two column file with Positions in the first column and Salaries in
the second column, and so I need to discard the first and last
name columns. I do this in a roundabout way.

The first step is to remove all commas so that each field is
separated only by a space:

    :%s/,//g

And then remove all dollar signs from the salaries:

    :%s/\$//g

Then I replace the last empty space, just before the salary, with
commas:

    %s/.*\zs /,/

And to mark the first field containing the last name, I replace
the first occurrence of a space (which is just after the last
name) with a tab:

    :%s/ /\t/

Now it's easy to remove the first field with the last name since
it's marked by a tab while the other fields are still separated by
a space. I remove the tab delimited field containing the last
name:

    :%s/^[^\t]*\t//

And I repeat these last two commands in order to remove the field
with the first names, which has been moved to the first field. So,
repeating the above code, I replace the first occurrence of a
space with a tab and remove the new first column:

    :%s/ /\t/
    :%s/^[^\t]*\t//

Finally, I add quotes around the Position field, which is a text
field. This isn't strictly necessary, but it helps to mark it:

    %s/^/"/
    %s/,/",/

Now, all that is left is a comma delimited file with Position in
the first column and Salary in the second column. I add a header for
this file (Position, Salary) and import into R for analysis.

Note: After I imported the file into RStudio, I found that the
Salary column was recognized as a factor data type instead of a
numeric data type. Based on previous experience, I knew this meant
something was wrong with this column. When I converted it to a
numeric data type, R notified me that the coercion resulted in
some NAs. I located one NA at line 5900 and opened the CSV file in
Vim and found that it was a line that carried over to the next. It
seems I had missed one line from the cleaning process. I fixed it
and re-imported the file in R, which now recognized it as a
numeric data type. And the analysis can begin.

The analysis won't be straightforward until I can determine if
there's any regularity to how higher administration titles are
named or what all the administration titles are. But it's trivial
to get some quick information from the data.

First, I can see how many people are employed by MU:

    length(personnel$Position)
    [1] 18894

So 18,894 people were employed by MU during the 2011 - 2012
academic year (there will certainly be some error with this -- as
I don't know how people may be excluded).

How many position titles were there?

    length(unique(personnel$Position))
    [1] 3126

There were 3,126 unique positions.

The average number of and salary for assistant, associate, and
full professors:

    length(personnel$Salary[personnel$Position == "prof asst"])
    summary(personnel$Salary[personnel$Position == "prof asst"])
    length(personnel$Salary[personnel$Position == "prof assoc"])
    summary(personnel$Salary[personnel$Position == "prof assoc"])
    length(personnel$Salary[personnel$Position == "professor"])
    summary(personnel$Salary[personnel$Position == "professor"])

The number, median and mean salaries, respectively, for:

| *Title*              | *n* | *Median* | *Mean*    |
|----------------------|----:|---------:|----------:|
| Assistant professors | 535 | $65,420  | $76,050   |
| Associate professors | 787 | $76,170  | $82,540   |
| Professors           | 754 | $108,900 | $123,200  |


More exploratory analysis will come later. I'll have to figure out
a way to identify and categorize administrative positions. This
will most likely involve creating a database with these title
names.
