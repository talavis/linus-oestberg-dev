---
date: '2025-05-06T16:20:57+02:00'
title: 'Practical Certification Exams at LF'
author: ["Linus Östberg"]
description: 'Experiences with writing practical certification exams at Linux Foundation'
summary: "Opinions about what setup to use for doing practical certification exams at Linux Foundation (CNCF)."
tags:
  - "kubernetes"
  - "linux-foundation"
categories:
  - "kubernetes"
---

I recently renewed my CKS certification from CNCF. Sadly, the experience was everything but smooth.

Tested setups:
## Mac

Using a M2 Macbook Pro 13 with the latest 15.4.1 macOS.

* Smooth installation
* Unable to write any special characters (instead having to use the virtual keyboard)
* No graphical bugs in the PSI browser

## Linux

Using a freshly installed Kubuntu 24.04 on a Dell XPS 13.

* Manual installation
* Requires reconfiguration of AppArmor to avoid PSI Browser crashing during install:

```
sysctl -w kernel.apparmor_restrict_unprivileged_userns=0
```

* Works fine once installed
* Special characters work fine
* Graphical bugs for the environment
  - All windows shifted (clicks ended up in unexpected places)
  - Graphics corruption outside the active desktop
  - Improved after resetting the desktop connection


## Network

Do no use WIFI, no matter the quality. The latency is messing up all functionality, including copying values from the sidebar.
