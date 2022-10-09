---
title: 'REDtalks #10 - Getting started with Ansible Tower and F5 iApps'
date: Thu, 24 Nov 2016 00:26:42 +0000
draft: false
tags: ['ansible', 'demo', 'introduction', 'video']
---

There's a lot of great content out there on Ansible playbooks. However, not so much on how to get started. So, this article is to share how I got from "never seen Ansible" to "automating L4 - L7 Service deployments". I hope its useful to you!

### WARNING: What are you actually installing?!

When I signed up for an Ansible Tower eval license I received a link to the Ansible Tower download (its a gzipped tarball – .tgz), which assumes you have Ansible running. Ansible Tower is the nice web interface that use the underlying Ansible (CLI) to do the work. Unless you're an expert, don't use the Ansible Tower tgz install! Instead, use the Anisble Tower \*bundle\*, which installs both Ansible and Ansible Tower together, configured and integrated. This way you won't have to mess with dependancies or configuration files afterwards... If you are new to Ansible, I promise you this will make a HUGE difference.

### Lab Guide

The complete step-by-step lab guide, including all the playbooks, has been published to GitHub, which you can find [here](https://github.com/npearce/F5-iApps_and_Ansible-playbooks). However, if video instructions are more your scene, I recorded all the steps in the various lab 'README' files into a single video, below.   https://www.youtube.com/watch?v=eu\_95ItWmyE