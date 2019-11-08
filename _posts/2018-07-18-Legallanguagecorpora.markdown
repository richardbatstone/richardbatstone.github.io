---
layout: post
title:  "Legal language corpora"
date:   2018-07-18 08:02:25 +0000
categories: Data, Machine Learning, Legal Language
---

I’ve recently been looking for corpuses of “legal” language to use as a starting point for machine learning applications. I have a few specific tasks in mind but to start with I’m just interested in seeing what corpuses already exist and the tasks they’ve been used to support. Given all the excitement about “legal tech” I’d assumed there would be a few well established corpuses that are regularly used for legal language modelling etc., so I was surprised by the lack of ready-made corpuses. To that end, I thought I would write about the different types of legal language, some sources of publically available legal text and how you might go about turning these into usable corpora.

### What is legal language?

When I think about legal language, I think about three broad categories of language reflecting different types of communication:

1. Legal rules: First, there’s the type of language used to express legal rules. Legal rules might be set out in legislation, or they might be contractual terms agreed between private persons. Things like:

> This section applies where: (a) a company makes a distribution consisting of or including, or treated as arising in consequence of, the sale, transfer or other disposition by the company of a non-cash asset, and (b) any part of the amount at which that asset is stated in the relevant accounts represents an unrealised profit.
(Section 846 Companies Act 2006)

2. Legal discourse: Second, there’s the type of language used by judges and advocates in courts and other tribunals. You could probably break this down further to distinguish between the oral submissions made in courts / tribunals and the written judgements produced by the courts. The latter includes things like:

> The main issue on this appeal concerns the ability of ministers to bring about changes in domestic law by exercising their powers at the international level, and it arises from two features of the United Kingdom’s constitutional arrangements. The first is that ministers generally enjoy a power freely to enter into and to terminate treaties without recourse to Parliament. This prerogative power is said by the Secretary of State for Exiting the European Union to include the right to withdraw from the treaties which govern UK membership of the European Union (“the EU Treaties”). The second feature is that ministers are not normally entitled to exercise any power they might otherwise have if it results in a change in UK domestic law, unless statute, ie an Act of Parliament, so provides. 
(Paragraph 5, R (on the application of Miller) v Secretary of State for Existing the European Union (2017) UKSC 5)

3. Legal advice: Third, there’s the type of language used by legal professionals in giving advice to their clients. Things like:

> It is because title to a lease is for a limited period that the owner of the building, the Landlord or Freeholder, is concerned to see that when he regains possession at the end of the term the property is in a good state of repair and condition. 
(Advice received on purchasing my leasehold...)

I think it is fairly self-evident that these three categories reflect different types of language, though it is interesting to consider exactly *how* they differ, and the *extent* of that difference. I think it’s also clear that legal language differs from other language domains. Intuitively, much of legal language (especially legal rules) has a lengthier and more complex structure than ordinary language. It also tends to use words in different senses to their more common meanings: in legislation, the words “bank” and “company” seldom refer to the sides of rivers or the presence of friends.

### What legal corpuses already exist?

