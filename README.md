<h1 align=center>
	<b>Net_Practice</b>
</h1>

<h2 align=center>
	<i>42cursus</i>
</h2>

<p align=center>
	<i>This project has been created as part of the 42 curriculum by <pabalvar></i>
</p>

<p align=center>
	The <b>net_practice</b> project is our first networking related project in the 42 curriculum. It consists of 10 exercises in which we configure small-scale networks so that hosts, switches and routers can communicate with each other, applying the concepts of TCP/IP addressing, subnet masks and default gateways.
</p>

---

<h2 align=center> Index </h2>
<h3 align="center"><b>
	<a href="#description">Description</a>
	<span> • </span>
	<a href="#instructions">Instructions</a>
	<span> • </span>
	<a href="#Network">What is a Network?</a>
	<span> • </span>
	<a href="#TCP">What is TCP and IP Address?</a>
	<span> • </span>
	<a href="#IPv4">IPv4 and Subnet Masks</a>
	<span> • </span>
	<a href="#Connecting">Connecting multiple devices</a>
	<span> • </span>
	<a href="#Switch">Switch</a>
	<span> • </span>
	<a href="#Router">Router</a>
	<span> • </span>
	<a href="#RouterTable">Routing Table</a>
	<span> • </span>
	<a href="#NetPractice">Net Practice — Levels</a>
	<span> • </span>
	<a href="#resources">Resources</a>
</b></h3>

---

<h2 id="description">Description</h2>

**NetPractice** is a practical introduction to computer networking. Instead of writing code, we are given a series of **10 broken network diagrams** in a training interface, and for each one we must fix the configuration (IP addresses, subnet masks, gateways, routing tables) so that all the devices in the diagram can reach each other correctly.

The goal is to build real, hands-on understanding of:
- How an IPv4 address is split into a network portion and a host portion.
- How subnet masks define the size and boundaries of a network.
- How switches connect devices within the same local network.
- How routers connect separate networks together and forward packets between them using routing tables.

Each of the 10 levels increases in complexity, going from a simple direct connection between two hosts, to multi-router topologies connected to "the Internet."

<h2 id="instructions">Instructions</h2>

### Running the training interface

1. Download and extract the project archive into any folder.
2. Launch the training interface:
   ```bash
   ./run.sh
   ```
   This starts a local web server and opens the NetPractice page in your browser.
3. If `run.sh` doesn't work, start the server manually:
   ```bash
   python3 -m http.server 49242
   ```
   Then open `http://localhost:49242` (or whichever port you used) in your browser.

> A local web server is required due to browser security restrictions — opening the HTML file directly will not work.

### Solving a level

- Enter your **login** in the interface field to load your personal configuration, or use the **evaluation** tab to generate a random one.
- For each level, edit the unshaded/editable fields (IP address, mask, gateway, routing table entries) until the diagram is fixed.
- Click **Check again** to validate your configuration.
- Read the **logs** panel at the bottom of the page — it explains what's wrong (invalid IP, missing gateway, overlapping subnet, etc.).
- Once a level is solved, click **Get my config** to export the configuration file for that level, then move on to the next one.

### Submission

- All **10 exported configuration files** (one per level) must be placed at the **root of this repository**.
- Your **login must be entered in the interface** before exporting — it is required for the file to be valid during evaluation.
- During the defense, you will need to solve **3 random levels** within a limited time, without using any external tools (a basic calculator such as `bc` is tolerated).

---

# Networking concepts

<h2 id="Network">
What is a Network?
</h2>

In simple terms, a **Network** in computing is a group of two or more devices that can communicate amongst each other. By communicating we mean the possibility of **sending** and **receiving** data (or *packets*) - between different devices (or *nodes*).

The **Internet** is one of many examples of a network. It is probably the biggest network in the world, and connects many different nodes, making the sharing of data available between devices located anywhere in the world.

The Internet is considered a **Public Network**. A Public Network allows for data transfer between any device connected to it, with no or very little access control. Usually, it is operated by a telecommunications company for the specific purpose of providing data transmission services to the public.

On the other hand, we also have **Private Networks**. A Private Network allows for communication between devices that are restricted to a specific location or domain. Data transfer is not allowed for anyone unregistered, making it a much safer and controlled environment. A Home Network is an example of a private network — you can connect to your personal printer, but no one outside your house's Wi-Fi could.

