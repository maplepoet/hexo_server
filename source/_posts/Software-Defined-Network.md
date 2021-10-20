---
title: Software Defined Network
date: 2021-08-17 11:08:52
tags:
---

### 1. Preliminaries

### 2. Introduction and overall picture

#### 2.1) Networking Technologies

Modern Networking Ecosystem

1. Users & Services Providers
2. Quality of Service: Measurable traffic characteristics
3. Quality of Experience: Subjective measures that may depend on traffic characteristics
4. subject to network provider and network access facilities

IP backbone: Provide connectivity to external networks and users

Edge Router/Aggregation Routers: At the periphery of an IP backbone

- Example: WAN with MPLS、Ethernet LAN...

**Network** Layout

​    Three tier-hierarchy: Access Network、Distribution Network、Core Network

​    Task: Local devices can access the network; interconnects multiple access networks; Aggregate traffic to the core network; 

Interconnection of geographically dispersed distribution networks.

**Intermediate Systems(IS): routers | Switches**

1. Routing: A successive exchange of connectivity information between routers.
2. Forwarding: A switch or router-local process which forwards packets towards the destination using the information given in the local routing table.
3. Queueing/ Buffering: Policies to discard/prioritize packets.

**Problems** of traditional networking infrastructure:

- Limited Flexibility
- Increasing Switch/Router complexity
- Separation of network and application

**Broadband Network Gateway (BNG)** 宽带网络业务网关

​    Point-to-point over Ethernet(PPPoE) between customer and BNG; Authentication and Authorization; Rate Limiting and Traffic Shaping; Packet Forwarding; Accounting; One BNG handles many thousands of customers.

​    Problems: Very specialized key functionality: Power hungry and costly

​    Idea: Shape the network, determine routes of packets

​        Increase resource usage and performance; Improve Predictability of performance; Benefit from network operations

​        Publish/Subscribe, reduces number of msg; faster filtering in hardware; shorter paths

#### 2.2) Internet Architecture

#### 2,3) Networking Elements

#### 2.4) A modern networking perspective

**SDN**, a new network architecture and paradigm

 that's makes it easier to program networks. with the core idea that software remotely controls network hardware.
 Benefits, leverages increased flexibility

- easy modification of the network control logic
- api to program the network
- high-level programming languages
- reduced switch complexity
- integrated system: application & network
- reduce the complexity of implementing control logic

 **Network OS(NOS)**: distributed system that creates a consistent, up-to-date network view

​     get state information from forwarding elements

​     give control directives to forwarding elements

 **OpenFlow**: remotely controlling the forwarding table of a switch or router

 **Architecture: Control Plane & Data Plane**

​     <u>Control Plane</u>: Defines routes, manages network graph

​         computes the forwarding rules for the Data Plane and installs them in the Data Plane. A new computation can be triggered by an event.

- **Fast Path**: the Data Plane typically has a very high throughput and is realized by special Chips.

- **Slow Path**: The Control Plane and the forwarding of packets to it is much slower. It is only used to install new flows once and to update them. 

​     <u>Data Plane</u>: Forwarding of packets

​         a packet enters a switch/route and be forwarded to an outgoing port of the device.

- **Match**: Based on fields of a packet a table lookup can be performed.

- **Action**: A action is performed in consequence of a match. 
  ​         set_egress_port

### 3. Foundations of software-defined networking(SDN)

**Flow-based forwarding**

​    any information of a packet that identifies a communication relation

​    fine-grained forwarding of selected flows or coarse-grained aggregation

**Distributed Routing vs. Logically Centralized Routing**

​    Distributed Routing

​        need time to converge to optimum \rightarrow lower resource utilization; Complex

​    Logically Centralized Routing

​        Greatly simplifies implementation of control logic; global view can increase performance of control; Faster Convergence $\rightarrow$ higher resource utilization

​        Physical distribution ensures high availability and scalability

​        makes implementation of control logic simpler and controller hard.

##### **Components**

######     Switches/Routers

- ​       implement data plane: packet forwarding

- ​        Typically multi-layer switches, forwarding based on layer 2-4 headers.

- ​        Hardware Switches: Fast matching, Ternary Content-addressable Memory (TCAM)

