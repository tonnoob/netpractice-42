*This project has been created as part of the 42 curriculum by osousa-d.*

# NetPractice

## Table of Contents

- [Description](#description)
- [Instructions](#instructions)
- [How to download, extract, and open the project](#how-to-download-extract-and-open-the-project)
- [Understanding network dynamics](#understanding-network-dynamics)
- [Resources](#resources)

## Description
This project focuses on understanding computer networks and IP addresses, connecting devices through routers, and understanding the role of gateways in a network. It also covers subnetting concepts, including IP masks, CIDR notation, block sizes, and network address ranges.

---
## Instructions

### How to download, extract, and open the project

The project is distributed as a compressed archive in the .tgz format. To use it correctly, follow the steps below:

1. Download the .tgz file to your machine.

2. Open the terminal and navigate to the folder where the archive was saved:

		cd ~/Downloads

3. Extract the archive contents:

		tar -xzf name-of-file.tgz

4. Enter the extracted folder:

		cd name-of-extracted-folder

5. Check whether there is an execution script such as run.sh:

		ls

6. If the script exists, make it executable and run it:

		chmod +x run.sh
		./run.sh

7. If the project does not include a startup script, run a local HTTP server from the project folder:

		python3 -m http.server 8080

8. Open the interface in your browser:

		http://localhost:8080/

9. To stop the local server, press Ctrl + C in the terminal where it is running.

> The execution flow may vary depending on the structure of the downloaded package, but the standard workflow is: download the .tgz file, extract it, enter the folder, run the script or start a local server, and open the interface in the browser.

### Exporting configurations

As you solve each level, use the export option available in the interface to save the configuration for that level as a JSON file.

Each level must generate a separate configuration file using the following convention:

	level1.json
	level2.json
	level3.json
	...
	level10.json

The exported files must be placed in the root of the repository, without subfolders.

### Submission requirements

The project submission requires that all 10 exported configuration files (one per level) be placed in the root of the repository.

Expected structure:

	/netpractice-42
	├── README.md
	├── level1.json
	├── level2.json
	├── level3.json
	├── level4.json
	├── level5.json
	├── level6.json
	├── level7.json
	├── level8.json
	├── level9.json
	├── level10.json

Main requirements:

- All files must be in the project root.
- Each file must correctly represent the final configuration for its corresponding level.
- The content must be valid JSON.
- The project must be delivered with updated documentation, including this README.

## Understanding network dynamics
---
### 1. Network devices
- **PC (Host)** - A PC, also called a host, is a device connected to the network. It has a network interface configured with an IP address and a subnet mask.

	The default gateway is used when the destination is outside the local network and the host needs to forward the packet to a router.

	Example configuration:

	| Field | Value |
	|---|---|
	| IP | 159.192.109.2 |
	| Mask | 255.255.255.192 (/26) |
	| Gateway | 159.192.109.1 |
---
- **Switch** - A switch connects multiple devices within the same local network (LAN). It forwards network frames between its interfaces using MAC addresses to identify devices.

	Important: connecting two PCs to the same switch does not guarantee that they can communicate directly.

	For direct IPv4 communication between two hosts to work, they usually need to be on the same subnet and configured correctly. If they are on different subnets, a router is required to forward packets between them.

	Example:
	- PC A: **192.168.1.10/24**
	- PC B: **192.168.1.20/24**

	Both belong to the network **192.168.1.0/24** and can communicate directly through the switch.

	Now, if PC B uses **192.168.2.20/24**, it will be on another subnet. Even connected to the same switch, communication between the subnets will require routing.
---
- **Router** - A router connects different IP networks and forwards packets between them.
	Each router interface that participates in a network has its own IP address and subnet mask.
	When a packet arrives at the router, it checks its routing table to determine which interface to forward it through and what the next hop will be.

	Example:
	- Local network: **192.168.1.0/24**
	- Router interface on that network: **192.168.1.1**
	- Gateway configured on the PCs: **192.168.1.1**

	When a PC needs to reach a destination outside its local network, it sends the packet to the gateway.
---
- **Internet** - The Internet is a global network formed by interconnecting many networks. In a NetPractice exercise, the Internet element represents an external destination. To reach it, routers must have appropriate routes.
	It is also necessary to have a return path so responses reach the host that initiated the communication.

-------------------
### 2. IPv4 address, mask, and subnet
- **IPv4 address**

	An IPv4 address has 32 bits, divided into four octets of 8 bits.

	Example:

	| Mask: | 255. | 255. | 255. | 255 | |
	|---|---|---|---|---|---|
	| Bits: | 11111111 | 11111111 | 11111111 | 11111111 | <- total 32 bits |

	Another example:

	- **159.192.109.6**
	- Each octet represents a number from **0 to 255**.

---
- **Subnet mask**

	The mask indicates which part of the IP address identifies the network and which part identifies the devices within it.

	Example:

		A PC with these settings:
		- IP: 192.168.1.10
		- Mask: 255.255.255.0 (CIDR: /24)

		This means it is inside this block:
		- Network: 192.168.1.0
		- Broadcast: 192.168.1.255

	With a **/24** mask, the first **24 bits** identify the network, while the remaining **8 bits** identify the addresses inside that network.

	| Mask: | 255. | 255. | 255. | 0 | |
	|---|---|---|---|---|---|
	| Bits: | 11111111 | 11111111 | 11111111 | 00000000 | <- total 24 bits |
---
- **Network address, hosts, and broadcast**

	An IPv4 subnet traditionally has:
	- Network address: identifies the subnet itself.
	- Host addresses: addresses normally available to devices.
	- Broadcast: address used to send a packet to all devices on the subnet.

	These concepts are important to identify which addresses belong to the same network.
---

### 3. The three private ranges of RFC 1918

RFC 1918 defines three blocks of IPv4 addresses reserved for private network use. These addresses are not routed directly on the public Internet.
The three reserved ranges (RFC 1918):

| Class | IP Range | Block / Mask | Common Use |
|------|---|---|---|
| Class A | 10.0.0.0 to 10.255.255.255 | 10.0.0.0/8 | Large companies and data centers |
| Class B | 172.16.0.0 to 172.31.255.255 | 172.16.0.0/12 | Medium and large corporate networks |
| Class C | 192.168.0.0 to 192.168.255.255 | 192.168.0.0/16 | Home routers and small networks |

	Note: the labels Class A, B, and C are historical. Today, network planning mainly uses CIDR, which allows defining blocks of different sizes.
---

### 4. Understanding CIDR

CIDR is the notation used to indicate how many bits of an IPv4 address belong to the network portion.

	Examples:

| CIDR | Mask | Network bits |
|---|---|---|
| /8 | 255.0.0.0 | 8 |
| /16 | 255.255.0.0 | 16 |
| /18 | 255.255.192.0 | 18 |
| /24 | 255.255.255.0 | 24 |
| /26 | 255.255.255.192 | 26 |
| /30 | 255.255.255.252 | 30 |

The larger the prefix, the more bits are used to identify the network and the fewer remain available for addresses inside it.

### 4.1. Convert CIDR to number of addresses

An IPv4 address has 32 bits.

To find how many addresses exist in a block:

	Number of addresses = 2^(32 − prefix)

- Example with /18:

	- Step 1: Prefix: /18
	- Step 2: Remaining bits: 32 − 18 = 14
	- Step 3: Number of addresses: 2¹⁴ = 16,384

	Therefore, a /18 network has 16,384 IPv4 addresses.

In a conventional subnet, normally two addresses are reserved: the network address and the broadcast address. Thus, the usual number of usable host addresses is 16,382.

---
### 4.2. Convert CIDR to mask

Each octet has 8 bits.

In the mask, the network bits are set to 1 and the host bits are set to 0.

Example: /18

- Step 1: set the first 18 bits to 1.

		11111111.11111111.11000000.00000000

- Step 2: convert each octet to decimal.

		11111111 = 255
		11111111 = 255
		11000000 = 128 + 64 = 192
		00000000 = 0

- Result:

	**/18** is **255.255.192.0**
---
### 4.3. Values of bits in an octet

Each binary position represents a power of 2.

| Bit | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | <- 8 bits total |
|------|---|---|---|---|---|---|---|---|---|
| Value | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 | <- 256 total values |

(In this example, since only the first 1 is on, the octet value is 128)

To convert binary to decimal, add the values of the positions with 1.

Example:

	11000000

Calculation:

128 + 64 = 192

Thus, **11000000** equals **192**.

Quick reference table for mental calculation: 128 + 64 + 32 + 16 + 8 + 4 + 2 + 1

---
### 4.4. Find the block size of a mask

When the octet in the mask is between 1 and 254, the block size can be calculated as:

	Block size = 256 − value of the octet in the mask

Example:

- Step 1: Mask **255.255.192.0**
- Step 2: In the third octet, we have **192**.
- Step 3: 256 − 192 = **64**, so the block size in that octet is 64.

Networks begin on multiples of 64 in that octet: 0, 64, 128, and 192.

---
### 4.5. Convert mask to CIDR

To convert a mask to CIDR, count the number of set bits (1) in the four octets.

Example:

- Mask: 255.255.192.0
- First octet: 8 bits set.
- Second octet: 8 bits set.
- Third octet: 2 bits set.
- Fourth octet: 0 bits set.
- Total: 8 + 8 + 2 = 18
- Result: /18.

Tip: you can also discover the prefix by observing the sequence of bits in the mask. A valid mask has all 1 bits before all 0 bits.

---
### 5. Practical example: subnet separation

During NetPractice, it was necessary to identify the network of a host, which we will call Host Three.

- Host Three network: **159.192.109.4/30**

	| Address | Function |
	|---|---|
	| 159.192.109.4 | Network address |
	| 159.192.109.5 | Router Two (R22) address |
	| 159.192.109.6 | Host Three address |
	| 159.192.109.7 | Broadcast |

	The mask **/30** is equivalent to **255.255.255.252**.

- Block size calculation: **256 − 252 = 4**

	Thus, networks in the last octet begin on multiples of 4: **0, 4, 8, 12**, and so on.

	The network **159.192.109.4/30** contains four addresses, of which two are normally usable for network interfaces.

	In this example, Router Two uses **159.192.109.5**, while Host Three uses **159.192.109.6**. Since both belong to the same subnet, they can communicate directly.

---
### 6. Routes: destination network and next hop

A routing table tells the router how to forward packets to different networks.
Each route includes, among other information, a **destination** and a **next hop**.

- **Destination network**

	This is the IP network the packet is trying to reach.

	The router compares the destination IP address of the packet with the available routes and chooses the most specific route that matches.

	Example:

		159.192.109.128/26

	This route represents the network from **159.192.109.128 to 159.192.109.191**.

- **Next hop (gateway)**

	This is the IP address of the next router to which the packet should be forwarded.

	Example:

		159.192.109.128/26 → 159.192.109.253

	In this case:

	Destination: the network **159.192.109.128/26**.

	Next hop: the **router** using the address **159.192.109.253**.

	The **destination** indicates where the packet needs to arrive. The **next hop** indicates which router to send it to in the next step of the path.

- **Default route**

	The default route is represented by:

		0.0.0.0/0

	It matches any IPv4 destination and is used when no more specific route applies.

	Example:

		0.0.0.0/0 → 159.192.109.254

	This means the router will forward to 159.192.109.254 any packets whose destination does not match a more specific route.

	Upstream and return path

	Communication between two devices depends on both the forward path and the return path.

	For example:

		Host → Router One → Router Two → Host Three

	For communication to work, the devices involved must have compatible configurations and routes to forward packets in both directions.

	A correct forward route does not guarantee that the reply can return to the source device.

---
### 7. Checklist for solving NetPractice exercises

- Identify the IPs and masks of each interface.

- Determine which network each IP belongs to.

- Check whether hosts connected to the same switch are on the same subnet when they need to communicate directly.

- Confirm that each host's gateway is on a network directly reachable through its interface.

- Examine the routers' routing tables and identify the next hop for each destination.

- Verify that there is a return route to the source network.

- Change one configuration at a time and confirm that previously working objectives still remain functional.

Reminder: the goal is not only to make the packet reach its destination. It is also to ensure it reaches the correct host and that the return path also works.

---

## Resources

This project covers fundamental concepts of computer networking and communication infrastructure, including:

- IPv4 and IPv6 addressing
- TCP/IP and protocol modeling
- Subnet masks and CIDR
- Network addresses, broadcast addresses, and hosts
- Default gateways and routing
- Routers, switches, and hubs
- Local area networks (LANs), external networks, and inter-network connectivity
- Routing tables and next-hop logic
- Communication between hosts on different subnets
- OSI model layers and their relationship to network behavior
- Connectivity, packet forwarding, and network troubleshooting

References and useful materials:

- https://www-freecodecamp-org.translate.goog/news/subnet-cheat-sheet-24-subnet-mask-30-26-27-29-and-other-ip-address-cidr-network-references/?_x_tr_sl=en&_x_tr_tl=pt&_x_tr_hl=pt&_x_tr_pto=tc
- https://chatgpt.com/
- https://gemini.google.com/
- 42 peers

