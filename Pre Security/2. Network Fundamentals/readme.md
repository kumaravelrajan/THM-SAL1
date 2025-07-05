# Intro to LAN

## Switch vs Router 

- Switch works within a LAN. 
- Router connects different LANs. Router is what has internet capabilities and support NATs (Network address translation).

- Subnets use IP addresses in three different ways:

    - Identify the network address 
    
        - identifies the start of the actual network and is used to identify a network's existence.

        - For example, a device with the IP address of 192.168.1.100 will be on the network identified by 192.168.1.0

    - Identify the host address

        - An IP address here is used to identify a device on the subnet.

        - For example, a device will have the network address of 192.168.1.1

    - Identify the default gateway

        - The default gateway address is a special address assigned to a device on the network that is **capable of sending information to another network**.

        - Any data that needs to go to a device that isn't on the same network (i.e. isn't on 192.168.1.0) will be sent to this device. These devices can use any host address but usually use either the first or last host address in a network (.1 or .254)


## ARP

- ARP allows a device to associate its MAC address with an IP address on the network. 

- Broadcast request sent by a host A asking what is the MAC address associated with a particular IP address 192.168.1.10. The host B that owns 192.168.1.10 gives ARP reply providing its MAC address. Host A is hence able to associate in its cache the IP address of host B with the MAC address of host B. 

    ![ARP](Images/ARP.png)


## DHCP

DHCP servers allow newly joining host devices to get an IP address automatically. 

![DHCP](Images/DHCP.png)

# OSI model

![OSI model](Images/OSI-model.svg)

## Physical

Put simply, this layer references the physical components of the hardware used in networking and is the lowest layer that you will find. Devices use electrical signals to transfer data between each other in a binary numbering system (1's and 0's). 

For example, **ethernet cables connecting devices.**

## Data Link 

Receives packet from Network layer and adds MAC address information.

The MAC address is the address burnt into the Network Interface Card (NIC) of the remote system by the NIC manufacturer. It cannot be changed but can be spoofed. 

## Network layer

Everything is dealt with using IP addresses. Routers also deal with IP addresses. 

## Transport layer

TCP/UDP layer. Deals with ports. 

## Session layer

Once data has been correctly translated or formatted from the presentation layer (layer 6), the session layer (layer 5) will begin to create and maintain the connection to other computer for which the data is destined. When a connection is established, a session is created. Whilst this connection is active, so is the session.

## Presentation

Protocols such as HTTPS. 

## Application layer

Everyday applications such as email clients, browsers, or file server browsing software such as FileZilla provide a friendly, Graphical User Interface (GUI) for users to interact with data sent or received. Other protocols include DNS (Domain Name System), which is how website addresses are translated into IP addresses.

# Packets and Frames

## Packets vs Frames

- Frames are at level 2 while packets are at level 3. Packets are encapsulated within frames i.e. the frame forms a outer shell around the packet. 

    When a frame arrives at a device (such as a router), the device checks the destination MAC address. If the device is the intended recipient (e.g., the router is the next hop), it strips off the Layer 2 frame (decapsulation) and processes the Layer 3 packet.

    The router then determines where to forward the packet next. Before sending it out another interface, the router re-encapsulates the packet in a new Layer 2 frame appropriate for the next link, with new source and destination MAC addresses.

    This process repeats at each hop: the frame is decapsulated and re-encapsulated at every Layer 3 device (router) until the packet reaches its final destination.

## TCP three way handshake

- Source Port

    This value is the port opened by the sender to send the TCP packet from. This value is chosen randomly (out of the ports from 0-65535 that aren't already in use at the time).

- Destination Port

    This value is the port number that an application or service is running on the remote host (the one receiving data); for example, a webserver running on port 80. **Unlike the source port, this value is not chosen at random.**

## Ports 101

- Any port b/w 0 and 1023 is a system port. 

# Extending your network

## Port forwarding

Port forwarding is a networking technique that allows external devices on the internet to access specific devices or services inside a private local network by directing incoming traffic on certain ports to designated internal IP addresses and ports.

![alt text](Images/portForward.svg)

- Example: 

    Imagine you want to access your home computer remotely from your office. Your home network is behind a router that has a public IP address. Normally, the router blocks unsolicited incoming connections for security.

    You set up port forwarding on your home router to forward requests coming to port 80 on your router’s public IP address to port 80 on your home computer’s private IP address (e.g., 192.168.1.10).

    When you type your home’s public IP address followed by :80 (like 82.62.51.70:80) in a browser or remote access tool at your office, the router receives the request.

    The router checks its port forwarding rules, sees that port 80 is forwarded to your home computer, and sends the incoming traffic to that computer.

    Your home computer then responds to the request, allowing you to access the service running on it remotely.

    In this way, port forwarding acts like a "doorway" through your router, directing external requests on specific ports to the right device inside your network without exposing all your devices to the internet.