- ​        Software Switches: Connect multiple virtual machines to physical interface of host

- ​        Hybrid Switches, SDN & standard L2/L3 forwarding


######     SDN Controller

- ​        implement control plane

- ​        implement southbound interface to switches: Configuration of forwarding tables; Injecting Packets; Events from switch; Collection of traffic statistics; Discovery of topology

- ​        interfaces with control logic via northbound interface


######     Control Logic

​        routes of flows: *Proactive and reactive routing*

Distribution, physically distributed; logically centralization (transparent to control logic)

- Replication, OpenFlow standard allows for connections from switch to multiple controllers
- Partitioning, support large-scale deployment, raises issues similar to P2P; knowledge about neigborhood; Coordination and consistency
- Goal: deal with failures, increase scalability

#### 3.1) Southbound Interface (OpenFlow)

This is the interface between the SDN controller and the physical network devices. The OpenFlow protocol is an instantiation of a southbound API. It is the protocol that is used for the configuration of the network devices as well as the signaling between the controller and the network devices. (背)

**Functionality**: Modification of Flow tables; inject packets; Events for receiving packets(Reactive routing); Querying traffic statistics(Counters)

##### **Architecture**

​    Flow Tables & Flow Entries, tables consist of a list of flow entries

​        Flow Entry: Match Field, Priority, Counters, Instructions, Timeouts

​            Match Fields: 10 tuples, ingress port, Ether src/dst, VLAN Id, VLAN type, IP src/dst, ip proto(6=TCP,17=UDP,1=ICMP), TCP/UDP src/dst port

​            Wildcard Matching, hardware switches(fast matching), 2 types of CAM

​                1. Binary CAM, good for exact match

​                2. Ternary CAM, implementation of longest prefix match on IP addresses

​                缺点: Consumes significant energy, silicon space.

​    Table-Miss Flow entry(dynamic routing), lowest priority, matches all packets; Drop(OF1.3+) or Send to controller

​    **Actions**

1. output, output packet on the specified port
   - ALL: Forwards the packet to all interfaces with exception of the incoming interface. Forwarding loops can occur and packets might circulate forever.
   - Flood: Forwards the packet to all output ports which belong to a minimum spanning tree (computed previously). By that, endless packet circulation can be avoided.

2. TTL modifications, decrement TTL, copy TTL outwards/inwards
3. Push and Pop tags, add/remove VLAN/MPLS/PBB tags to/from the packet
4. Set header fields
5. Group actions: Multicast
6. order of execution of actions

​    **Duplicate**

​        Instructions, Action application, action list with multiple packet_out actions.

​            Write actions; go to table with given id; apply specific actions immediately; clear action set; meter id

​            Pro: Action application, action list with multiple packet_out actions.

​            Con: Number of allowed packet_out actions per action list is not known to the controller. For updating a duplication rule, the flow rule has to be modified.

​        Group Table, group type all

​            Pro: Created exactly for this purpose, fine grained control over the duplication process through group buckets, can be updated without modifying flow tables.

​        packet_out to flood port 

​            No control, can only be used for a very specific use case: if a packet should be sent out to all ports.

##### **Proactive vs. Reactive Routing**

1. Proactive, before the flow starts, controller proactively pushes flow table entries onto switches
   - pro: reduce controller load
   - con: occupy space in flow table of switch

2. Reactive, as soon as the flow starts, switch receives a packet without matching flow table entry, control logic calculate route, controller installs flow table entries along path
   - Pro: saves flow table space
   - con: Puts load onto controller and control network

​    Required Information(dynamic)

​        Network Topology: switches and hosts, links btw switches/hosts and switches, bandwidth of links

​            Controller implements standard protocols: LLDP, ARP handling for host discovery

​            Controller exposes discovered information to control logic

​        Traffic Statistics: # of packets on bytes, # of dropped packets, receive/transmit errors, per flow, link/port, group
​            Flow, Port, Table, Queue, Group, Meter

​        Counters

#### 3.2) SDN Controller

**Northbound**(Program and API), **Southbound**(Interact with Data Plane), **East/West**(other controllers)

Issues: Routing; Notification manager; Security mechanisms; Topology manager; Statistics manager; Device manager

