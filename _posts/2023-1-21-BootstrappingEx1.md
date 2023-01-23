---
layout: post
title: "An Exploration of Bootstrapping"
subtitle: "A simple, yet powerful strategy."
date: 2023-01-21 13:45:00 -0400
background: '/img/posts/bootstrap-expl.jpg'
---

## What is Bootstrapping?
<br>
 Think for a moment about human drug trials. They are usually conducted on small groups volunteer subjects who are compensated for the their participation. Repeating a trial would require orchestrating a new group of volunteers and committing the time to run each trial. For many reasons, this is often impossible, and we are left with our original small sample. From this, we can calculate any statistic of interest, such as the trimmed mean or the odds ratio. But, from only one small sample, how can we get a feeling for how these sample statistics vary and their quality as an estimator? Its often the case, such as a small group of volunteers, that we cannot reasonably make any assumptions about the sampling distribution that aid with inference about our statistic(s). For instance, one may determine the sample mean and standard deviation to contruct a confidence interval to test some hypothesis. There's a lovely formula for this, but this involves the assumption of a normal distirbution. We can't necessarily make this assumption because the Central Limit Theorem only guarantees assymptotic normality for large enough sample sizes. Resampling methods can conveniently circumvent the (often failed) requirements for classical inference, and allow us to make sound conclusions from only a single small sample. Bootstrapping is a simple, yet powerful resampling method that is commonly used. 

 Lets start with a simple example to demonstrate the basic idea of bootstrapping. Suppose we run an experiment giving treatment A to a group of 10 volunteers, and a placebo to 10 others. We take some metric $\mu$ which indicates drug performance (higher is 'better'), and compare the average of this metric in the treatment group to that of the control. Suppose this is our data.
 

 We arrive at a modestly large