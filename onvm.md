---
layout: page
title: openNetVM
permalink: /onvm/
---

<img src="/res/onvm-logo.png" style="float:right; padding-left:15px;">

openNetVM is a high performance NFV platform based on [DPDK](http://dpdk.org) and [Docker](http://www.docker.com) containers.  openNetVM can be SDN-enabled, allowing the network controller to provide rules that dictate what network functions need to process each packet flow.

openNetVM is an open source version of the NetVM platform described in our [NSDI 2014 paper](http://faculty.cs.gwu.edu/~timwood/papers/14-NSDI-netvm.pdf), released under the [BSD license](https://github.com/sdnfv/openNetVM/blob/master/LICENSE). Our [HotMiddlebox workshop paper](http://faculty.cs.gwu.edu/~timwood/papers/16-HotMiddlebox-onvm.pdf) is a good way to learn about openNetVM's overall architecture.


## Current Status
**OpenNetVM's source code and documentation are [available on github](https://github.com/sdnfv/openNetVM).**

The fastest way to get started with OpenNetVM is using NSF CloudLab. You can find a premade [profile here](https://www.cloudlab.us/p/GWCloudLab/onvm-18.03). 

<img src="/res/netvm-arch.png" width="700px" style="padding-bottom:10px; margin-left: auto; margin-right: auto; display: block;">

## Features

**Container-based NFs:** Writing and managing network functions for openNetVM is easy since they run as standard user space processes inside Docker containers.

**NF Manager:** This component keeps track of what network functions are currently running in containers and distributes packets to them as they arrive.

**SDN-enabled:** The NF Manager coordinates with SDN controllers using OpenFlow, allowing the controller to specify service chains composed of multiple NFs that must process a flow.

**Zero-Copy IO:** Packets are DMA'd directly into a shared memory region that allows the NF Manager to grant NFs direct access to packets with no additional copies.

**No Interrupts:** We use DPDK's poll mode driver in place of traditional interrupt-driven networking, allowing the system to process packets at line rates of 10 Gbps and beyond.

**Scalable:** NFs can be easily replicated for scalability, and the NF Manager will automatically load balance packets across threads to maximize performance.

## Performance

<img src="/res/onvm-chain-perf.png" width="300px" height="196px" style="float:left; padding-right:10px">

The OpenNetVM Speed Tester NF can be used to measure the throughput of the system by generating either fake packets or replaying PCAP files to simulate real traffic. To stress test packet movement through ONVM, a service chain of Speed Tester NFs can be run on a single machine, avoiding NIC overheads. Because there is no data copying and each NF handles its own sending and receiving of packets, we get high throughput even for long NF chains. Our measurements at left show that a chain of length two using our NF Direct communication mechanism has a maximum throughput of 32 million packets per second, while extending the chain to seven NFs only incurs a 10% throughput drop. Using indirect NF communication via the management layer sees decreasing performance as the manager's TX thread becomes a bottleneck.

<img src="/res/onvm-web-traffic.png"  height="2700px" style="float:right; padding-left:10px">

We have evaluated the performance of NetVM compared to SR-IOV and raw DPDK on a machine with eight 10 Gbps NIC ports. If web traffic is directed to a single NF, we observe a maximum throughput of 48Gbps, at which point the NF itself (running a simple forwarding example) becomes the bottleneck. Starting a second replica of the NF allows OpenNetVM to automatically load balance traffic across the two NFs, while preserving flow affinity. This improves performance up to 68 Gbps, which we believe is the hardware limit on our server.  Even if the traffic is sent through a chain of 5 NFs, we can still process 40 Gbps.

