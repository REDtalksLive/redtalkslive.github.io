---
title: 'REDtalks.live #34 - GitHub Integration v0.2 released'
date: Mon, 09 Jul 2018 14:30:23 +0000
draft: false
tags: ['demo', 'GitHub', 'Infrastructure as Code', 'video']
---

Greetings automators!!! I am delighted to be sharing with you the release of the GitHub Webhook Server for BIG-IP v0.2. w00t! This release was a major re-write that touched almost every line of code. Why? Well, as with many early prototypes, in the first release I just wanted to see if it could be done. And from the experiment I learned a lot about integrating with GitHub, took a lot of notes, and, well, v0.2 is the result of all those findings: a more robust integration that you should totally take a look at yourself! The two key adds to version 0.2 are:

*   deployment queuing – so we don't DDoS the Control Plane with many concurrent declaration commits at once
*   octokit –  in v0.1 I used native HTTP calls to the GitHub API. In v0.2 I switched to the GitHub octokit/rest.js node module, which simplified a lot of code. And simple = robust.

There's a couple other features I snuck in there, which you can see in this demonstration video: https://www.youtube.com/watch?v=locapQwjXZI   If there are any features you'd like to see, please create a GitHub Issue against the repository, here: [https://github.com/npearce/BigHook/issues](https://github.com/npearce/BigHook/issues) For more episode on Network Infrastructure as Code, go here: [Network Configuration as Code](http://redtalks.live/cac/) Thanks for listening!