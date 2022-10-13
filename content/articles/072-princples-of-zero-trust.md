---
title: '#072 - The Principles of Zero Trust'
date: '2022-10-10'
draft: false
toc: true
tags: ['Zero Trust', 'Security']
---

## Overview

The Six Principles of Zero Trust:

* Identity Driven
* Mutually Authenticated
* Authorized
* Time-bound
* Encrypted Transit
* Audited & Logged


To be applied to:

* Access
* Communication


For:

* Human to Machine
* Machine to Machine

---

## The principles

### Identity Driven

"Idnetity" refers to the service itself. Not the host, machine, not its operating system, its network interface, or its IP address. Zero Trust security must apply to the service itself as the origin or destination.


### Mutually Authenticated

We're all used to visiting websites where the site presents a certificate that is verified as trusted, or not. In that curcumstance the website has identified itself to the visiting browser/machine, but the client is both unverified and untrusted, and yet still allowed to communicate. Mutual authentication is a process whereby both sides of the communication are challenged to provde identity. 

### Authorized

While all services might be authenticated, only some should be authorized. Separation of Authenticated and Authorized is critical in the adoption of Zero Trust practice. 


### Time-bound

Open-ended access is an invitation to exploitation. Ensuring that access is short-lived, time-bound, but with auto-renewal is a sound practice to avoid zombie services waiting for hackers. 

### Encrypted Transit

Encrypt everything, everywhere, but do so with an Identity Driven, Mutually-Authenticated implemetation.

### Audited & Logged

Google it.