Network Operating system(NOS), provides essential services, common application programming interfaces, abstraction of lower layer elements to developers

​    Pro: The network topology does not need to be discovered (as required for a distributed implementation) before focusing on the actual implementation of the policies. Using SDN with a NOS, the topology information is already available from the NOS and policies can directly be applied based on this information.

High-Level Architecture of SDN

​    1. Application support, API for SDN applications, access network information and program application-specific network behavior

​    2. Orchestration, automated control and management of resources; coordination of application requests; topology

​    3. Abstraction, management; capabilities; independent of underlying infrastructure

##### Example:

######     NOX

- Granularity： Scalability vs. Flexibility
- Scalability: different time scales; consistent network view
- Event driven update of control logic
- example: Policy lookup
- Not solve: Reliability and robustness, Generality

######     ONIX(closed source)

​        Goals: Generality, Scalability, Reliability, simplicity, Performance

​        scalability, flexible state distribution mechanisms; flexible scaling and reliability mechanisms

​            Workload Balancing

​            Partitioning of Flow Space

​            different Consistency models

​                Strong Consistency: system with replication exhibits identical behavior to system without replication

​                    算法: Consensus、Transactional Processing

​                    Consensus, a key mechanism for strong consistency

​                        Validity、Agreement、Termination、Integrity

​                        Process order by rank

​                        Uniform consensus

​                Eventual Consistency: All replicas will eventually converge into a mutually consistent state. read operation return items that result from valid write operations.

​                    算法: Gossiping

######     OpenDaylight

​    BGP with OpenFlow, exchange with AS1 and AS3: reachability, flow update, capability

**East/Westbound Interface**

​    This is the interface between different controllers. Just as the north-bound API, currently, the east-/westbound is more like an abstract concept, with many different possibilities to implement it. The idea is that this interface is used, e.g., for inter-domain coordination, route exchanges, etc. (背)

#### 3.3) Northbound Interface

This is the interface between the controller and any type of network application running on top of it. This interface is very hard to define in a precise manner as any kind of network applications should be supported. Therefore, for now, the northbound interface is more of a high-level concept and not yet well-defined. （背）

##### REST (resource-oriented)

- ​    Con: event not supported, HTTP based on request/response paradigm, restricted to proactive routing.

- ​    Unique identifiers (URI, uniform resource Identifier), C/S architecture(server->manage resource; client->manipulate resource)

- ​    Get, Post, Put, Delete, Head, Options; stateless protocol between client and server

- ​    example: OpenDaylight、Ryu 


##### Java/OSGi Interface of OpenDaylight, Full power of OpenFlow

- ​    event interface(callback functions), service for sending packets, both reactive&proactive routing

- ​    OSGi(开放服务网关计划Open Services Gateway Initiative), allows for add&remove at runtime; no restart of SDN controller
  - Service: Data Packet Service; Data Packet Listener; Flow Programmer Service; Switch Manager

#### 3.4) SDN Abstractions

An abstraction layer is a mechanism that translates a high-level request into the low-level commands required to perform the request

​    Shields the implementation details of a lower level of abstraction from software at a higher level

​    Represents the basic properties or characteristics of network entities in such a way network programs can focus on the desired functionality without having to program the detailed actions

**Three Basic Network Interfaces**:

​    Forwarding, shield higher layers from forwarding

​    Distribution, shield higher layers from state dissemination/collection

​    Specification, shield control program from details of physical network

Example of Abstractions

​    Network

​        Single network view, authentication with respect to a network. consistent model provides # of connected devices.

​        Full network view, exhibits # of connected devices + # of switching elements + topology information

​    Pipeline, automatically installed in the network at multiple switching devices

##### **SDN Languages**

​    $\star$ **Frenetic/Pyretic Design** (Key concept: Modularity) 

​        Modularity: Slicing enables different apps to work on different parts of the network without influencing each other
​        Types of Composition: Sequential; Parallel

​        Packets: 

​        Policy: flood, drop. 

​            Dynamic: sequence of static policies, important to adapt the network

​            Static: snapchat of a network's global forwarding behavior;

​                Actions: drop, passthrough, fwd(port), flood, push/pop/move

