---
layout: post
title:  "Brushing up on explanations"
date:   2018-03-17 12:02:25 +0000
categories: GDPR
---

![Header](https://richardbatstone.github.io/images/DD_002.PNG)

Next week, I am off to the [Alan Turing Institute](https://www.turing.ac.uk/) for a workshop titled *"The GDPR and beyond: Privacy, Transparency and the Law"* and, in preparation, I am catching up on my GDPR reading. I am a little late to the GDPR gravy train. The professional services space is choking with thought pieces, implementation guides and bespoke training (all of which you *must* buy / read / share if you are to have any hope of avoiding criminal charges for data related offences come May). Almost every major corporate I've worked with has a GDPR compliance programme. Nevertheless, there are some knotty (and interesting) issues in the GDPR and this seems like a good opportunity to catch up with current thinking, and add my two cents.

In particular, I am keen to see what people have made of the "right to explanation", a collection of provisions and recitals in the GDPR which set some boundaries around how "automated decisions" may be made about individuals. I've been surprised that there still seems to be so much disagreement about the scope of what the GDPR provides, much less about what you need to do in practice. In this blog, I'm going to pick on a couple of papers on the topic that I've found interesting.

### A few words on "explanation" in the GDPR

A description of the various provisions in the GDPR which contribute to the right (or "so called right", depending on your persuasion) to explanation is worthy of its own blog. However, for those who are unfamiliar with the GDPR, here are the basics.

There's no single provision in the GDPR which sets out an individual's rights in relation to automated decision processes. Instead, there's a suit of provisions around:

1. information that organisations need to give individuals when they collect their data;
2. information that organisations must provide individuals on request; and
3. an individual's right not to be subject to automated decision making, unless certain conditions have been met.

The provisions making up points 1 and 2 talk about individuals being provided with (where the organisation intends to use automated decision making):

> *"...meaningful information about the logic involved, as well as the significance and the envisaged consequences of such data processing for the data subject."*

The provision covering point 3. requires that:

> *"The data subject shall have the right not to be subject to a decision based solely on automated processing, including profiling, which produces legal effects concerning him or her or similarly significantly affects him or her"*

There are some significant carve-outs to this right, including where the individual has explicitly consented to the processing. However, in that case, the organisation must put safeguards in place to protect the individual's rights and freedoms and including:

> *"...at least the right to obtain human intervention on the part of the data controller, to express his or her point of view and to contest the decision."*

If you're reading these provisions for the first time, I expect you have some follow up questions. You should take comfort from the fact that, in this regard at least, you are not alone. The key issues in the debate are:

1. What even is automated decision making? How much human intervention would there need to be, say, in an online credit card application for the decision to be considered non-automated? Which decisions produce "legal effects"? The level of insurance premiums? My Netflix recommendations?
2. Assuming there is some automated processing, what is "meaningful information about the logic involved"? Is that a description of the architecture of my ML algorithm? Or something more detailed, and interpretable?
3. How and when should the information be given to the individual? How meaningful is information about an ML algorithm outside the context of a decision?

Much of the academic debate is grounded in two opposing papers. The [first](https://arxiv.org/abs/1606.08813), by Goodman and Flaxman arguing that the GDPR gives rise to a strong "right to explanation", but with little detail on where this right is located in the text or how it should work. The [second paper](https://academic.oup.com/idpl/article/7/2/76/3860948), provocatively named *"Why a Right to Explanation of Automated Decision-Making Does Not Exist in the General Data Protection Regulation"* comes from Wachter, Mittelstadt and Russell. It argues, unsurprisingly, that there is no "right to explanation" in the GDPR in the sense that many people believe. 

### Explanation through counterfactuals

*["Counterfactual explanations without opening the black box: automated decisions and the GDPR"](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3063289)* offers a neat perspective on explanations of decisions made by complex ML algorithms. I quite like this paper. It's another from Wachter, Mittelstadt and Russell, but this time they put forward a positive case of what a good explanation might look like. There's still a technical debate about the support for their proposals in the GDPR, but there's also some intuitive, practical suggestions.

The basic idea goes like this. If I am trying to understand how a car insurer decides whether to offer me cover or not, a description of their ML architecture is not going to help me very much. It's not helpful to know that they're using a classifier to group applicants into 20 different classes based on a list of 500 features about the applicant. It's probably not even that helpful to be told what the features *are*. After all, there are so many of them and, for many, I may not have an intuitive sense of what counts in my favour and what counts against me. Maybe if the organisation could single out the top 5 features that are most important in coming to any decision (e.g. I guess, things like age of driver, number of years with no claims, price of car etc.) that would be helpful. But, if I am rejected, I still won't know what deficiency in these features caused my rejection, and I might still have been an outlier, rejected because of some other feature.

Instead, what *is* helpful to know is, if I am rejected, what would have to have been different for me to be accepted? That is, what would I need to change if I wanted to come back and have my application accepted? There's a simple power to this. If I'm rejected and told "If you had somewhere off-road to park your car, we would have offered you insurance", then I have clear, actionable information about what I need to do if I want insurance from this provider. Equally, if I do already park my car in a garage, I can challenge the rejection as a mistake (maybe the algorithm got this information from somewhere other than my application).

There are some difficulties with this approach though.

First, I think Wachter *et al.* underestimate the complexity of relationships between features in many ML models. If useful decisions could be made through models based on linear combinations of input features, there would be no use for neural nets. There's some acknowledgement of this in the paper, but I would be interested to see how far the approach gets you in the real world. If the counterfactual ends up being a list of 200 things that need to be changed in some minor way, that doesn't feel like useful information (though perhaps, in that case, nothing is going to be very useful).

Second, (and related to the above) there's an implicit assumption in this approach that the features described in the counterfactual are interpretable by humans. Wachter *et al.* have in mind things like, "income", "level of education" etc. But it strikes me that many of the features driving complex ML algorithms are abstract in nature (either carefully hand-engineered abstracted features, or vast amounts of raw input which are, as part of the algorithm, combined into abstracted features). For example, maybe one of the key features used by the car insurer is a "social media coefficient" - some metric extracted from the applicant's social media presence: the content of their posts, the content of their friends' posts, the frequency of alcoholic drinks in photos in which they are tagged etc. Telling an applicant "If your social media coefficient was 3.5 instead of 3.7, we would have offered you insurance" is clearly unhelpful. But how can such features be explained? Perhaps an applicant can self-audit the number of photos of them with alcoholic drinks, but they can't hope to change, or even understand, why their posts (and those of their friends) have been found to exhibit "reckless" sentiments (or whatever NLP has been carried out).

Wachter and Mittelstadt are due to be speaking at the workshop next week, so perhaps they'll have some ideas on these issues.

### No one-size fits all

*["Meaningful information and the right to explanation"](https://academic.oup.com/idpl/article/7/4/233/4762325)* by Selbst and Powles is an attempt to refocus the debate around the "right to explanation" in the text of the GDPR. Selbst and Powles are advocates of a strong "right to explanation", and spend a fair amount of time knocking Wachter *et al.* However, without getting into that debate, there were a couple of things that stood out of the paper for me.

First, they make it clear early on that we should not allow one single type of explanation become the standard for compliance with the GDPR. For example, even if you generally like the counterfactual idea discussed above, you should still stay open enough to allow that other types of explanation are legitimate, and can be used in lawful processing under the GDPR. In Selbst *et al.*'s words:

> *"For example, one might think that meaningful information should include an explanation of the principle factors that led to a decision. But such a rigid rule may prevent beneficial uses of more complex ML systems such as neural nets, even if they can be usefully explained in another way."*

The authors are talking about the dangers of a dogmatic academic conception of "meaningful information", but the sentiment applies equally to market practice. Data protection has always been driven by market practice and it would be unfortunate if a standard type of explanation became "market" such that organisations felt that they could not deploy algorithms which are not capable of explanation on those terms.

Second, in a discussion about when information needs to be given to individuals, Selbst *et al.* muse that, theoretically, ML models could be made available to individuals so that they can run the assessments themselves, before submitting their data to the relevant organisations. My first thought on reading this was that this is typical academic nonsense, utterly detached from the real world and best skipped over for more compelling arguments. However, I wonder now whether that's fair. Obviously, individuals will (typically) not possess the programming skills to implement a model themselves, even it has been made available to them. But couldn't an organisation make a model available in an online environment, in a way which limits opportunity for abuse? Some applications (like credit card applications) take information from external sources each time the model is run, so there would be a challenge in collecting this information without adverse effect on the individual. I'd be interested to hear others view though. What is the scope of the technical challenges that would need to be overcome?

Have any thoughts on this? Please get in touch below.