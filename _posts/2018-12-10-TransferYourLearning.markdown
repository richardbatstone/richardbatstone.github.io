---
layout: post
title:  "Transfer your learning"
date:   2018-12-10 12:02:25 +0000
categories: other
---

![Header](https://richardbatstone.github.io/images/DD_008.PNG)

It's not often that you see academic papers cited in the national press, so it was great to see the [Financial Times](https://www.ft.com/content/a3943548-e9cb-11e8-94da-a6478f64c783) pick up on some recent work in 'transfer learning' for natural language processing (NLP).

### What is transfer learning?

Transfer learning is a machine learning problem and one of the most important short term challenges for NLP.

The problem that transfer learning tries to solve is how to build sophisticated ML models in scenarios where you do not have much data. For example, say you want to build an app that recognises different species of birds. To build this model from the ground up you would likely need many hundreds of thousands of labelled example images of birds. If you don't have that data to start with, then it will be costly and time consuming to collect.

However, intuitively, we should not have to start from scratch when building this model. The first few layers of the model will probably learn to extract generic features of objects, like edges or corners. It's only in the higher levels of the model that these will be brought together to learn to distinguish between different species of birds. So rather than re-learn vision from scratch with birds, it makes more sense to train a generic image classifier on one of the big publicly available datasets (i.e. build a model that can distinguish a bird from a bridge), and then 'fine-tune' it on a much smaller dataset of different species of bird.

### Transfer learning for NLP

This approach has seen great success in computer vision but has been much harder to replicate in NLP. This has been frustrating for developers and researchers alike because there are lots of use cases for NLP where the volume of relevant training data is small. For example, auto-predict / complete functions for email would be more convincing if they could mimic the user's writing style, but few users (even the most verbose lawyers) generate enough text to train a model from scratch. Alternatively, we might want to classify some legal text but have only a few labelled training examples to hand: the ability to leverage the data we already have rather than diverting internal resources to label more examples would be a massive boon.

The [methods](https://arxiv.org/abs/1801.06146) described by [Sebastian Ruder](http://ruder.io/) and [Jeremy Howard](https://www.fast.ai/about/) earlier this year point a way through this barrier and, in doing so, open up lots of doors to new industry applications. We're certainly looking forward to exploring how these techniques can be brought to bear in the legal world!

> Transfer learning — taking a general-purpose model trained on one data set and fine-tuning it to work on a different problem — is one of the most promising techniques for making the technology more flexible and cost effective. [Financial Times](https://www.ft.com/content/a3943548-e9cb-11e8-94da-a6478f64c783)

![bird](/images/bird.jpg){:class="img-responsive"}