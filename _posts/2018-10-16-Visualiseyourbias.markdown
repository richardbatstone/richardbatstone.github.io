---
layout: post
title:  "Visualise your bias"
date:   2018-10-16 12:02:25 +0000
categories: other
---

![Header](https://richardbatstone.github.io/images/DD_006.PNG)

Tackling bias and unfairness in machine learning models can be tricky. The more complex the model, the harder it can be for developers and users to interrogate how inputs relate to outputs, and there have been many well publicised examples of models producing weird and/or unsettling results.

Part of the solution might be to empower more people to inspect what is going on inside ML models. That’s where I think tools like Google’s new “What-if Tool” will come in. The “What-if Tool” is a plug-in for Tensorboard, Google’s existing visualisation package for its popular ML platform, Tensorflow. But you don’t need to be familiar with Tensorflow to use the tool: the idea is that developers build a model and then anyone can poke around and see how the model treats different inputs. 

There are a few toy models to play with online and if you have a spare 10 minutes I would thoroughly recommend having a look (e.g. see the “Smile Detection” model, for which Google challenges you to work out which group is missing from the training data, resulting in a biased model).

There might also be a place for some capabilities like these to be made available to people who are the subject of a model’s decision making. For example, the “counterfactual” feature shows you, for a given input, the nearest input for which the model predicted a different outcome. That is, it can show you how the world would need to be different for you to get the result you want. Suitably anonymised, that would be fascinating information for someone (for example) applying for an online credit card, and damning if the model revealed the only difference between a successful and unsuccessful applicant was their gender, race or other protected characteristic.

> What If... you could inspect a machine learning model, with no coding required? [Google's What If tool](https://pair-code.github.io/what-if-tool/index.html)

![equity](/images/equity.jpg){:class="img-responsive"}