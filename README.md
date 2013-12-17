Lecture 1 - Switches
================

Switch forwarding has 2 types

1. Store-and-forward
	- Complete frame is received before forwarding

2. Cut-through
	- Frame is forwarded therefore the entire frame is received


### Store-and-forward

Stores frame in a buffer
Performs error checking using FCS

	If fail -> discard frame (reduces amount of bandwidth consumed by corrupt data)

	if pass -> looks up destination MAC address in switching table
	forwards frame to the right interface

high latency - because reason.
high integrity ? Store and forward is required if QoS is important


### Cut-through Fast Forward

Buffers just enough of the frame to read destination MAC and forwards it.

Low latency - does not perform error checking (UDP of the 2nd layer world :D)

Low integrity - corrupted data consumes bandwidth so destination NIC discards them instead of switch


### Cut-through Fragment Free

- Stores first 64 bytes of frame
- Most collisions occur in first 64 bytes apparently
- Performs check on first 64 bytes to ensure no collision
- It's a compromise between High Latency and High integrity

Going deeper to the switch ports (interfaces), there's Memory Buffering.

1. Port-based memory

- frames are stored in queues that are linked to specific incoming ports
- a frame is only transmitted to the outgoing port only when all frames ahead of it is successfully transmitted

2. Shared memory

- Common memory buffer all the ports on the switch share.
- Queues dynamically linked to destination ports


## 

Lecture 2 - VLANs
=================

#### VLAN benefits:

1. Security

	- Separated networks between VLANs

2. Performance

	- Reduces unncessary traffic between VLANs

3. Broadcast storm mitigation

	- Reduces unncessary broadcast traffics

4. Improved IT staff efficiency

	- Easier to manage network
	- Control network traffic via ACL

#### VLAN Trunking / Tagging

- Carrying data from more than one VLANs in one single physical link
- frames are tagged from which VLAN when entering a trunk port

Theres the Native VLAN and then there's the Management VLAN.

Native VLAN sets which VLAN should untagged traffic go to.

Management VLAN is configured as long as u assign an IP address to it.
Default is VLAN 1 for Management VLAN.

Then there's the voice VLAN. Dedicated for QoS :P

## 


Lecture 3 -Trunking
===================

### Dynamic Trunking Protocol (DTP)

Modes of DTP

1. On (default)
	- sends DTP advertisements

2. Dynamic Auto
	- Sends DTP frames to the remote port. Advertises that it is able to trunk but doesn't request to go to trunking state.
	- If both is Auto then it wont be in trunking state.

3. Dynamic Desirable
	- DTP frames are sent periodically.
	- Advertises that it is able to trunk AND ask the remote switch port to go to the trunking state.

4. Turn off DTP
	- unconditional trunking state


### VLAN Trunking Protocol (VTP)

VLAN changes on one switch to be propagated to other switching via VTP

3 modes for VTP

1. Server
	- advertise the VTP VLAN info
	- create, delete, rename VLANs
2. Client
	- forwards advertisements to other client
	- gets updated VTP message from servers and udpates its database
3. Transparent
	- Forwards VTP advertisements but ignore information contained in the message


#### Inter-VLAN routing methods:

1. Traditional
2. Router-on-a-stick
	- subinterfaces
3. Switch based
	- Using layer 3 switch
	- Uses Switch Virtual Interfaces (SVI) to retag the frame

## 


Lecture 4 - Access Control List (ACL)
===========================

- Routers are stateless
- Packets are treated independently

ACL = If-then-else tree
either deny or permit packet to pass

Unique ACL can be applied:
On each router interface
In each direction (outbound / inbound)

