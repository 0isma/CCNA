# CCNA Notes

# Day 1

- Basic info about network devices

# Day 2: Interfaces and cables

- RJ-45 are ethernet cables
- UTP cables (Cooper Cables): Unshielded Twisted Pair
    - Protect against EMI (Electromagnetic Interference)
    - 2 pairs (10BASE-T & 100BASE-T) or 4 pairs (1000BASE-T & 10GBASE-T)
- What pins in computers, switches and routers send and receive information.
    - C, S, R: Send 1 and 2, receive in  3 and 6
    - S: The opposite to C
- Straight-through cables, Crossover Cables and Auto MDI-X (modern).
    - If Auto MDI-X is disabled, UTP must transmit with crossover cables
- SFP (Small Form-Factor Pluggable) Transciever for Fiber Optics
    - 1 cable to transmit and 1 cable to receive
    - Fiber optics single mode (more expensive) and multi mode
    
    [comment]: ![Untitled](CCNA%20Notes%20181f28b43e264c7ea740b5e956923c5c/Untitled.png)
    
    [comment]: ![Untitled](CCNA%20Notes%20181f28b43e264c7ea740b5e956923c5c/Untitled%201.png)
    

# Day 3: The OSI Model

1. Physical
    - Medium through which info is transmitted
2. Data Link
    - Node to node connectivity
    - How data is formatted for transmission
    - Detects and corrects errors
    - Layer 2 Addressing (MAC) & Switches

# Day 4: Intro to the CLI

- CLI: command line interface
    - Cisco IOS CLI
- GUI: Graphical User Interface
- Connecting to a cisco device with USB Mini-B or RJ-45 with USB/VGA end (Rolloever Cable).
    - Access Via Putty, etc.
