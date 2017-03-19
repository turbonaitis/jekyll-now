---
layout: post
title: Choosing AWS region
tags: aws
---
When we were building our system, one of the decisions we had to make was which AWS region to host it in.

At the moment, AWS didn't have a region in the country, where our customers are, so we had to chose another one. As I had pretty much no cloud experience then, I had to rely on the advice from our external experts (or "experts", as I understand now) when making AWS-related decisions. We also wanted to use a certain set of AWS services and at the time, the region, which seemed to be close enough to us and also supported all of these services was Sydney. We've also asked our AWS "experts", how hard will it be to migrate to another region, if need be, and they assured us, that it'll only take a couple of days at most. So we went for Sydney.

It turned out, that the distance was adding 250-300ms latency to our requests, which was making the user experience quite slow, so now we're in the middle of a migration project. You might ask how well is it going? Well, it's certainly taking more than those few days, but I'll leave that for another post. There are things to be learned from it too.

The key learning for me is that regardless of what you might be told, migrations are hard, and so when choosing, where to host your service, you need to consider the following (in my subjective order of priority):
1. __Physical distance to your customer__ - this of course depends on your particular application, but for us, as we're a consumer-facing service, this was the main mistake. Also, what seems close geographically, is not necessarily the location, that has the quickest connection - those internet cables always don't  follow a straight line, so please check the latency before you decide. There is a number of tools out there:
  - <https://www.cloudping.co/> shows latency matrix between different AWS regions
  - <http://www.cloudping.info/> can measure latency to different AWS regions from your machine
  - <http://www.azurespeed.com/> measures different speed metrics, including latency, from your machine to different Azure data centers
I'm sure there are other tools out there, but this should give you a good general idea.
2. __Features available__ in different AWS regions, differ, so be sure to check, whether all the mandatory ones are there, before committing to anything. What if they're not? Well, then you need to make a decision, depending on whether you can work around the missing feature, or not.
3. __Cost__ may also differ between regions, so pay attention to that, especially if you're consuming a lot of resources.

To sum up, the "measure thrice before you cut once" fits this story quite well - you need to consider different factors before choosing your AWS region, as picking the wrong one might cost in different ways.

What is your experience with choosing a region for your cloud deployment? Any other important factors to consider? Share your experiences in the comments.