<h2 id="TCP">
What is TCP and IP Address?
</h2>

In order to transfer data from one point to another across a network, many different types of protocols exist to ensure access, transport, security and stability. All these protocols work together, each one responsible for what we call a "layer" in the communication process.

**Transmission Control Protocol** (or **TCP**) is a communication standard protocol that enables nodes to communicate with each other. TCP is responsible for "breaking" data into small pieces (packets), sending them over the network, and then "rebuilding" them all back again while ensuring no data is lost in the process.

And how does it know where to send those pieces? It does so using an **Internet Protocol Address** (or **IP Address**), a series of numbers used to identify any device connected to a network, either public or private.

TCP and IP are not the same thing, but rather **two separate protocols** that work together to ensure data transfer between different devices.

There are two versions of IP Addresses: **IPv4** and **IPv6**. For this project, we only need IPv4, the oldest and most broadly used protocol model. From now on we will use the terms IP and IPv4 interchangeably.

<h2 id="IPv4">
IPv4 and Subnet Masks
</h2>

An IPv4 address is a 32-bit number divided into four 8-bit blocks, each ranging from `0` to `255` (`00000000` to `11111111` in binary).

Every IP Address can be split into two parts: the **Network** portion and the **Host** portion.

- The **Network Address** portion identifies the network the device belongs to. Devices must share the same Network Address portion to be able to communicate directly.
- The **Host Address** portion is the individual identifier of the device within that network, and must be unique per network.

To know which bits belong to the network and which to the host, we apply a **Subnet Mask** (also a 32-bit number in four 8-bit blocks), where a `1` marks a network bit.

Example:
```
IP Address:   153.172.250.12
Subnet Mask:  255.255.255.0
CIDR:         /24
```
In binary:
```
IP Address:   10011001.10101100.11111010.00001100
Subnet Mask:  11111111.11111111.11111111.00000000
```
The first 24 bits are the network portion, so any device from `153.172.250.1` to `153.172.250.254` belongs to the same network (extremities reserved, see below).

A larger example:
```
IP Address:   153.172.175.12
Subnet Mask:  255.255.240.0
CIDR:         /20
```
Here the first 20 bits are the network portion, giving a range of `153.172.160.0` to `153.172.175.255`.

Note that in a valid mask, once a `0` appears (reading left to right), everything after it must also be `0`. There are only 9 valid 8-bit blocks for a mask:

```
0 0 0 0 0 0 0 0 ->   0
1 0 0 0 0 0 0 0 -> 128
1 1 0 0 0 0 0 0 -> 192
1 1 1 0 0 0 0 0 -> 224
1 1 1 1 0 0 0 0 -> 240
1 1 1 1 1 0 0 0 -> 248
1 1 1 1 1 1 0 0 -> 252
1 1 1 1 1 1 1 0 -> 254
1 1 1 1 1 1 1 1 -> 255
```

When splitting a network into subnets, **two addresses are always reserved** and cannot be assigned to a device:
- The **first IP** in the range identifies the subnet itself (network address).
- The **last IP** in the range is reserved for broadcasting to all devices in the subnet.

| CIDR | Subnet Mask | # of IP Addresses | # of Usable IPs |
|------|-------------|--------------------|------------------|
| /30  | 255.255.255.252 | 4     | 2      |
| /29  | 255.255.255.248 | 8     | 6      |
| /28  | 255.255.255.240 | 16    | 14     |
| /27  | 255.255.255.224 | 32    | 30     |
| /26  | 255.255.255.192 | 64    | 62     |
| /25  | 255.255.255.128 | 128   | 126    |
| /24  | 255.255.255.0   | 256   | 254    |
| /20  | 255.255.240.0   | 4,096 | 4,094  |
| /18  | 255.255.192.0   | 16,384| 16,382 |
| /16  | 255.255.0.0     | 65,536| 65,534 |

<h2 id="Connecting">
Connecting multiple devices
</h2>

Public Networks like the **Internet** are established by telecommunication providers to connect devices worldwide.

A **Public IP Address** is assigned by your **Internet Service Provider (ISP)** directly to your personal **router**, giving connectivity to your private network.