​                Predicate: all_packets,  no_packets, ingress, egress, match(h=v)

​                Query Policies: packets(limit, [h]), counts(every,[h])

​                Policies: Actions | Query| Predicate[Policies] | (C|C) | C >> C | if (P,C,C)

​    $\star$ **P4**, an open source language allowing the specification of packet processing logic

​        Goal: Reconfigurability, Protocol independence, target independence

​        Basics: build on a protocol independent switch architecture, maps specification to physical resources.

​        P4_14 (simple reference pipeline、easy to start、better support)

​        P4_16(more general、allow special functions、more complex)

​        **Benefits:** 

​            define suitable data plane behavior: flexible usage of header fields; flexible definition of actions

​            Better usage of resources: limited capacity in TCAM

​            Performance gains: Time packet in processing, energy/delay

​            integrated and combined with existing NOS and SDN controller framworks

​        **Expressions**

​            Header

​            Control

​                Table (包括 key、actions)

​                Actions

​                使用apply{ }

​            在terminal使用match和action （Flow entry）来运行

​                default, action=drop(); //example

​                default, action = set_has_no_vlan();

##### SDN Applications

Pyretic Applications:

​    ARP responder、firewalls、Gateways、Load Balancers、Monitoring、Big Switch、Spanning Tree

### 4. Network Function Virtualization(NFV)

#### 4.1) Architectural Overview NFV

Goal: improved CAPEX、OPEX; Faster deployment; Flexible management

Use Case from ETSI: VNFaaS; Mobile Core/IMS functionality, mobile base stations; Virtualized home environment; Virtualize content distribution networks

Infrastructure (NFVI), support deployment and execution of VNF, Totality of the hardware and software components which build up the environment in which VNFs are deployed; leverage existing virtualization technology from computing.

NFVI executes NFV, separate description necessary.

Big Picture Architecture

ETSI, European Telecommunication Standards Institute

​    Compute Domain, computational and storage components, typically commodity off-the-shelf

​    Hypervisor管理程序 Domain, provide an abstract machine to VNFs and management/orchestration functions

​    Infrastructure Network Domain, provide logical connectivity between components of VNF, different VNFs and VNFs and their context.

​        Infrastructure Network provide addressing scheme, routing capabilities and resource management

​            Common Header; Address binding; Transparent encapsulation

Problems:

​    Model dependencies of many VNFs; Placement; Deployment; Scaling

IETF(Internet Engineering Task Force): Service Function Chaining

#### 4.2) Performance of NFs

VNFs are executed on standard hardware; Host environment builds on OperatingSystem/Hypervisor: Read packets from Network Interface Card (NIC)

User Space: Network Function; Kernel Space: Packets Received

Goal: Understand Efficiency of NetworkFunction, Limiting Factors

Operating System Packet Processing

​    Traditional Networking

​        NIC (network  interface card) uses DMA to copy data into kernel buffer; interrupt when packets arrive; copy packet data from kernel space to user space; use system call to send data from user space

​    User Space Packet Processing (allow user space apps to directly access packet data)

​        NIC uses DMA to copy data into kernel user space buffer; interrupt use polling to find when packets arrive; copy packet data from kernel space to user space; use system regular function call to send data from user space

DPDK(Data Plane Development Kit), high performance I/O library, poll more driver reads packets from NIC

​    network Interrupt: NIC notifies OS about new packets, OS interrupts its current job to receive packet.
​        interrupt can arrive during critical sections; interrupts can be delivered to the wrong CPU core; pay context switch cost.
​        Pro: A job is only - and immediately - started if a packet arrives
​        Situation: Low Network Load; Few Processor cores;

​    Polling: Driver asks frequently
​        Pro: Current jobs(i.e.packet modifications) are not interrupted upon a packet reception, and can be completed before the next job (packet) arrives. 
​        Situation: High, constant network load

​    Kernel-User Overhead
​        Reads packets into kernel memory; Stack pulls data out of packets; Data is copied into user space for application; Application uses system calls to interface with OS

​    Kernel Bypass

​        Kernel only sets up basic access to NIC; User-space driver tells NIC to DMA data directly into user-space memory
​        No extra copies; No in-kernel processing; No context switching

