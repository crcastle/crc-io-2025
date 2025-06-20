---
title: "Highlighting the Internet"
slug: highlighting-the-internet
date:   "2016-01-13"
---

I spend a large chunk of my day reading -- learning about the world, the people in it, and the ways in which we interact.  So many people, events, and discoveries only 15 years ago were hidden (from me) because they weren't easy to broadly, publicly share.  The internet and its [news.google.com][news] and [Twitter][twitter] and [Hacker News][hackernews] and [Medium][medium] and [Amazon][amazon] and a multitude of other sites and applications have made the world's information quickly and cheaply accessible to anyone with a connection to the internet.

#### Recall
As I was finding more and more to read, I grew frustrated by not being able to recall the important parts.  Remember back in school you used to underline and highlight and annotate the crap out of stuff you read for class?  I wanted that experience back.  Highlighting not only helps me better remember what I'm reading, it helps me distill and remember the important parts.

#### Highlighting Browser Text
But how do you highlight in a web browser on a laptop?  And what about on a phone?  Or a Kindle?

![Medium Highlight](medium-highlight.png)

Last year [Medium began allowing readers to highlight and save bits of text from blog posts][medium-highlights].  And the Kindle has always allowed readers to do this.  These two constitute a large part of my reading, but what about other websites?  Maybe a browser plugin?

#### Organizing My Highlights
Ok, now that I've got a solution to highlighting text on the internet, let's talk about how to collect and organize those highlights.  Why might I want to do that?  It started from an urge to better journal and reflect on my days, but there are lots of other reasons: posterity, manual review/lookup, statistical analysis, or even to power a section of this site!

I'm currently working on a set of simple [AWS Lambda][aws-lambda] functions to automatically collect all my highlights and write them to a Google Spreadsheet.  [Check out the spreadsheet][highlights-spreadsheet], fully populated from a set of AWS Lambda functions over the past few weeks.  The Google Spreadsheet acts as a database in this setup.  [Google Spreadsheets work very well as databases for simple applications like this][sheetsee-basics].

{{< image/with-background src=highlights-architecture.png alt="Architectural diagram" >}}

Here's a simple diagram of how all the pieces fit together.

Check out [Part 2][part2], where I share more details about how I built this and what I'm using it for!


[amazon]: http://www.amazon.com/Kindle-eBooks
[aws-lambda]: https://aws.amazon.com/lambda/
[hackernews]: https://news.ycombinator.com
[highlights-spreadsheet]: https://docs.google.com/spreadsheets/d/1VCkPCinhG-HiZJuvWPYq1wMIEpdHOdbCRF2lTDsOMBc
[medium]: https://medium.com
[medium-highlights]: https://medium.com/the-story/is-it-ok-to-highlight-your-own-stuff-fd3768dace9a#.djpp8o5z3
[news]: https://news.google.com
[part2]: /blog/2016/01/highlighting-the-internet-part-2/
[sheetsee-basics]: http://jlord.us/sheetsee.js/docs/basics.html
[twitter]: https://twitter.com