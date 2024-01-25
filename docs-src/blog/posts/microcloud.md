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

[MicroCloud](https://canonical.com/microcloud) is an intriguing project that, through virtualization, quickly generates a cluster of up to 50 computing nodes that require few resources, allowing you to use common hardware available at home to simulate a cluster, such as Raspberry Pi and Laptops. Lightweight virtualization allows several computers to be emulated by one host computer.

It is a simple method to start an LXD cluster with high availability. It makes use of *snap* packages, has the ability to automatically configure LXD and Ceph on a group of servers, and uses mDNS to find other servers on the network automatically. This allows you to run a single command on one machine to build an entire cluster.
