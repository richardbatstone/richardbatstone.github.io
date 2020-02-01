---
layout: post
title:  "A day at the Turing Institute"
date:   2018-04-05 08:02:25 +0000
categories: GDPR
---

![Header](https://richardbatstone.github.io/images/DD_003.PNG)

A quick follow up on the "GDPR and Beyond: Privacy, Transparency and the Law" conference at the [Alan Turing Institute](https://www.turing.ac.uk/).

Overall, a really interesting day with lots of intelligent debate, with speakers and attendees from a broad range of fields (including an appearance from Elizabeth Denham, before she headed back to court…) Here are some of my thoughts from the day.

### The ICO's view: AI and the public

Elizabeth's talk was a fairly high level statement piece about the importance of, and challenges facing, good privacy regulation in the AI world. I think the recording will be made available, but a few the points that stood out for me:

1. She sees the use of “stop processing” notices as a key part of the regulator’s tool kit (and noted they can have a greater impact than fines);

2. She suggested that explanation plays a key role in fair processing: if processing can’t be explained, then it will not be fair. I think that’s intuitive but perhaps lost in the ongoing debate around the provision of explanations in the case of automated decision making. Whether or not you are engaged in automated decision making, you should (internally at least) be able to explain the processing. Some of the arguments raised against the provision of explanations in automated decision making would, I think, fall foul of the more general principle; and

3. She acknowledged that it is a current line of enquiry as to whether or not you need explicit consent to process special categories of personal data where that information has been inferred from ordinary personal data. I think that’s a far reaching question and there was some further discussion on this point later in the day, though I can’t see that there will be a clear cut answer. 

### Counterfactual explanations

Sandra and Brent also gave a bit of a walkthrough of their paper on counterfactual explanations (previously discussed [here](https://richardbatstone.github.io/gdpr/2018/03/17/brushinguponexplanations.html)). I had a quick chat with Sandra about the challenges of providing counterfactual explanations where the underlying variables are not easily interpretable. She suggested that you can usually make some head way on ‘hard to interpret tasks’ (e.g. where decisions have driven by semantic properties of processed data, by describing the key words in the processed data which led to the decision), but I’m still sceptical that it will always be possible to "scale up" this approach.

### Are we doing ML wrong?

One of the most challenging points of view came from Mireille Hildebrandt. Mireille is a lawyer and philosopher at [Virje University, Brussels](https://www.vub.ac.be/LSTS/members/hildebrandt/) (and is also a dedicated tweeter - @mireillemoret) whose central proposition is that data minimisation and purpose limitation (as envisaged in the GDPR) will give rise to better machine learning. This didn’t always chime with the mood in the room, with, I think, most people’s intuition being that purpose limitation restricts legitimate ‘data exploration’. 

However, the more I read about these ideas, the more I think there’s a really serious point to be made. Just after the conference, I heard this great interview with [Claire Gollnick](https://twimlai.com/twiml-talk-121-reproducibility-philosophy-data-clare-gollnick/), which echoed many of Mireille’s concerns. I’ll aim to do a longer piece on the topic but, for now, the intuition is that the only legitimate way to do rigorous data analysis is to be clear about your purpose/aims/hypothesis from the outset, and to collect and process your data accordingly. Simply sitting down with a large data set and performing grid searches to identify relationships which “generalise” to your test set is just bad science.

Have any thoughts on this? Please get in touch below.