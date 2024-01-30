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

[MicroCloud](https://canonical.com/microcloud) is an interesting project that, through virtualization, quickly generates a cluster of computing nodes that require few resources, allowing you to use common hardware available at home to simulate a cluster, such as Raspberry Pi and Laptops. Lightweight virtualization allows several computers to be emulated by one host computer.

Its sufficiently small to function on a developer laptop. It can be utilized for safely experimenting with new technologies, simulating or testing complicated infrastructure processes, simulating how your workloads would operate in production, and creating lightweight, restricted disposable testing environments.

It is a simple method to start an LXD cluster with high availability. It makes use of *snap* packages, has the ability to automatically configure LXD and Ceph on a group of servers, and uses mDNS to find other servers on the network automatically. This allows you to run a single command on one machine to build an entire cluster.
