
tags: #ccde #transport #ethernet \
domain: 02-Transport Technologies \
subdomain: Ethernet protocol \
related: [] \
status: draft \
date created: 2026-02-15 16:15 \
last modified: 2026-02-15 16:19 


## Overview
An Ethernet frame is a Layer 2 transport mechanism that allows for the switching of data in a L2 network using Media Access Control (MAC) addresses. Ethernet frames are designed to be transmitted using Ethernet physical media. This media may support the encapsulation of higher level protocols (like copper or fiber) or may not (like coaxial cable).

The first Ethernet network was created in 1980 at Xerox Parc to connect a computer to a laser printer (at the time, it was called a DIX frame, short for Digital, Intel, and Xerox). It was later standardised by the IEEE in the 802.3 standard for Carrier Sense Multiple Access for Collision Detection (CSMA/CD). 

Ethernet frames are the standard transmission protocol for Layer 2 networks. Importantly, Ethernet frames are capable of encapsulating higher level protocols (such as Internet Protocol (IP) packets) for transmission.

## What Problem Does This Solve 

Ethernet was designed to provide a cheap, scalable and multi-vendor transport layer technology. Prior to the creation of Ethernet, networks either required:

	a) Point-to-point links between every network device, and/or;
	b) Proprietary vendor technologies (like IBM Token Ring)

Ethernet won out against other transport technologies due to consistent increases in speed & economies of scale. Ethernet solves a set of fundamental problems:

	a) How can I reliably forward data from one host to another within the same L2 broadcast domain?
	b) How can I send higher-level protocols (ie. IP, TCP) within a L2 frame?
	c) How can I logically separate and join physical links interconnecting different broadcast domains?
## How It Works 

Ethernet networks communicate data between hosts using *frames*, which is a standardised data format used to transport data across L2 networks. There are multiple different types of Ethernet frames. The format of the most common Ethernet frame (Ethernet II/DIX) is shown below:

![[Pasted image 20260218052232.png]]

The standard Ethernet frame is 1518 bytes. The actual size of the Ethernet frame will depend on the data sent within the frame. Some additional features of the Ethernet protocol (802.1Q, QinQ) require additional information to be transmitted within the frame and therefore are slightly larger than a typical Ethernet frame.

The Ethernet preamble is an alternating sequence of bits (01010101) that signals its own conclusion. The reason for this sequence is to synchronize the receiver's clock edge to the incoming signal. The preamble ensures that any oscillation between the sender and receiver's transmissions are accounted for by the receiver's Phased Lock-Loop (PLL).
## Design Considerations 
## Scalability 
## Resilience and Redundancy 
## Performance Characteristics 
## Operational Complexity 
## Cost Factors 
## When to Use / When to Avoid 

**Use when:** - 

**Avoid when:** - 

## Design Trade-offs 

## Links 

#### RFCs

#### CVDs

#### Books