Your private network is established through a **switch** inside that router, which assigns a **Private IP Address** to each device, following the same subnetting rules.

Some IP ranges are reserved for specific uses:

| IP Address Range | Reserved for |
|---|---|
| 10.0.0.0 – 10.255.255.255 | Private IPs (Class A) |
| 127.0.0.0 – 127.255.255.255 | Loopback / internal testing |
| 172.16.0.0 – 172.31.255.255 | Private IPs (Class B) |
| 192.168.0.0 – 192.168.255.255 | Private IPs (Class C) |
| 224.0.0.0 – 239.255.255.255 | Multicast |
| 240.0.0.0 – 255.255.255.255 | Experimental / research |

<h3 id="Switch">Switch</h3>

A network **switch** distributes packets between devices within the same (usually local) network. A switch has no IP interface of its own and cannot talk to a network outside of its own.

<h3 id="Router">Router</h3>

A **router** connects multiple networks together. Every router has one interface per network it connects to, and the IP range on each interface must never overlap with the range on another interface of the same router — otherwise those interfaces would belong to the same network.

<h3 id="RouterTable">Routing Table</h3>

A **Routing Table** lists the routes a router or host knows to reach a given destination network. In NetPractice, each entry has two fields:

- **Destination**: the target network, expressed as IP/CIDR (e.g. `190.3.2.252/30`). Use `0.0.0.0/0` (default) to match any destination not covered by a more specific route.
- **Next Hop**: the IP address of the next router to forward packets to on the way to that destination.

---

<h1 id="NetPractice">
Net Practice — Levels
</h1>

<details>
  <summary>Level 1 — Direct communication</summary>
  <br>

The first exercise is a simple problem of direct communication between two pairs of devices. Each pair must share the same network.

For Client A and Client B, both with mask `255.255.255.0` (`/24`), Client A's IP must fall in the same network as B, `104.94.23.0/24` — any address from `104.94.23.1` to `104.94.23.254`.

For Client C and Client D, both with mask `255.255.0.0` (`/16`), Client D's IP must fall within `211.191.0.0/16` — any address from `211.191.0.1` to `211.191.255.254`.

</details>

---

<details>
  <summary>Level 2 — Masks and reserved IPs</summary>
  <br>

Two things matter here: devices on the same network must share the same mask, and you cannot use reserved IPs.

Client A and Client B share mask `255.255.255.224` (`/27`). Since Client B is fixed at `192.168.36.222`, the network range is `192.168.36.192`–`192.168.36.223` (extremities excluded); Client A can take any other address in that range.

Client C and Client D share mask `255.255.255.252` (`/30`) — only 4 IPs in range, 2 usable. Pick IPs that aren't the network or broadcast address.

</details>

---

<details>
  <summary>Level 3 — Introducing the switch</summary>
  <br>

All three clients (A, B, C) must sit on the same network through a switch. Client C's mask is fixed at `255.255.255.128` (`/25`), so that's the mask for everyone. Client A is fixed at `104.198.73.125`, which places the network range at `104.198.73.0`–`104.198.73.127` (extremities excluded).

</details>

---

<details>
  <summary>Level 4 — Introducing the router</summary>
  <br>

This router has 3 interfaces, each needing its own non-overlapping IP range.

Interface R2 already covers `63.12.111.0`–`63.12.111.127`, and Interface R3 covers `63.12.111.192`–`63.12.111.255`. Client A is fixed at `63.12.111.132`, right in the gap between them. A mask of `/27` or smaller works; choosing `255.255.255.240` (`/28`) gives the range `63.12.111.128`–`63.12.111.143` for Interface R1's network, with no overlap.

</details>

---

<details>
  <summary>Level 5 — First routing table</summary>
  <br>

Client A shares mask `255.255.255.128` (`/25`) with Interface R1, in range `74.150.109.0`–`74.150.109.127`.

Client B shares mask `255.255.192.0` (`/18`) with Interface R2, in range `158.42.64.0`–`158.42.127.255`.

For the routing tables: Client A's destination can be Client B's IP (`158.42.127.42/18`) or simply `default` (`0.0.0.0/0`); the next hop is the local router interface in each case.

</details>

---

<details>
  <summary>Level 6 — Reaching "the Internet"</summary>
  <br>