Cisco default ACL rule is DENY :(

Standard ACL vs Extended ACL
Standard ACL is placed closest to the destination.
Extended ACL is placed closest to source


## 


Lecture 5 - LAN Design
=================

### 3 layers:
- Access
- Distribution
- Core

Access layer is for end devices.
Distribution layer aggregates the data from access layer to core.
Core is the high-speed backbone.

Benefits of a hierarchical network

1. Scalability
	- Can be expanded easily
2. Redundancy
	- Ensure path availability
3. Performance
	- Link aggregation between levels and high-performance core and distribution level switches allow for near wire-speed throughout the network
4. Security
	- Port security at the access level and policies at the distribution level make the network more secure.
5. Manageability
	- Consistency between switches makes management more simple
6. Maintainability
	- The modularity of the hierarchical design allows for the network to scale without becoming overly complicated.


## Design Principles

1. Network Diameter

  - Number of devices a packet has to cross before it reaches its destination
  - Keeping it low ensures low and predictable latency

2. Bandwidth Aggregation

  - Parcitce of considering specific bandwidth req. of each part of the hierarchy.

3. Redundancy


What is converged network?

Convergence is the process of combining voice and video communications on a data network.
Using a properly designed hierarchical network and implementing QoS policies that can prioritize the audio and video, they can be converged onto the data network with little to no impact on quality of service.


### 


Lecture 6 - Spanning Tree Protocol (STP)
========================================

Why do we need STP?

When we implement redundancy on our network design, layer 2 loops can happen.
Frames do not have TTL like Packet does.

STP ensures that there is only one logical path between all destination by intentionally blocking redundant paths that could cause a loop (loop-free path)

STP uses Spanning Tree Algorithm (STA) to determine which switch ports on a network need to be blocked.

STA designates a single switch as the root bridge and use it as a refernce point for all STA calculations.

3 steps of STA:

1. Elect one root bridge
2. Select the root port on all non-root bridges
3. Select the designated and non-designated ports

Bridge ID (BID) is used to identify each bridge/switch.
Switch with the lowest BID becomes the root bridge.

BID consists of:

1. 2-byte bridge priority
2. 6-byte MAC address

If all devices have the same priority, MAC address takes into account.

### Port states
1. Blocking - non-designated port and does not forward frames but participates in STP
2. Listening - Can participate in frame forwarding depending on STP. Doesn't forward frames.
3. Learning - Prepares to participate in frame forwarding
4. Forwarding - Forwards frame
5. Disabled - Doesn't participate in STP and does not forward frames.


STP Convergence:

- Time taken for root bridge to be elected
- setting port roles to eliminate loops. (root / designated / non-designated ports)

### Algorhyme
    I think that I shall never see
    A graph as lovely as a tree.
    A tree which must be sure to span.
    So packets can reach every LAN.
    First the root must be selected.
    By ID, it is elected.
    Least cost paths from Root are traced.
    In the tree these paths are placed.
    A mesh is made by folks like me.
    Then bridges find a spanning tree.

## 

Lecture 7 - Wireless Concepts
=============================


Wireless Technologies:
1. PAN (Personal Area Network)
2. LAN (Local Area Network)
3. MAN (Metropolitan Area Network)
4. WAN (Wide Area Network)

### PAN
802.15.3 (Bluetooth)
Range --> short
Application (P2P or Device-to-device)

### LAN
802.11 (Wireless LAN)
Range ------> medium
Application (Enterprise Networks)

### MAN
802.11, 802.16, 802.20
Range ---------------> Medium-long
Application (Last Mile Access)

### WAN
GSM, CDMA, Satellite
Range -------------------------------------------------> Long
Application (Mobile Data Devices)


### Wireless Access Point (AP)

- Connects wireless clients to the wired LAN
- Layer 2 device
- Converts TCP/IP from 802.11 frame (Wireless LAN) encapsulation format in the air to the 802.3 (Ethernet) frame format on the wired Ethernet network
- Uses CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance)


### Wireless Router
Performs the role of AP, Ethernet Switch and a router. 3 in 1 teh tarik ;D

Service Set Identifier (SSID) is to distinguish between wireless networks


### 802.11 WLAN topologies

1. Ad Hoc network or Independent Basic Service Set (IBSS)
	- NO need access points.
	- Client configure the wireless parameters between themselves.

2. Basic Service Sets (BSS)
	- Improves range for clients using an AP
	- Adds services ??

3. Extended Service Sets
	- When a single BSS provides insufficient RF coverage, use more of BSS and join them into an extended service set (ESS).
	- BSSID is different but the SSID is the same.
	- The new coverage area is called extended service area (ESA)
	- BSSID is the MAC address of the AP serving the BSS

4. Common Distribution System
	- multiple points in an ESS to appear to be single BSS
	- ESS common SSID to allow user to roam from AP to AP


### 802.11 Client and AP Association
There's a few components:

1. Beacons
2. Probes
3. Authentication
4. Association


### Beacons
- Frames used by the WLAN network to advertise its presence.
- APs may broadcast beacons periodically.
- main goal is to allow WLAN clients to learn which networks and APs are available

### Probes
- Frames used by WLAN clients to find their networks

### Authentication
- A process which is an artifact from the original 802.11 standard, but still required by the standard.

### Association
- The process of establishing the data link between an AP and a WLAN client


Before an 802.11 client can send data over a WLAN network it goes through this 3 stages.
1. 802.11 probing
2. Authentication
3. Association


| AP | <-- Probe request 					<-- | Client |
| AP | --> Probe reply  					--> | Client |
| AP | <-- Authentication request <-- | Client |
| AP | --> Authentication reply 	--> | Client |
| AP | <-- Association request 		<-- | Client |
| AP | --> Association reply 			--> | Client |
| AP | <-> Data Transmission 			<-> | Client |

## 

Lecture 8 - WAN Technologies and PPP
=============================

