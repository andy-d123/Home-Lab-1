# Home Lab

## Objective

This Homelab is a diverse and structured environment designed to replicate a real-world network, as shown in the network topology. It will include multiple VLANs, firewall, SIEM, DFIR tools, IDS/IPS, various hosts and servers, an isolated analysis environment, etc. Everything will be documented, and this setup will continue to grow as I add more features and refine the environment.

## Inspiration/Guide

This homelab setup was inspired by [Building a Blue Team Homelab] by Marko Andrejic (alias facyber). You can check out the original guide here: [https://facyber.me/page3/]. As I was new to creating this, I used it as a foundation for creating my own homelab. 

## Skills Learned

Currently:
- VLAN Configuration
- pfSense Firewall Setup and Rule Management

## Tools Used

- pfSense for firewall configuration and network segmentation


# Part 1 - Network Topology

## VLANs, Subnets, and Firewall

| VLAN Name         | VLAN ID | Subnet         | Gateway       | Description                     |
|-------------------|---------|----------------|---------------|---------------------------------|
| Management        | 1       | 10.0.1.0/24    | 10.0.1.1      | Access to pfSense web interface (https://10.0.1.50/) |
| Corporate WAN     | 10      | 10.0.10.0/24   | 10.0.10.254   | Simulated external network      |
| Corporate LAN     | 20      | 10.0.20.0/24   | 10.0.20.254   | Internal corporate network      |
| Security          | 50      | 10.0.50.0/24   | 10.0.50.254   | Security network                |
| Isolation LAN     | 99      | 10.0.99.0/24   | 10.0.99.254   | Isolated LAN                    |

---

## pfSense Configuration

Wan Interface: 
- **IPv4 Address:** `192.168.x.100/24`
- **Gateway:** `192.168.xxx.xxx` *(ISP modem/router)*

Lan Interface (Management):
- **IPv4 Address:** `10.0.1.1/24`

---

## Firewall Wall Rules

### Corporate WAN (VLAN 10)

 Rule # | Source          | Destination     | Protocol | Action | Description                      |
|--------|-----------------|-----------------|----------|--------|---------------------------------|
| 1      | 10.0.10.0/24    | 10.0.20.0/24    | Any      | Allow  | Allow traffic to Corporate LAN  |
| 2      | 10.0.10.0/24    | 10.0.10.0/24    | Any      | Allow  | Allow traffic within VLAN 10    |

### Corporate LAN (VLAN 20)

| Rule # | Source          | Destination     | Protocol | Action | Description                     |
|--------|-----------------|-----------------|----------|--------|---------------------------------|
| 1      | 10.0.20.0/24    | 10.0.20.0/24    | Any      | Allow  | Allow traffic within VLAN 20    |
| 2      | 10.0.20.0/24    | 10.0.10.0/24    | Any      | Allow  | Allow traffic to Corporate WAN  |

### Security (VLAN 50)

| Rule # | Source          | Destination     | Protocol | Action | Description                     |
|--------|-----------------|-----------------|----------|--------|---------------------------------|
| 1      | 10.0.50.0/24    | Any             | Any      | Block  | Block traffic towards WAN       |
| 2      | 10.0.50.0/24    | 10.0.1.0/24     | Any      | Block  | Block traffic towards Management       |
| 3      | 10.0.50.0/24    | 10.0.10.0/24    | Any      | Block  | Block traffic towards Corporate WAN       |
| 4      | 10.0.50.0/24    | Any             | Any      | Allow  | Allow traffic anywhere else       |

### Isolated VLAN

| Rule # | Source          | Destination     | Protocol | Action | Description                     |
|--------|-----------------|-----------------|----------|--------|---------------------------------|
| 1      | 10.0.99.0/24    | 10.0.99.0/24    | Any      | Allow  | Allow traffic within Isolated VLAN       |
| 2      | 10.0.99.0/24    | 10.0.99.0/24    | Any      | Block  | Block all other traffic       |

---

## Rough Network Topology

<img src="https://i.imgur.com/StlAbmO.png" alt="Alt Text" width="600" height = "350">

---


