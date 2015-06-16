---
layout: page
title: openNetVM
permalink: /onvm/
---

<img src="res/onvm-logo.png" style="float:right; padding-right:15px;">

openNetVM is a high performance NFV platform based on [Intel DPDK](http://dpdk.org) and [Docker](http://www.docker.com) containers.  **openNetVM** is SDN-enabled, allowing the network controller to provide rules that dictate what network functions need to process each packet flow.

openNetVM is an open source release of the NetVM platform described in our [NSDI 2014 paper]((http://faculty.cs.gwu.edu/~timwood/papers/14-NSDI-netvm.pdf) under the [Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0).

## Features

**NF Manager:** This component keeps track of what network functions are currently running in containers and distributes packets to them as they arrive.

**Container-based NFs:** Writing and managing network functions for openNetVM is easy since they run as standard user space processes inside Docker containers.

**SDN-enabled:** The NF Manager coordinates with SDN controllers using OpenFlow, allowing the controller to specify service chains composed of multiple NFs that must process a flow.

**Zero-Copy IO:** Packets are DMA'd by Intel DPDK directly into a shared memory region that allows the NF Manager to grant NFs direct access to packets with no additional copies.

**NUMA-Aare:** openNetVM maximizes performance by ensuring that packets in memory DIMMs local to a particular socket are only processed by threads running on that socket.

**No Interrupts:** We use DPDK's poll mode driver in place of traditional interrupt-driven networking, allowing the system to process packets at line rates of 10 Gbps and beyond.
