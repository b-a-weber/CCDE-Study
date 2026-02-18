
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

Ethernet networks communicate data between hosts using *frames*, which is a standardised data format used to transport data across L2 networks. There are multiple different types of Ethernet frames. The format of the most common Ethernet frame (Ethernet II/DIX) is shown below.

![](Pasted%20image%2020260219054204.png)

The standard Ethernet frame is 1518 bytes. The actual size of the Ethernet frame will depend on the data sent within the frame. Some additional features of the Ethernet protocol (802.1Q, QinQ) require additional information to be transmitted within the frame and therefore are slightly larger than a typical Ethernet frame.

The Ethernet preamble is an 7 byte alternating sequence of bits (01010101). The reason for this sequence is to synchronize the receiver's clock edge to the incoming signal. The preamble ensures that any oscillation between the sender and receiver's transmissions are accounted for by the receiver's Phased Lock-Loop (PLL). The preamble is concluded by the transmission of the Start Frame Delimeter, which is a 1 byte sequence of data that ends in two positive bits (e.g. 1011). The SFD component of an Ethernet frame signals to the receiver that the content of the frame will be sent next.

The next two components of an Ethernet frame are the Destination Address (DA) and the Source Address (SA). Addresses in Ethernet frames are represented as MAC addresses, which are a 6 byte value represented as six hexadecimal pairs (e.g. 0F:6E:F9:54:A1:AA). The first 3 bytes of a MAC address identify the organisation that the NIC comes from - this is known as the Organisationally Unique Identifier (OUI). The first bit of the OUI signals whether the frame uses multicast (known as the LSB). The second bit of the OUI identifies whether the OUI has been manually set (say, in the case of a VM) or set by the issuing organisation. The second 3 bytes of the MAC address are specific to the NIC and are used to uniquely identify different devices created by the same manufacturer.

The DA is sent before the SA to allow for processing efficiencies. If the DA does not match the NIC's MAC address, then the frame can be dropped. If it does match the MAC address, then a new entry is added to the Content Addressable Memory (CAM) table to allow frames to be forwarded. In modern switches with purpose-built ASICs, switching is done at line rate using the CAM table without punting frames to the CPU. CAM is closely coupled with the ASIC - when data is provided to a switch's CAM, the comparison against existing MAC entries is done in parallel. Each row of the CAM table performs a simultaneous check against the data provided to the switch in a single clock cycle, with the appropriate match returning the switch port to forward the frame from. 




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


