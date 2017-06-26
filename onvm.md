---
layout: page
title: openNetVM
permalink: /onvm/
---

<img src="/res/onvm-logo.png" style="float:right; padding-left:15px;">

openNetVM is a high performance NFV platform based on [Intel DPDK](http://dpdk.org) and [Docker](http://www.docker.com) containers.  openNetVM is SDN-enabled, allowing the network controller to provide rules that dictate what network functions need to process each packet flow.

openNetVM is an open source version of the NetVM platform described in our [NSDI 2014 paper](http://faculty.cs.gwu.edu/~timwood/papers/14-NSDI-netvm.pdf), released under the [BSD license](https://github.com/sdnfv/openNetVM/blob/master/LICENSE).


## Current Status
Our [HotMiddlebox workshop paper](http://faculty.cs.gwu.edu/~timwood/papers/16-HotMiddlebox-onvm.pdf) is a good way to learn about openNetVM's overall architecture.

**OpenNetVM's source code and documentation are [available on github](https://github.com/sdnfv/openNetVM).**

We will be releasing [NSF CloudLab](http://cloudlab.us) template images soon.  Please [contact us](mailto:timwood@gwu.edu) if you are interested in testing these.

## Features

<img src="/res/netvm-arch.png" style="float:right; padding-left:15px; padding-bottom:10px">


**Container-based NFs:** Writing and managing network functions for openNetVM is easy since they run as standard user space processes inside Docker containers.

**NF Manager:** This component keeps track of what network functions are currently running in containers and distributes packets to them as they arrive.

**SDN-enabled:** The NF Manager coordinates with SDN controllers using OpenFlow, allowing the controller to specify service chains composed of multiple NFs that must process a flow.

**Zero-Copy IO:** Packets are DMA'd directly into a shared memory region that allows the NF Manager to grant NFs direct access to packets with no additional copies.

**NUMA-Aware:** openNetVM maximizes performance by ensuring that packets in memory DIMMs local to a particular CPU socket are only processed by threads running on that socket.

**No Interrupts:** We use DPDK's poll mode driver in place of traditional interrupt-driven networking, allowing the system to process packets at line rates of 10 Gbps and beyond.

**Scalable:** NFs can be easily replicated for scalability, and the NF Manager will automatically load balance packets across threads to maximize performance.

## Performance

Our original NetVM platform was able to achieve performance significantly higher than the state of the art when processing packets through a chain of virtual machines. We expect similar or better performance on our updated openNetVM platform.

<img src="/res/netvm-perf.png" width="300px" height="221px" style="float:left; padding-right:10px">

We have evaluated the performance of NetVM compared to SR-IOV and raw DPDK on a machine with four 10 Gbps NIC ports. DPDK provides the highest performance for software-based switching, but it does not directly support running NFs in virtual machines or containers, limiting its use for NFV deployments.  SR-IOV allows a physical NIC to be virtualized and given to a virtual machine for direct access; we measure the performance of running DPDK inside a VM with SR-IOV. The forwarding rate achieved by NetVM for 64 byte packets significantly surpasses that of SR-IOV, nearly reaching the same level of performance as DPDK, even though NetVM must transfer packets from the host to the virtual machine that processes the packets.

The performance gap between SR-IOV and NetVM becomes even larger when there are multiple NFs that must process a packet.  SR-IOV drops to nearly one third of the line rate when sending packets through a chain of two VMs, while NetVM can maintain the full line rate as long as there are sufficient CPU cores to dedicate to each VM.

For additional performance results, please see our [TNSM Journal article](http://faculty.cs.gwu.edu/~timwood/papers/15-TNSM-netvm.pdf).
