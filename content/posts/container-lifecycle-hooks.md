---
date: '2026-01-12T11:10:00+01:00'
title: 'Container Lifecycle Hooks Can Block Pod Creation and Termination'
author: ["Linus Östberg"]
description: 'An example of an issue that may be caused by handling container lifecycle hooks in a suboptimal way'
summary: 'Container lifecycles hooks can make sure commands run before and after container creation. However, they may block container start and termination indefinitely, and you should be very careful when you use them.'
tags:
  - "containers"
  - "lifecycle hooks"
  - "kubernetes"
categories: 
  - kubernetes
---

During one of my assignments as a consultant, a team ended up with a misbehaving pod. It was perpetually stuck in the `ContainerCreating` state, and when deleted it instead got stuck in the `Terminating` state. When it had been in that state for more than 24 hours, my help was requested.

*The examples below is a recreation done using my Rancher Desktop setup*

As I started off with a pod in the `Terminating` state, my first thought was to investigate logs and to check whether there were any finalizers; nothing. Finally I logged into the node and manually deleted the container, which in turn killed the pod. Instead I got a new pod that got stuck in the `ContainerCreating`state. Killing that pod gave me a container stuck in the `terminating` state, and I was back where I started.

```
$ kubectl get pods
NAME     READY   STATUS              RESTARTS   AGE
broken   0/1     ContainerCreating   0          5s

$ kubectl delete pods broken

$ kubectl get pods
NAME     READY   STATUS        RESTARTS   AGE
broken   0/1     Terminating   0          11m
```

After again manually killing the container, I was back at the `ContainerCreating` state. As the pod was still starting, Kubernetes reported that no containers were, and thus no logs were available.

```
$ kubectl get pods
NAME     READY   STATUS              RESTARTS   AGE
broken   0/1     ContainerCreating   0          3m34s

$ kubectl logs broken
Error from server (BadRequest): container "hooks-container" in pod "broken" is waiting to start: ContainerCreating
```

Interestingly enough, the container was running, and logs were available when interacting with the container on the node:

```
$ docker ps
CONTAINER ID   IMAGE                        COMMAND                  CREATED          STATUS          PORTS     NAMES
18d35d234415   nginx                        "/docker-entrypoint.…"   16 minutes ago   Up 16 minutes             k8s_hooks-container_broken_hooks_e54e57ac-0046-4a28-b380-63582239f086_0
/.../

$ docker logs 18d35d234415
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/.../
```

At this stage, I started looking more thoroughly at the pod definition and a couple of rows caught my attention:

```
lifecycle:
  postStart:
	exec:
	  command: ...
  preStop:
	exec:
	  command: ...
```

The pod was using [container lifecycle hooks](https://kubernetes.io/docs/concepts/containers/container-lifecycle-hooks/). These hooks are run at certain points in the container lifecycle. The `postStart` hook runs after the container has started, but stops the pod from reaching the state `Running` until it has finished. The `preStop` hook runs before the container is terminated, and can thus block pod termination.

In the teams case some data failed to sync, and thus the `postStart` and `preStop` commands never terminated, blocking the pod start and termination. After the data was synced correctly, everything worked as intended.

As the hooks prevented the pod from starting and terminating, and also prevented the users from getting information about what was happening in the pod, they extended the time needed to remediate the issue by a lot. The manual intervention to check the container logs and to stop the containers also required intervention by admins, increasing the complexity as well. In order to avoid this type of issue, avoid handling/using the container lifecycle hooks or, if they truly are needed, make sure to not block pod creation and termination indefinitely, and finally make sure that the pod behaviour is well documented.