- Different modes when accessing the CLI:
    - User EXEC Mode (>)
        - No changes can be made, only view.
    - Privilege EXEC Mode (#)
- Commands in Cisco IOS CLI
    - launched with enabled
    - ? to view commands
    - configure terminal to access global configuration
    - enable password "*password"*
    - exit to retrieve
- Two different configuration files:
    - Running-config: active while running
        - show running-config
    - Start-config: loaded when launched
    - Saving running-config to startup with **write**
- Encrypt password with service password-encryption
    - Current and future passwords will be encrypted
    - Enable secret not affected
- Enable secret "*password*"
- run
- no (remove)

# Day 5:  Ethernet LAN Switching

- LAN: Network composed in a small area
    - Switches do not separate LANs (Routers do)
- PDU’s of layer 2 are called Frames
- Ethernet Header
    - Preamble
        - 7 bytes
        - 1 and 0’s: 10101010 x 7
        - Synchronisation of  receiver clocks
    - SFD
        - 1 byte
        - 10101011
        - End of Preamble
    - Destination (of MAC addresses)
        - 6 bytes
    - Source (of MAC addresses)
        - 6 bytes
    - Type (regarding layer 3 or length)
        - 2 bytes
        - If 1500 or less: length
        - If 1536 or more: type of encapsulated Packet
- Ethernet Trailer
    - FCS (checks errors)
        - 4 bytes
        - Detects corrupted data
- MAC Addresses
    - 6 bytes
    - Unique
    - OUI frabricant (3 bytes)
    - Hexadecimal
    - MAC Address Table
        - Dynamic and Static MAC addresses
        - Unicast Frames  (unknown or known)
        - MAC address Table
            - Stored in Switches
            - Filled with source IP’s with interfaces used to reach them
        

# Day 6:  Ethernet LAN Switching Pt2

- Minimum size of Ethernet frame is 64 bytes
    - 46 bytes of payload
    - 18 bytes of header and trailer
    - If not reached, padding is added
- Use of ARP to discover the associated MAC addresses to the known IP destination addresses.
    - ARP request (Broadcast)
    - ARP reply (Unicast)
- Ping
    - Tests if two computers can reach each other
    - Measures RTT
    - ICMP Echo reply and Echo request
    - padding used if 46 bytes min size (min size of the payload) not reached.
    

→ **Cisco CLI commands:**

- *ping <ip_address>*
- *show mac address-table*
- *clear mac addresses-table dynamic*
    - removed every 5 minutes after unused
    - command used if one does not want to wait
- *clear mac address-table dynamic address <mac address>*
- *clear mac address-table dynamic interface <interface-id>*

# Day 7: IPv4 Addressing

- How IP addresses are assigned
    - Class A: 0-127 (0xxxxxxx) /8
    - Class B: 128-191 (10xxxxxx) /16
    - Class C: 192-223 (110xxxxx) /24
    - Class D: 224-239
    - Class E: 240-255
- Class D and E are reserved for special purposes
- Very important for subnetting!!!
    - masks tell how many hosts and nets can be established
- Network address: .0
- Broadcast address: .255
- First and last usable addresses: .1 and .254

→ **Cisco CLI commands:**

- *en* (enable) → admin mode
- *show ip interface brief* (all info)
    - Status (layer 1) and Protocol (layer 2) administratively down and down by default in **routers** (shutdown command)
    - If down in status impossible down in protocol
- *conf t* → global configuration
- *interface <interface_name>* → config-if (interface configuration mode)
- *ip address <ip_address> <net_mask>*
- *no shutdown* → removes adm. down of routers.
    - also rebooting
- do + shortcuts (do sh ip int br)
- *interfaces* → info about interfaces
- *description <description for inteface>*

# Day 9: Switch Interfaces

- *Show ip interface brief*
    - Up by default if connected to other devices
    - Down and administratively down are not the same
    - Shutdown command is not applied by default
- *Show interfaces status*
    - Port (list of interfaces)
    - Names (empty default)
    - Status
    - Duplex is auto by default
        - a(auto)-full will negotiate full duplex communication
    - Speed: b/s
    - Type: send and receive data at the same time (duplex) or not (half duplex)
- *Concept of half and full duplex*
    - Can/Cannot Send and receive data at the same time
    - Avoid collisions with CSMA/CD in half duplex

### Interface speed and duplex

- Configuration of speed and duplex according to previous speed and type fields.
- *Interface range* *int_name_range* (i.e f0/5 - 10, or f0/5 -7, f0/9-10)

### Speed and duplex auto-negotiation

- Interfaces can run at different speeds (10 (ethernet), 10/100 (fast Eth), 10/100/1000 (gigabit Eth))
- Interfaces advertise their neighbour devices about their capabilities to then negotiate best speed and duplex they’re capable of.
- If autonegotiation is off
    - Duplex will be:
        - Half if 10 or 100
        - Full if 1000 or greater
    - Speed will be sensed
        - If fails, least possible used (i.e. 10 in case 10/100/1000)

### Interface Status

### Interface counters and errors

- Runts: smaller than min frame size
- Giants: bigger than max
- CRC: failed CRC check
- Frame: incorrect format
- Input errors: total of counter errors like crc or frame.
- Output errors: tried to send but couldn’t

→ **Cisco CLI commands:**

- *Show ip interface brief*
- *Show interfaces status* only for Switches)
- *speed*
- *duplex*
- *Interface range* *int_name_range* (i.e f0/5 - 10, or f0/5 -7, f0/9-10)
    - One can separate them with commas
  
- Lab Examples
    - When duplex type is set in one end (Router) but not in the other (Switch), Protocol (Layer 2) is down.

[comment]: ![Untitled](CCNA%20Notes%20181f28b43e264c7ea740b5e956923c5c/Untitled%202.png)

# Day 10: IPv4 Header

# Day 11: Static Routing

# Day 12: The life of a Packet

# Day 13-15: Subnetting

[comment]: ![Untitled](CCNA%20Notes%20181f28b43e264c7ea740b5e956923c5c/Untitled%203.png)