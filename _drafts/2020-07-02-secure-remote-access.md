---
layout: post
title: Secure remote access
tags: security
---

Recently, there was a public vulnerability disclosure [regarding insecure setup of remote management of Telia users' routers](https://full-disclosure.eu/reports/2019/FDEU-CVE-2019-10222-telia-savitarna-backdoor.html?fbclid=IwAR34rysOVFr41oL-Y-2Ca5vje-kM3uq_Wby8sSAsoBOYjZfS8SZXfKaW-Wo). As this exposes multiple issues with their security setup and I've been openly critical of the situation, I'll write this series of blog articles of how all of this could have been avoided. 

## Collaboration
The first thing that sticks out to me in this situation is the inability to collaborate with the security researchers who discovered the vulnerability. Looking at the timeline (as per CVE), Telia has been first contacted by the researchers on 2019-10-22. You can read the whole story in the CVE, but the short of it is that they didn't seem to be willing to collaborate or even address the vulnerabilities by the time of public disclosure. Please, don't repeat their mistakes. If someone is telling you about a vulnerability in your system, for free, collaborate with them, or at the very least, don't threaten (even if you're investigating the legality of their actions in the background). Also, as they'll disclose the vulnerability publicly, please do address at security issues in question. 

That said, don't give in to extortion efforts - in a company I've worked in the past, we've received emails, saying that "there are multiple serious security issues in our system" and that they'll disclose them for a price. This is not how honest security researchers work.

Now that we established, that we should not ignore security recommendations, let's have a look, how we could do this in a (more) secure way. 

## Transport security
Regardless of whether you 