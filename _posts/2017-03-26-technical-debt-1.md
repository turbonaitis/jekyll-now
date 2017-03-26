---
layout: post
title: What technical debt to avoid (1)&#58; New components
tags: technical-debt
---
Every software developer wants to create perfect software. Every product manager wants their product to have no flaws. Every customer wants an immaculate user experience. Should this be our goal?

In my opinion - no, not without the constraints at least. There's a saying, that _good-enough has made much more millionaires, than perfect_. I think it's absolutely spot on - achieving that frictionless user experience, that fully-featured product and that reference-level architecture takes an enormous amount of time (and other resources) - and we never have enough of that. That's why we inevitably we all have to cut corners, the real question is which ones and by how much. The way I'll try to come to an answer it in the following series of posts is by looking at what not to do.

One of the errors, that I've seen being made reasonably often, is releasing a new component to production environment, without it meeting certain criteria of being production-ready. Maybe it's because it's "just a POC" (more on POCs in later posts), maybe because you're REALLY in a hurry - it doesn't matter, this is not the corner, that you want to cut. If you're releasing some piece of software to your users - here's a minimalistic production readiness checklist, that I'd use:
  - __Is the code in the company's code repository?__ Regardless of how great or small or urgent your changes are, they need to be in a central repository, so that others can find that code later and modify it.
  - __Does it meet the requirements of my release process?__ If you're releasing it once, you're quite likely to release it again - maybe it's a new feature, maybe it's fixing that "random feature", which was found after initial release. So whether it's a CD pipeline for this component, or a simple list of manual instructions (for those who are still stuck in the last century) - you want to make sure they exist. Having to release an urgent bug fix and realising, that you don't know how or where, is certainly not great. If other components have a CI/CD pipeline - the new one should too.
  - __Can it be efficiently diagnosed in production?__ Even if it's a POC, that'll only be there for a short while, you still want to know whether it's working and if not - why. Actually, it's almost more important with POCs - what concept can you prove, without knowing, if anything works at all? So have sufficient level of logs and if you're using some kind of log-aggregation service, make sure it is picking up the logs of this new component.

  Have you ever released something to production in a hurry only to later regret it? What steps of production readiness did you miss? Please let the others know in the comments section.
