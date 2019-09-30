---
layout: default
title: CCP
---
### CCP: Congestion Control Plane

The congestion control plane is a new API for writing congestion control algorithms.
It provides three benefits:
1. *Supporting Sophisticated Algorithms*. [Many](https://github.com/StanfordSNR/indigo) [modern](https://github.com/PCCproject) [congestion](http://alfalfa.mit.edu/) [control](http://web.mit.edu/remy/) proposals use sophisticated processing to make control decisions. It is [often difficult](https://netdevconf.org/0x12/session.html?a-pcc-vivace-kernel-module-for-congestion-control) to implement these algorithms within the "datapath" - any module that provides data transmission and reception interfaces between higher-layer applications and lower-layer network hardware. CCP allows algorithm implementations to take advantage of useful user-space libraries independent of the target datapath.
2. *Write-Once, Run-Anywhere*. Recently, [new](https://www.chromium.org/quic) [datapaths](http://shader.kaist.edu/mtcp/) have emerged, leading to the "matrix of sadness," where congestion control algorithm developers must re-implement their algorithms anew on each datapath. CCP provides a runtime for algorithms which interfaces with each datapath, so the same algorithm code runs in multiple datapaths without modification.
3. *New Capabilities*. It is difficult to implement new congestion control capabilities such as the [congestion manager](http://www.nms.lcs.mit.edu/cm/) in current datapaths. CCP allows developers to add these capabilities without datapath modification.

To get started using CCP, please see [our guide](https://ccp-project.github.io/ccp-guide/). It provides detailed setup instructions
for running existing algorithms on CCP and a tutorial for writing new algorithms.

To learn more, please refer to [our SIGCOMM 2018 paper](https://akshayn.xyz/res/ccp-sigcomm18.pdf).

If you have questions, you can reach us at `ccp@csail.mit.edu`.

### Implementation

We have implemented several components in the CCP ecosystem:

1. [*Portus*](https://github.com/ccp-project/portus) is a user-space CCP agent. It provides a runtime and API for developers to implement congestion control algorithms. 
It is published with documentation [on crates.io](https://crates.io/crates/portus).
2. [`libccp`](https://github.com/ccp-project/libccp) is a convenience library which, given an IPC mechanism and the definitions of various congestion control primitives in the context of a datapath, can communicate with `Portus` to run a congestion control algorithm.
3. ['ccp-kernel`](https://github.com/ccp-project/ccp-kernel) implements a Linux kernel TCP datapath using the pluggable TCP API.
4. [`ccp-mtcp`](https://github.com/ccp-project/ccp-mtcp) is a fork of mTCP, a TCP stack on top of DPDK, which implements CCP support.
5. [`ccp-quic`](https://github.com/ccp-project/ccp-quic) hosts a patch to QUIC which implements CCP support.
6. Finally, we have implemented or are aware of various congestion control algorithm implementations ([bbr](https://github.com/ccp-project/bbr), [copa](https://github.com/venkatarun95/ccp_copa), [nimbus](https://github.com/ccp-project/nimbus) and more). If you have implemented a congestion control algorithm using CCP, or are considering doing so, please get in touch with us!

### Fidelity

CCP algorithms perform similarly to their in-datapath counterparts. While we evaluate this more thoroughly in the paper, it it simplest to view this demo, comparing the Linux kernel implementation of BBR:

<iframe width="560" height="315" src="https://www.youtube.com/embed/uXsN3Fe39dk" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

The CCP implementation of BBR performs similarly: 

<iframe width="560" height="315" src="https://www.youtube.com/embed/ORBn5CvP0lk" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

### Reproduce Our Results

Should you wish to replicate the experiments in our paper, [this set of scripts](https://github.com/ccp-project/eval-scripts) may be useful.
