---
title: 'REDtalks.live #024: Becoming Super-NetOps: Day 1'
date: '2018-01-30'
draft: false
tags: ['API', 'Super-NetOps', 'Automation']
---

If you're right at the beginning of your Super-NetOps journey, then this article is for you!

{{< youtube -UF_D2XdIQM >}}

A common question from our NetOps pals is, "How do you get started?" Well, let me take a stab at that in todays article. Before we begin, a few things to keep in mind.

1.  EVERYONE is starting from a different level. Focusing on who's ahead of you in the learning path will only distract you from your success. This journey is about you. Go at YOUR pace.
2.  Making mistakes is good. Mistakes are evidence that you are learning new things and have the confidence in yourself to evolve. Learn more about a great culture for innovation from John Allspaw, [here](https://codeascraft.com/2012/05/22/blameless-postmortems/). <- A good one to share with the team!
3.  Beyond the tools and scripts, pay close attention to how you change your approach to troubleshooting and moving forward with a solution. Often overlooked, learning to automate without adopting and nurturing new practices and culture is almost pointless. Take the time to see how process changes and how you begin to look at challenges differently.

Ok, keeping these points in mind, here's some new concepts to look into:

1. Understand RESTful interfaces
=================================

If this is your first time venturing away from the GUI/CLI, or maybe you just want a refresh, I recommend you watch this great REST API introduction video posted by [WebConcepts](https://www.youtube.com/channel/UCV4-mrR8UZh6AsWZbmW5uhQ). In this video you'll see how you can communicate with popular on-line services including Facebook, Google Maps, and Instagram via their REST APIs:

{{< youtube 7YcW25PHnAA >}}

2. Interact with a RESTful Interface
=====================================

There are many tutorials on the internet that show how to communicate with a RESTful interface from a scripting language, like Python or Javascript. But what if you don't know the scripting languages they refer to? Sometimes its best to avoid an overload of too many new concepts to learn at once. For this very reason, I tend to direct people to the awesome, multi-platform REST client, POSTMAN: [https://www.getpostman.com/](https://www.getpostman.com/) While their messaging does target 'API Development teams', its fantastic for API beginners, too. With POSTMAN installed, I recommend you watch the great tutorial "How to use the POSTAN API Response Viewer": https://youtu.be/e0LSRmPw-b0   Once you've worked through the basics, I recommend going through the POSTMAN video tutorials to learn some of the time-saving features you'll come to depend on: [https://www.youtube.com/playlist?list=PLM-7VG-sgbtCJYpjQfmLCcJZ6Yd74oytQ](https://www.youtube.com/playlist?list=PLM-7VG-sgbtCJYpjQfmLCcJZ6Yd74oytQ)

3. Troubleshooting JSON
========================

Now that you've had some interaction with a RESTful interface, you've probably had some experience with how a small error can break things. Fear not, while you're on your path to becoming JSON-fluent there's always the great on-line JSON validator, [https://jsonlint.com/](https://jsonlint.com/) Simply 'paste' your misbehaving JSON data into the text field and click 'Validate JSON'. Below its showing me that I missed a comma on the end of the second line: ![JSONLint_-_The_JSON_Validator](https://redtalkslive.files.wordpress.com/2017/12/jsonlint_-_the_json_validator.png) We'll look at more data formats, like YAML, in future posts.

4. Conclusion
==============

If you've worked through these exercises then congratulations is in order. You have already begun your journey towards becoming a Super-NetOps engineer! If you're still feeling a little overwhelmed with these concepts, there is no shame is working through them again from the beginning. Repetition builds expertise and being comfortable with change is all part of the journey! Next in the series we'll look at some more advanced POSTMAN features and then take what you've learned in POSTMAN and apply it to scripting languages.