​    CPU Core Affinity

​    Paging Overhead

​    Locks

​        Thread synchronization is expensive: Tens of nanoseconds to take an uncontested lock; 10Gbps -> 68ns per packet

​        Producer/Consumer architecture: Gather packets from NIC (producer) and ask worker to process them (consumer)

​        Lock-free communication: Ring-buffer based message queues

​    Bulk Operations, DPDK helps batch requests into bulk operations

​        PCIe bus uses messaging protocols for CPU to interact with devices (NICs)

​    Limitations:

​        Barebones准系统 I/O library, no protocol processing

​        Focused on running a single NF, need to specially design NFs to be able to run multiple functions at once; NF monopolizes entire NIC port

​        Extensive use of polling means high CPU usage

​        No management layer (addressing, reliability, auto scaling, fault tolerance)

Service Chaining (Chain together functionality to build more complex services), move packets through chain efficiently

​    OpenNetVM

​        Zero Copy I/O: DMA into shared memory, directly accessible to NFs.

​        Architecture: DPDK, VNFs, Service Chains, NF manager, Shared Memory.

Efficiency vs. Modularity

​    VNFs must be modular and easy to compose, isolated processes; simple development, deployment and chaining

​    Break abstractions when necessary, bypass OS and network stack to get raw packets. shared memory between processes for faster chains.

​    Balance control across the hierarchy, similar SDN 

**Summary:**

​     **SDN: a centralized network control plane**

​    **NFV: efficient software data plane**

​    **DPDK: high performance user-space network I/O**

​    **OpenNetVM: service chaining and NF management**

#### 4.3) Foundations & Algorithms for NFV

Facility Location (reduce Round Trip Time)

​    Uncapacitated、Global: GreedyFL

​        Client connects if Budgets $\geq$ Connection cost and Facility is open

​        Facility opens if Sum of Clients' Contribution $\geq$ facility opening cost and Clients' Contribution = max(0, budget-distance)

​    Capacitated、Local: GreedyFL^L

​    Multi-commodity:  different products have to be handled by the facilities

Routing

​    Routing problem: in a graph, find paths that minimize some cost metric

​    Flow problem: move stream of data along a network from a source to a sink

​    Splittable vs. unsplittable flows

​    Single vs. Multi-commodity flows

​        single: 

​        Multi-Commodity：

Network Embedding

​    Fixed Networks

​    Malleable networks: Template embedding

### 5. Deep Technical Chapters

**DTD**: Data Plane Hardware Acceleration

​    Flow(Rule) Matching

​    Physical Memory Technologies

​        DRAM, off-chip - close to CPU/ASIC

​            tables * ns * lookups 

​            slow, PC/laptop memory

​            1 transistor + 1 capacitor, require refresh

​        SRAM, on-chip - integrated in switching ASIC

​            fast, small, processor cache

​            6 transistors, require no refresh, small capacity

#####     **Matching Algo.**

######         *Problems*

​            exact (Cuckoo hashing, TRIE)

​            longest prefix match (Cuckoo hashing, TRIE, TCAM)

​            ternary (TCAM)

​            range (between)

######         *Lookup Approach*

​            Linear Search O(n)

​            Simple Prefix Tree, TRIE O(log(n))

​                Compact Prefix Tree

​            Hashing based mechanism

​                Multi-way Cuckoo Hashing

​            CAM 最差O(n) 最好O(1)

​                **TCAM**, search all table entries in parallel, TCAM is SRAM-cell based but no SRAM memory

​                    Lookup(priority is important); IP lpm match (prefix address part as matcher; prefix length as priority)

​                **Comparison:**

​                OpenFlow increases the flexibility of TCAM usage

​                    if TCAM is full, add OpenFlow matcher slow path; refuse additional rules.

​    Existing OpenFlow Hardware

### 6. Guest Lectures

Attach
 OP1.0: 12 fields(Ethernet, TCP/IPv4)
 OP1.1: 15 fields(MPLS, inter-table metadata)
 OP1.2: 36 fields(ARP, ICMP, IPv6, etc)
 OP1.3: 40 fields
 OP1.4: 41 fields