---
date: '2025-02-16T07:57:14+01:00'
title: 'Snacka Kubernetes - Supply Chain Security'
author: ["Linus Östberg"]
description: 'A summary of my "Snacka Kubernetes - Supply Chain Security" presentation, with an overview of image and git commit signing.'
summary: "A short overview of image and git commit signing."
tags:
  - "security"
  - "snacka kubernetes"
  - "supply chain security"
  - "cosign"
  - "sigstore"
  - "kubernetes"
categories: 
  - security
  - "snacka kubernetes"
series: 
  - "Snacka Kubernetes"
---

Snacka Kubernetes is a series of webinars presented by [Conoa](https://www.conoa.se/). This is a summary of a [webinar](https://www.youtube.com/watch?v=i7pjf3Tnfxk) (in Swedish) I presented on Febuary 14th. As it was live-streamed on February 14th, the demo had a simple Valentine theme and could be described as "why you should always sign your love greetings".

Most people do dot verify the source of their images. It means that anyone who get write access to the container registry can freely replace the tagged images with their own. Later, when e.g. a Kubernetes cluster pulls the image, the replaced image will be used in place of the real one.

The best way to verify that you use only the images you have built is to sign them. There are multiple ways of doing it, but [Sigstore](https://www.sigstore.dev/) has been emerging as one of the major players. Sigstore consists of multiple parts, e.g. a tamper-proof ledger (Rekor) for signing certificates, and a command-line tool called Cosign to perform the actual signing.

The Cosign tool by default connects to a [public Rekor instance](https://rekor.sigstore.dev/), meaning that some metadata about a signed image is made publicly available if you sign your image. In the cases where this is a problem, you may want to host a private Sigstore setup. Some inspiration for how to set it up can be obtained by looking at [Sigstore Scaffolding](https://github.com/sigstore/scaffolding).

When an image is signed, there will be a signing certificate stored in the Rekor instance and a signature in the container registry. It can be verified using an admission controller with support for Sigstore signatures, e.g. [Kyverno](https://kyverno.io/docs/writing-policies/verify-images/sigstore/). After configuring a cluster policy for verification of e.g. a key signature, any unsigned images will be rejected.

While a signed image may protect from tampering with container registries, there are many other ways to attack a supply chain, see e.g. [SLSA](https://slsa.dev/spec/v1.0/threats-overview) for an overview. SLSA itself is a framework for defining the trust in a build chain, but is not a central topic for this presentation.

No matter how secure the build and deploy chain is, it won't matter if the code is manipulated. It is thus important to also sign any git commits as well so their origin can be verified.

Git has no verification of the authenticity of the commiter. It means that anyone can pretend to be someone else and push commits in the other persons name (given that they are authorised to push to the repository). In order to verify the origin of the commits, they need to be signed. Git supports signing with GPG and SSH keys out of the box. Setting up commit signing with an SSH key is very straight-forward and only requires a few steps.

A simple setup would be:
```
ssh-keygen # Fill in key password, location etc in the prompts
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/keyname.pub
```

Sign commits using `git commit -S`, or sign commits automatically by setting `git config --global commit.gpgsign true`.

If you use Github, you can [associate the SSH public key with your account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) to get a "Verified" marker on your signed commits. Any incorrectly signed (or optionally also unsigned) commits will have an "unverified" marker.