What is WAN?
WAN is the data communication network that operates beyond the geographic scope of a LAN.


Cisco Enterprise Architecture models:
1. Enterprise Campus Architecture
2. Enterprise Branch Architecture
3. Enterprise Data Center Architecture
4. Enterprise Teleworker Architecture

WAN operation focus on Layer 1 and Layer 2 of the OSI model.

Data Link Encapsulation
1. Dedicated Point-to-point
	- Cisco HDLC
	- PPP
	- LAPS

2. Packet Switched
	- X.25
	- Frame Relay

3. Circuit Switched
	- ISDN


_Circuit_ vs _Packet_ Switching

    Circuit sets up a path end to end.
    Circuit using Time Division Multiplexing (TDM)
    Expensive way to move data

    Packets pass from switch to switch via addressing scheme in packet
    Can be connectionless (internet)
    Connection-oriented (Frame Relay)
    Prone to delays because bandwidth is shared.
TDM ?

    TDM uses timeslots.
    Bandwidth is statically allocated.
    It is protocol independant.

### WAN Technologies
Analog Dialup

- Analog local loop
- 33 kbps to 56 kbps
- Low implementation cost
- Dial-up technology

ISDN

- Digital local loop
- Two 64 kbps data channels, 1 16 kbps signaling channel
- Sometimes used as a WAN backup
- Dial-up technology

Leased Line

- Permanent dedicated point-to-point link
- Up to 2.5 Gbps bandwidth
- Very costly, particularly when connecting many sites

X.25

- First standardized packet switched technology
- Low bandwidth, maximum 48 kbps
- Provides full error and flow control
- Typical use is for POS card readers

Frame Relay

- Data rates typically up to 4 Mbps
- Simpler protocol that provides no error or flow control
- Provides permanent shared medium bandwidth connectivity that carries voice and data

ATM

- Data rates beyond 155 Mbps
- Built on cell-based architecture
- Cells have a fixed length of 53 bytes (payload 48 bytes)
- Well suited for carrying video and voice

Digital Subscriber Line – DSL

- Uses existing twisted-pair telephone line
- Different types of DSL
- Always on technology
- Multiple DSL lines multiplexed into a single high capacity link by a DSLAM
- Places data transmissions above the 4 Khz frequency used for voice
- For satisfactory service, must be less than 5.5km from DSLAM

Cable Modem

- High speed data transmission using same coaxial lines that transmit Cable TV
- Always-on shared connection
- Possibility of 30 – 40 Mbps of data on one 6 Mhz cable channel.


Point to Point Protocol (PPP)
=====================

PPP contains two sub-protocols:
1. Network Control Protocol
	- Responsible for configuring, enabling and disabling the network layer protocol.
	- Encapsulate and negotiate options for multiple network layer protocols

2. Link Control Protocol (LCP)
	- Negotiate and setup control options on the WAN data link.
	- The LCP sits on top of the physical layer and is used to establish, configure and test the data-link connection.
	- Handles varying limits on packet size
	- Terminate link
	- Determine when a link is functioning properly or when it is failing

### LCP
- LCP Listen
- Option negotiation
- Link Quality is determined (optional)
- Network layer configuratoin begins (IPCP, IPXCP, ATCP)
- Link establishment (LCP Open)
- LCP termination

PPP session Establishment

1. Link establishment
2. Authentication
3. Network Layer Protocol

### Link-establishment phase
- Each PPP device sends LCP frames to configure and test the data link.
- Phase is complete when a config ACK frame has been sent and received.

### Authentication phase (Optional)
- Takes place right before the network layer protocol phase.

### Network Layer Protocol Phase
- sends NCP packets to choose and configure one or more network layer protocols such as IP.
- Once configured, packets can be sent over the link.
- LCP closes the link.
- Informs the network layer protocol so they can take appropriate actions.
- PPP link remains configured until LCP or NCP frames closes the link or until an inactivity timer expires or user intervenes.

### Password Authentication Protocol (PAP)
- two-way handshake.
- not a strong authentication tool. IT IS WEAK WEAK WEAK!
- passwords are sent in clear text. noob as fuck lol
-  no protection from playback or repeated trial-and-error attacks.

