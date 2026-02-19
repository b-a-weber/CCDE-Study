
tags: #ccde #transport #ethernet \
domain: 02-Transport Technologies \
subdomain: Ethernet protocol \
related: [] \
status: draft \
date created: 2026-02-15 16:15 \
last modified: 2026-02-15 16:19 


## Overview
An Ethernet frame is a Layer 2 transport mechanism that allows for the switching of data in a L2 network using Media Access Control (MAC) addresses. Ethernet frames are designed to be transmitted using Ethernet physical media. 

The first Ethernet network was created in 1980 at Xerox Parc to connect a computer to a laser printer (at the time, it was called a DIX frame, short for Digital, Intel, and Xerox). It was later standardised by the IEEE in the 802.3 standard for Carrier Sense Multiple Access for Collision Detection (CSMA/CD). CSMA/CD was designed to prevent both hosts attempting to transmit a frame over a single half-duplex link at the same time (which would cause a collision). Hosts listen to the network to see if the link is free, and if it is then they attempt to forward their frames. If they detect a collision, they stop forwarding and transmit a jam signal to let other devices know of the collision. CSMA/CD is not required in full-duplex networks, as each host as its own dedicated channel, thereby removing the risk of multiple devices transmitting over the same channel at the same time.

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

| Preamble | SFD    | Destination MAC | Source MAC | EtherType | Payload       | FCS     |
| -------- | ------ | --------------- | ---------- | --------- | ------------- | ------- |
| 7 bytes  | 1 byte | 6 bytes         | 6 bytes    | 2 bytes   | 46-1500 bytes | 4 bytes |

The standard Ethernet frame is 1518 bytes. The actual size of the Ethernet frame will depend on the data sent within the frame. Some additional features of the Ethernet protocol (802.1Q, QinQ) require additional information to be transmitted within the frame and therefore are slightly larger than a typical Ethernet frame.

The Ethernet preamble is an 7 byte alternating sequence of bits (01010101). The reason for this sequence is to synchronize the receiver's clock edge to the incoming signal. The preamble ensures that any oscillation between the sender and receiver's transmissions are accounted for by the receiver's Phased Lock-Loop (PLL). The preamble is concluded by the transmission of the Start Frame Delimeter, which is a 1 byte sequence of data that ends in two positive bits (e.g. 1011). The SFD component of an Ethernet frame signals to the receiver that the content of the frame will be sent next.

The next two fields of an Ethernet frame are the Destination Address (DA) and the Source Address (SA). Addresses in Ethernet frames are represented as MAC addresses, which are a 6 byte value represented as six hexadecimal pairs (e.g. 0F:6E:F9:54:A1:AA). The first 3 bytes of a MAC address identify the organisation that the NIC comes from - this is known as the Organisationally Unique Identifier (OUI). The first bit of the OUI signals whether the frame uses multicast (known as the LSB). The second bit of the OUI identifies whether the OUI has been manually set (say, in the case of a VM) or set by the issuing organisation. The second 3 bytes of the MAC address are specific to the NIC and are used to uniquely identify different devices created by the same manufacturer.

The DA is sent before the SA to allow for processing efficiencies. If the DA does not match the NIC's MAC address, then the frame can be dropped. If it does match the MAC address, then a new entry is added to the Content Addressable Memory (CAM) table to allow frames to be forwarded. In modern switches with purpose-built ASICs, switching is done at line rate using the CAM table without punting frames to the CPU. CAM is closely coupled with the ASIC - when data is provided to a switch's CAM, the comparison against existing MAC entries is done in parallel. Each row of the CAM table performs a simultaneous check against the data provided to the switch in a single clock cycle, with the appropriate match returning the switch port to forward the frame from. 

The next field in the frame is the EtherType field, which identifies which higher-level protocol is encapsulated within the Ethernet frame. EtherType is a 2 byte field typically represented as 4 hexadecimal values (i.e. 0x0800 for IP) that is used to communicate to the receiver the protocol used to format the data located within the Payload field of an Ethernet frame. Some common EtherType values are:

	- 0x8000: Ethernet
	- 0x0800: IPv4
	- 0x0806: Address Resolution Protocol (ARP)
	- 0x86DD: IPv6
	- 0x8100: 802.1q (VLAN Tagging)
	- 0x8847: MPLS Unicast
	- 0x8848: MPLS Multicast
	- 0x88CC: LLDP
	- 0x8906: Fibre Channel over Ethernet (FCoE)

The Payload field contains the data that is intended for transport over the Ethernet network. The Payload is opaque to Ethernet - Ethernet is just the transport for the encapsulated Payload. Payload data will frequently have its own headers and reliability measures to ensure the Payload is correctly received and interpreted by the receiver. The Maximum Transmission Unit (MTU) comes from the byte range used by the Payload field for Ethernet frames (46 - 1500 bytes), though this can be expanded if necessary (in the case of jumbo frames, which have an MTU of 9100 bytes). If jumbo frames are required, they need to be configured across every hop in the network, otherwise the frame will either be fragmented (if the DF bit is not set) or dropped (if the DF bit is set).

The Ethernet frame concludes with the Frame Check Sequence (FCS) field. The FCS field is used for error detection - the frame is evaluated by the CRC-32 value to generate a 4 byte representation of the frame. The receiver runs the same computation on the received frame and evaluates their value against the sender's FCS. If the FCS does not match, the Ethernet frame is dropped. There is no error correction or in-built reliability mechanism into FCS - if there is no match, the frame is dropped.

When 802.1q tagging is used to trunk VLANs across a link, an additional field is added to the 802.3 Ethernet frame. 

| Preamble | SFD    | Destination MAC | Source MAC | ==802.1Q Tag== | EtherType | Payload       | FCS     |
| -------- | ------ | --------------- | ---------- | ---------- | --------- | ------------- | ------- |
| 7 bytes  | 1 byte | 6 bytes         | 6 bytes    | ==4 bytes==    | 2 bytes   | 46-1500 bytes | 4 bytes |

The 802.1q tag consists of 4 components. 
The Type ID (TPID) field is 16 bits and is used to identify the frame as an 802.1q tagged frame. Consider it similar to the EtherType field in the traditional Ethernet II frame, except that it is used specifically to signal that VLAN information will be transmitted in the next 2 bytes (instead of the Payload). 
The Type Control Information (TCI) field consists of three sub-fields. Firstly, the 3-bit Priority Code Point (PCP) value is used to convey Class of Service (CoS) information regarding the frame's priority level. The 1-bit Drop Eligible Indicator (DEI) value is used to signal whether the frame can be dropped if the link is congested. The 12-bit VLAN ID (VID) value specifies the VLAN that the frame belongs to. 
Frames may also hold two 802.1q tags, known as double tagging or QinQ (802.1ad). This is primarily used by ISPs to allow both customer and ISP to tag their traffic. 




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


