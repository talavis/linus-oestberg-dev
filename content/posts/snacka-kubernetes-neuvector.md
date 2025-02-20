---
date: '2025-02-20T17:08:33+01:00'
title: 'Snacka Kubernetes - Neuvector'
author: ["Linus Östberg"]
description: 'A summary of my "Snacka Kubernetes - Neuvector i praktionen" presentation, covering some of the things I consider important when running Neuvector in production.'
summary: "What to think of when running Neuvector in production."
tags:
  - "security"
  - "snacka kubernetes"
  - "neuvector"
  - "kubernetes"
categories: 
  - security
  - "snacka kubernetes"
series: 
  - "Snacka Kubernetes"
---

Snacka Kubernetes is a series of webinars presented by [Conoa](https://www.conoa.se/). This is a summary of a [webinar](https://www.youtube.com/watch?v=e6AcCVxS9u8) (in Swedish) I presented on November 22nd 2024.

SUSE Security, formerly known as Neuvector, is an open-source container security platform intended especially for Kubernetes. In the text i will use the abbreviation NV. Like any other security platform, it's not enough to just install it: it also requires customization and active monitoring over time.

It can be easily installed using a [helm chart](https://github.com/neuvector/neuvector-helm) with the default settings, but I recommend that you check the values to harden the setup (the values file even contains multiple `MUST be set for Rancher hardened cluster` comments).

If you are using Rancher, you need to make the decision whether you want to use the Rancher-integrated version or the stand-alone version of NV. The main difference is in regard to how to authenticate: in one case you use Rancher, in the other you use local accounts, AD, OIDC, or SAML. Personally, I prefer to use the stand-alone version as it currently allows for more fine-grained RBAC management for different teams.

The first thing you should do after setting up a NV instance is to active auto-scanning. It means that the program will automatically scan both running containers and the cluster nodes, providing information about all known CVEs. NV can also set up automated register scanning, providing CVE information for each layer in the scanned image.

As can be expected from a container security program, it is possible to both list workloads/nodes with a CVE, as well as list CVEs in a workload/node. It is also possible to "accept" specific CVEs, both globally and at the workload level.

One of the main functionalities of NV is it's runtime protection. It analyses two aspects for each workload (deployment, pod etc): network and processes. The protection is based on whitelisting the allowed traffic and processes, while alerting about or blocking anything not listed. In `discovery` mode, NV will consider any observed traffic or process as normal and it to the whitelist. Monitor and protect in turns alerts or blocks the processes and network trafic. The modes for network traffic and processes can be set separately. In my experience, the process protection works well, but someone needs to be responsible for keeping track of any changes to the behaviour of the workload to make sure it doesn't suddently stop working because of blocked processes or network traffic. I believe this work should be performed by the (development) team responsible for the application. They are the ones who know the most about the application.

The rules for the runtime protection can be managed as a namespaced CRD that can be synchronised via e.g. a GitOps system.

There are a couple of different types of security events that are reported by NV. Of course, any violations of the runtime security rules are listed as security events, but there are also some other built-in rules detecting suspicios activity such as nmap runs inside a container. Apart from the security events, events are also generated for e.g. vulnerability scanning results.

It is possible to define response rules for any events, the available responses are to send a notification to e.g. a webhook, or to put a pod in quarantine. Quarantine means that any ingress or egress traffic to the pod will be blocked, and differs from the pod killing that is common in many other container security programs. By defining response rules, you can e.g. set any container containing a certain CVE in quarantine, making sure it cannot be exploited.

When it comes to configuring a NV instance, there are a couple of choices avaialble. Of course, you can configure it using the GUI (frontend) but it's also possible to set the config in the helm chart as config maps and secrets, but that config will only be applied when the NV controller pods have been scaled to 0 replicates. It is also possible to use federation to access and federate the configuration to multiple instances. Finally, each NV instance has an [API](https://github.com/neuvector/neuvector/blob/main/controller/api/apis.yaml) with access to all configuration and information in the Neuvector instance, e.g. scanning results.

Personally, I prefer to use Infrastructure as Code, and the only efficient way I've found to apply configuration in a running NV instance has been to write some code that will interact with the API and apply the configuration that way.
