
tags: #ccde #transport #ethernet \
domain: 02-Transport Technologies \
subdomain: Ethernet protocol \
related: [] \
status: draft \
date created: 2026-02-15 16:15 \
last modified: 2026-02-15 16:19 


## Overview
An Ethernet frame is a Layer 2 transport protocol that allows for the switching of data in a L2 network using Media Access Control (MAC) addresses. Ethernet frames are designed to be transmitted using Ethernet physical media (this media may support the encapsulation of higher level protocols, like copper or fiber optic cables, or may not, such as coaxial cable).

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