I didn’t come across any ready-made corpuses of legal rules or legal advice (although there are accessible sources of the former, see further below). The [British National Corpus](http://www.natcorp.ox.ac.uk/) includes some language from court and parliamentary proceedings, though this is spoken language and is relatively limited in size.

#### UK Legislation

A huge amount of legislation is available online from the [National Archives](http://www.legislation.gov.uk/). Whilst not a ready-made corpus that you can start processing, this is a great resource. Many pieces of legislation are available as XML tagged documents, allowing reasonably straightforward processing.

I wrote a short script (see "leg_extract.ipynb" in my [Legal Corpora Repo](https://github.com/richardbatstone/Legal_copora)) to extract whole sentences and sentence fragments from legislation and write them to a CSV for onward processing. The repo also contains a sample CSV, containing language extracted from 21 pieces of legislation. It comprises about 1.17 million words, with a total vocabulary of around 7,400 (which is much smaller than you might expect from other language domains with a sample of this size). See the notes in the script for an explanation of the labelling scheme and some nuances around the extraction. The 21 pieces of legislation were chosen, roughly, to reflect: (i) “popular” legislation (as reported by the National Archives); (ii) legislation, over a certain period, which was subject to substantive debate by the whole House of Commons (following this [report](http://researchbriefings.parliament.uk/ResearchBriefing/Summary/SN05435), being a proxy for controversial legislation); and (iii) legislation that I use regularly in my work.

I wasn’t able to scrape the National Archives domain (i.e. scrape requests returned 403 errors). I’m not sure if this is intentional on their part, or if there’s just a trick that I’m missing in my spider’s script. Fortunately, you can build a fairly large corpus from relatively few legislative documents, so it is not too much work to manually download the XMLs. All data is made available under the [Open Government License](http://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/), so be sure to cite the National Archives as the source of the data if you do use it.

#### Contracts

Generally, contracts are private documents, so it’s understandable that they are slightly harder to collect in bulk. [Michael Curtotti’s thesis](https://openresearch-repository.anu.edu.au/handle/1885/110522) (which is generally great for helping frame thinking about the communication of law) suggests just doing Google searches for word documents with certain search terms (e.g. “contract” and “party”). As Curtotti notes, there are a number of drawbacks to this approach – you tend to get contract templates and only in a few categories. Moreover, it’s a pain to clean up those word documents and get the text in a form that’s ready for processing.

There are a couple of other places you can look to for publically available contracts. If you don’t mind your contracts coming from the corporate/finance space, then you can look to the contractual terms set out in documents disclosed by listed companies. For example, you could look to the terms and conditions set out in bond prospectuses. To collect these in volume you would likely want to scrape a repository of RNS announcements (e.g. through the [London Stock Exchange](https://www.londonstockexchange.com/exchange/news/market-news/market-news-home.html) and/or [Morninstar](http://www.morningstar.co.uk/uk/)). However, these too seem limited in scope (there’s not that much variation in the terms of bonds).

A better source, I think, are the “material contracts” which companies with US securities are required to file with the SEC. These capture a broader range of contracts, albeit having a US bias. You can scrape the [SEC’s EDGAR database](https://www.sec.gov/edgar/searchedgar/companysearch.html), though you cannot directly filter the database for material contracts, so you need to do a little tuning to get the right results. However, there is also a fantastic resource available from [Law Insider](https://www.lawinsider.com/). Not only have these guys already extracted the material contracts, they’ve also got tools to search for particular types of agreements / clauses.

I built a scraper (see "LIscrape.py") to extract the text from the first 10 clauses listed on a “sub-snippet” page (like [this](https://www.lawinsider.com/clause/notices/_1)) and write them as lines of a CSV. At this point, I’m not interested in the change analysis that has been carried out between the different example clauses, so I just extract the plain text. But I can imagine that would be useful for lots of other applications. The domain URLs are pretty sensibly organised so it’s possible to be quite targeted in the types of clause that you want to extract. As a starting point, I just compiled a list of the URLs I wanted to scrape (being all clause types with more than 25k examples, sampling the first 11 “sub-snippet pages” for each type). The resulting CSV is in my [Legal Corpora Repo](https://github.com/richardbatstone/Legal_copora). It comprises about 740k words with a vocab of about 7k.

The licensing arrangements around the use of SEC data (and Law Insider’s cataloguing of that data) are a little less straightforward, so you should think carefully about what’s permissible if you plan to make non-personal use of the data.

#### Other sources

Many court judgements are also easily available from [BAILII](http://www.bailii.org/), and the proceedings of parliamentary committees are available through [https://www.parliament.uk/business/committees/](https://www.parliament.uk/business/committees/). The court judgements look like they should be fairly easy to scrape (do get in touch if you have done so!) but, anecdotally, the committee minutes are a little harder. I am not aware of any freely accessible sources of professional legal advice…

If anyone is aware of other sources or if you have any questions on the data/code, please do let me know!