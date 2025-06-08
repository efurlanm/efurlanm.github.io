---
draft: false
date: 2024-01-25
categories:
  - Virtualization
tags:
  - LXC
  - Conteiner
---

# MicroCloud

MicroCloud is an interesting project that, through lightweight virtualization and few host computers, allows you to quickly generate a cluster of several computing nodes, requiring few resources and allowing you to use common available hardware at home, such as Raspberry Pi and laptops.

<https://canonical.com/microcloud>

Its sufficiently small to function on a developer laptop. It can be utilized for safely experimenting with new technologies, simulating or testing complicated infrastructure processes, simulating how your workloads would operate in production, and creating lightweight, restricted disposable testing environments.

It is a simple method to start an LXD cluster with high availability. It makes use of *snap* packages, has the ability to automatically configure LXD and Ceph on a group of servers, and uses mDNS to find other servers on the network automatically. This allows you to run a single command on one machine to build an entire cluster.
