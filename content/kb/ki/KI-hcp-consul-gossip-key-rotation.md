---
title: "KNOWN ISSUE: No support for gossip key rotation on HCP Consul"
date: '2022-10-09'
draft: false
description: "Production deployment best practices are to have a gossip key rotation process to execute in the event of compromise."
tags: ["KB", "Known Issue", "HCP", "Security"]
featured_image: ''
omit_header_text: true
---

Documented at: 10/06/2022
Review by: 31/01/2023

The HCP Consul platform does not permit operators to execute a Gossip Key rotation. The Gossip key is used to encrypt the consul agent gossip communication. It is important to have a key rotation process for execution in the event of suspected security compromise.