### Challenge Handshake Authentication Protocol (CHAP)
- used at the startup of a link and periodically verifies the identity of the remote node using a 3 way handshake.
- after the Link-establishment phase, local router sends a "challenge" message.
- Remote node responds with a value calculated using a one-way hash function (MD5.
- Responside is based on the password and challenge message.
- Local router checks the response.
If values match, authentication is acknowledged, otherwise terminate connection. LIKE ZAP.
- CHAP provides protection against playback attack.
- Challenge is unique and random.

## 
Lecture 9 - Frame Relay
=======================

- Frame relay is packet-switched, connection-oriented, WAN service.
- Uses a subset of High-level Data link control (HDLC) protocol called Link Access Procedure for Frame Relay (LAPF)
- Carries data between Data Terminal Equipment (DTE) user devices and the Data Communication Equipment (DCE) at the edge of the WAN

Switched Virtual Circuit (SVC) between the two DTEs may change.

Permanent Virtual Circuits (PVC) between the two DTEs will always be the same. Like duhhh

DLCI ?

    Data-link connection identifier identifies the logical VC between the CPE and Frame Relay Switch (FRS)

## 

Lecture 10 - IP addressing Services - DHCP and NAT
==================================================


### Dynamic Host Configuration Protocol (DHCP)

Methods of address allocation:
1.  Manual
	- IP address for client is pre-allocated by the admin and DHCP conveys the address to the client

2.	Automatic
	- DHCP automatically assigns a permanent IP address to a client with no lease period.

3. Dynamic
	- DHCP assigns, or leases, an IP address to the client for a limited period of time.


### DHCP Operation

     Client | --> DHCPDISCOVER  --> | Server (Broadcast)
     Client | <-- DHCPOFFER     <-- | Server
     Client | --> DHCPREQUEST   --> | Server
     Client | <-- DHCPACK 	  <-- | Server


### Network Address Translation (NAT)

- can occur dynamically or statically
- NAT (PAT) port address translation allows multiple inside addresses to map to the same global address.


## 


Lecture 11 - Network Security
==============================

- White hat
	- An individual who looks for vulnerabilities in systems and reports these so that they can be fixed.
- Black hat
	- An individual who uses their knowledge to break into systems that they are not authorized to use
- Hacker
	- Programming expert

- Cracker
	- Someone who tries to gain unauthorized access to network resources with malicious intent.

- Phreaker
	- Individual who manipulates phone network, through a payphone, to make free long distance calls.

- Spammer
	- An individual who sends large quantities of unsolicited e-mail messages.

- Phisher
	- Uses e-mail or other means to trick others into providing information. 


### Open vs Closed networks

Security challenge faced by network admins are balancing the 2 important needs.

- Protect private, personal and business info.
- Keep networks open to support business requirements.

Open - any service is permitted unless it is expressly denied.

Restrictive - services are denied by default unless deemed necessary.


### Developing a Security Policy

- Informs users, staff and managers of their requirement for protecting information assets.
- Specifies the mechanisms through which these requirements can be met.
- Provides a baseline from which to acquire, configure and audit computer systems for compliance.


### Network Security

1. Vulnerability
	- Degree of weakness which is inherent in every network and device.

2. Threats
	- They are the people interested in taking advantages of each security weakness.

3. Attack
	- The threats use a variety of tools and programs to launch attack against networks.

### E.g: 

- Your back is your __vulnerability__.
- __Threat__ is someone threatening you to attack you using by taking advantages of your _vulnerability_.
- An __attack__ is when they make the _threat_ come true.


### Network attacks - 4 primary classes
1. Reconnaissance
	- unauthorized discovery and mapping of systems, services or vulnerabilities
	- a.k.a information gathering ;)
	- similar to someone looking around the neighbourhood and spying to see which homes are vulnerable to break into.

2. Access
	- Ability for an intruder to gain access to a device for which the intruder does not have password to.

3. Denial of Service - DoS
	- Disables or corrupts networks, systems with the intent to deny services to intended users.
	- the most annoying one of all UGH

4. Worms, Viruses and Trojan Horses
	- Malicious software to damage or corrupt a system, replicate itself or deny access to networks, ,systems or services.


### Examples

1. Reconnaisance

	- port scans (nmap, superscan)
	- packet sniffers (wireshark)

2. Network Access

	- password attacks
	- trust exploitation
	- port redirection
	- MITM attack

3. Network Attacks

	- DoS
	- Worm: makes lots of copies of itself.
	- Virus: executing a particular unwanted function on a workstation
	- Trojan horse: written to look like something else but actually is a backdoor :D


## 


Lecture 12 - Teleworkers
==================


Business requirements for Teleworkers
- Organizational Benefits
		- Continuity of operations.
		- Increased responsiveness.
		- Secure, reliable and manageable access to info.
		- Cost-effective integration of voice, vid and data
		- increased employee productivity, satisfaction and retention.

- Social
		- Increased employment opportunities.
		- Less travel and commuter related stress.

- Environmental
		- Smaller carbon footprint ;)


Connecting Teleworkers to the WAN
1. Dialup Access
	- cheap but slow

2. DSL Access
	- seperates DSL signal from the telephone signal.
	- faster than dialup.
	- provides ethernet connection to a host computer or LAN

3. Cable Access
	- signal is carried on the same coaxial cable that delivers cable TV

4. Satellite Access

5. Broadband wireless