The Internet is modeled as a router with many interfaces. Client A and Router R's Interface R1 share mask `255.255.255.128` (`/25`); Interface R1 must sit in range `45.149.96.128`–`45.149.96.255`.

Client A's route destination is the Internet interface (`8.8.8.8/16`) or `default`, with next hop `45.149.96.254`. The Internet's route destination is Interface A1 (`45.149.96.227/25`) or `default`, with next hop being Router R's other interface. Router R's own table can be set to `default` since its listed next hop doesn't match any interface directly.

</details>

---

<details>
  <summary>Level 7 — Avoiding overlaps</summary>
  <br>

Three separate networks exist here:
- Network 1: Interface A1 ↔ Interface R11
- Network 2: Interface R12 ↔ Interface R21
- Network 3: Interface R22 ↔ Interface C1

None of their IP ranges can overlap. Using a `/28` mask for all three gives 16 addresses each — plenty for this exercise (masks don't have to match across networks, though).

With `/28`:
- Network 1: `119.198.14.0`–`119.198.14.15`
- Network 2: `119.198.14.240`–`119.198.14.255`
- Network 3: `119.198.14.128`–`119.198.14.143`

Routing tables: Client A's destination is Client C's IP (and vice versa); routers use each other's interfaces as next hop.

</details>

---

<details>
  <summary>Level 8 — Two hosts reaching the Internet</summary>
  <br>

The Internet's routing table has a fixed destination of `163.14.136.0/26`, so every network on the path must fit inside that /26 without overlapping.

Three networks, split with `/28`:
- Network 1 (C1 ↔ R22): `163.14.136.16`–`163.14.136.30`
- Network 2 (D1 ↔ R23): `163.14.136.0`–`163.14.136.15`
- Network 3 (R21 ↔ R13): `163.14.136.48`–`163.14.136.63`

Router R2's next hop is fixed at `163.14.136.62`, which fixes Interface R13's IP. Clients C and D route with `default`; routers R1 and R2 point to each other.

</details>

---

<details>
  <summary>Level 9 — Combining everything</summary>
  <br>

An aggregation of all previous concepts, with 4 networks to configure:
- Network 1 (A1, B1, R11): `/25`, `142.168.31.0`–`142.168.31.127`
- Network 2 (C1, R22): `/24`, `42.42.42.0`–`42.42.42.254`
- Network 3 (D1, R23): `/18` (pre-set), around `102.155.128.0`–`102.155.128.255`
- Network 4 (R13, R21): `/30` (pre-set), `37.219.16.252`–`37.219.16.255`

The key routing table is the Internet's, which must list both hosts that need direct external access.

</details>

---

<details>
  <summary>Level 10 — Final aggregation</summary>
  <br>

Most settings are pre-configured; the main task is avoiding overlaps, since the Internet only has one route to Router R1 covering the whole range.

Four networks:
- Network 1 (H11, H21, R11): `/25`, `169.222.32.0`–`169.222.32.127`
- Network 2 (H31, R22): `/28`, `169.222.32.224`–`169.222.32.239`
- Network 3 (H41, R23): `/26`, around `169.222.32.128`–`169.222.32.191`
- Network 4 (R21, R13): `/30`, `169.222.32.252`–`169.222.32.255`

Configure the routing tables accordingly, keeping in mind the Internet's table has a single destination, `169.222.32.0/24`.

</details>

---

<h2 id="resources">Resources</h2>

Networking concepts studied and applied in this project:
- TCP/IP addressing
- Subnet masks and CIDR notation
- Default gateways
- Switches vs. routers
- Routing tables (destination / next hop)
- OSI model — mainly Layer 2 (Data Link) and Layer 3 (Network)
- Reserved IP ranges (private, loopback, multicast, experimental)

Classic references:
- RFC 791 – Internet Protocol
- RFC 950 – Internet Standard Subnetting Procedure
- Cisco Networking Academy – Introduction to Networks

### AI usage

AI (an LLM-based assistant) was used to:
- Help structure and format this README (index, sections, tables).
- Clarify theoretical points on subnetting and routing tables while studying.

AI was **not** used to solve the network levels themselves in the training interface — every configuration was worked out and understood manually, then reviewed with peers before submission, in line with the project's learning goals.
