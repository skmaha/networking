Cisco 891F router and 3560cx switch configuration

# Cisco 891F Router Configuration

## Overview

This README documents the configuration of the Cisco 891F Router (`skuas_router01`). The configuration includes interfaces, DHCP pools, NAT settings, routing, and other system-level configurations. This router is part of a network that serves several VLANs and utilizes Dynamic Host Configuration Protocol (DHCP) for automatic IP address allocation.

## General Information

- **Hostname**: `skuas_router01`
- **Device Model**: Cisco 891F
- **IOS Version**: 15.3
- **Last Configuration Change**: 22:28:58 UTC, Sat Mar 15 2025

## Key Features

1. **VLAN Configuration**: The router is set up with multiple VLANs including management, home network, development, QA, production, and DMZ.
2. **DHCP Configuration**: The router provides DHCP services for several VLANs.
3. **NAT Configuration**: Network Address Translation (NAT) is used for outbound traffic through the ISP connection.
4. **Routing**: Static routes are configured for internet connectivity.

## Interface Configuration

- **GigabitEthernet0**: 
  - **Description**: Connection to Switch
  - **VLAN**: 10 (Management Network)
  - **IP Address**: None (Layer 2 interface, VLAN tagging)
  
- **GigabitEthernet1**: 
  - **VLAN**: 30 (Home Network)
  - **IP Address**: None (Layer 2 interface)

- **GigabitEthernet2 to GigabitEthernet7**: 
  - **IP Address**: None (Unused interfaces)

- **GigabitEthernet8**:
  - **Description**: ISP Connection
  - **IP Address**: DHCP (Dynamic IP from ISP)
  - **NAT Outside**: Enabled
  
- **VLAN Interfaces (Vlan1 to Vlan70)**:
  - Each VLAN interface is configured with an IP address and description that reflects the intended use of the respective network.
  - **IP NAT Inside** is enabled on these interfaces to handle internal traffic.

### VLAN Details:

1. **VLAN 10** - Management Network: `10.1.1.1/24`
2. **VLAN 30** - Home Network: `192.168.30.1/24`
3. **VLAN 40** - Development Network: `172.16.40.1/24`
4. **VLAN 50** - QA Network: `172.16.50.1/24`
5. **VLAN 60** - Production Network: `20.60.60.1/24`
6. **VLAN 70** - DMZ Network: `20.70.70.1/24`

### Disabled Interfaces:
- **BRI0**: ISDN interface (disabled and shutdown)
- **Async3**: Async interface (disabled and shutdown)

## DHCP Configuration

The router is configured with DHCP pools to allocate IP addresses to different VLANs. The DHCP settings include default routers and DNS servers.

### DHCP Pools:

1. **VLAN 10 (Management Network)**:
   - **Network**: `10.1.1.0/24`
   - **Default Router**: `10.1.1.1`
   - **DNS Servers**: `8.8.8.8`
   
2. **VLAN 30 (Home Network)**:
   - **Network**: `192.168.30.0/24`
   - **Default Router**: `192.168.30.1`
   - **DNS Servers**: `8.8.8.8, 8.8.4.4`
   
3. **VLAN 40 (Development Network)**:
   - **Network**: `172.16.40.0/24`
   - **Default Router**: `172.16.40.1`
   - **DNS Servers**: `8.8.8.8, 8.8.4.4`
   
4. **VLAN 50 (QA Network)**:
   - **Network**: `172.16.50.0/24`
   - **Default Router**: `172.16.50.1`
   - **DNS Servers**: `8.8.8.8, 8.8.4.4`
   
5. **VLAN 60 (Production Network)**:
   - **Network**: `20.60.60.0/24`
   - **Default Router**: `20.60.60.1`
   - **DNS Servers**: `8.8.8.8, 8.8.4.4`
   
6. **VLAN 70 (DMZ Network)**:
   - **Network**: `20.70.70.0/24`
   - **Default Router**: `20.70.70.1`
   - **DNS Servers**: `8.8.8.8, 8.8.4.4`

## NAT Configuration

The router is configured to perform **Network Address Translation (NAT)** for internal traffic to access the internet.

- **NAT Inside**: Enabled on all VLAN interfaces (Vlan10, Vlan30, Vlan40, Vlan50, Vlan60, Vlan70).
- **NAT Outside**: Enabled on the interface connecting to the ISP (GigabitEthernet8).
- **NAT Overload**: Enabled using the interface `GigabitEthernet8`.

## Static Routes

- **Default Route**: The router has two default routes:
  - `0.0.0.0 0.0.0.0` via `GigabitEthernet8` (ISP Connection).
  - `0.0.0.0 0.0.0.0` via DHCP (used for backup).

## Access Control Lists (ACL)

The router uses **ACL 100** to allow traffic from the internal networks (VLANs) to reach external destinations:

- **Networks Allowed by ACL 100**:
  - `10.1.1.0/24` (VLAN 10)
  - `192.168.30.0/24` (VLAN 30)
  - `172.16.40.0/24` (VLAN 40)
  - `172.16.50.0/24` (VLAN 50)
  - `20.60.60.0/24` (VLAN 60)
  - `20.70.70.0/24` (VLAN 70)

## Additional Configuration

- **MGCP**: The router has some settings related to **MGCP** (Media Gateway Control Protocol) with specific behaviors configured.
- **Console, AUX, and VTY Lines**: Access is enabled via console, auxiliary, and virtual terminal lines.
- **HTTP Server**: Disabled (`ip http server` and `ip http secure-server` are turned off).

## Security

- **Password Encryption**: Disabled (`no service password-encryption`).
- **AAA**: Disabled (`no aaa new-model`).
- **Access to VTY Lines**: Login required for remote access.

## Conclusion

This configuration provides a robust setup for handling multiple VLANs, offering DHCP services for each VLAN, implementing NAT for internet access, and securing access to the router via VTY, console, and auxiliary lines. This document is designed to serve as an overview of the router configuration and can be used for future reference or troubleshooting.

# 3560cx Access Switch Configuration

This document provides a detailed overview of the running configuration for the **Skuas Access Switch (skuas-access-sw1)**.

## Overview

- **Hostname**: `skuas-access-sw1`
- **IOS Version**: 15.2
- **VLAN Configuration**: Multiple VLANs are defined for segmented networking.
- **Spanning Tree**: Rapid PVST is enabled for improved network resilience.
- **Interfaces**: A mixture of access and trunk ports, with specified VLAN assignments.
- **Security**: SSH for secure remote access, with local user authentication and encryption.
  
## Key Configurations

### 1. **Hostname and Version**

- **Hostname**: `skuas-access-sw1`
- **IOS Version**: `15.2`
  
### 2. **VLANs**

The following VLANs are configured for network segmentation:

- **VLAN 10**: Management Network
- **VLAN 30**: Home Network
- **VLAN 40**: Development Network
- **VLAN 50**: QA Network
- **VLAN 60**: Production Network
- **VLAN 70**: DMZ Network

### 3. **Interface Configuration**

- **GigabitEthernet0/1 - 4**: Access ports assigned to specific VLANs.
  - `GigabitEthernet0/1` → VLAN 10 (Management)
  - `GigabitEthernet0/2 - 4` → VLAN 40 (Dev Network)
  - `GigabitEthernet0/8 - 9` → VLAN 30 (Home Network)
  - `GigabitEthernet0/10` → VLAN 70 (DMZ Network)

- **GigabitEthernet0/13**: Trunk port, connecting to `891F router`.

- **GigabitEthernet0/14**: Disabled port (no switchport, no IP address assigned).

- **VLAN Interfaces**: The switch also has several VLAN interfaces (SVIs) configured with IP addresses for routing:
  - `Vlan10`: `10.1.1.2/24` (Management)
  - `Vlan30`: `192.168.30.2/24` (Home Network)
  - `Vlan40`: `172.16.40.2/24` (Dev Network)
  - `Vlan50`: `172.16.50.2/24` (QA Network)
  - `Vlan60`: `20.60.60.2/24` (Production Network)
  - `Vlan70`: `20.70.70.2/24` (DMZ Network)

### 4. **Spanning Tree Protocol (STP)**

- **STP Mode**: `rapid-pvst` (Rapid Per VLAN Spanning Tree) for faster convergence.
- **Extend System ID**: Ensures unique bridge IDs for each VLAN.

### 5. **Security Settings**

- **SSH Version 2**: Secure Shell (SSH) is enabled for encrypted remote access.
- **HTTP Server**: HTTP and HTTPS servers are enabled for web-based management.
- **Login Settings**: Local user database is used for login authentication via SSH on virtual terminal lines.

  - **VTY Lines 0-4**: SSH access only.
  - **VTY Lines 5-15**: Standard login, no SSH restrictions.

### 6. **Crypto Settings**

A self-signed certificate is used for secure communications:

- **Trustpoint**: `TP-self-signed-3833869440`
- **Subject Name**: `IOS-Self-Signed-Certificate-3833869440`

### 7. **Routing and MTU**

- **Routing**: IP routing is enabled.
- **MTU**: System Maximum Transmission Unit is set to `1500`.

### 8. **Access Control and User Credentials**

- **Username**: `sku` with privilege level 15.
- **Password**: Encrypted, for secure access.

### 9. **Additional Configuration**

- **IP Forwarding**: Protocol `nd` is forwarded (ND for Neighbor Discovery Protocol).
- **VTP Mode**: Set to `transparent`, ensuring the switch does not participate in VLAN database propagation.

## Conclusion

This configuration sets up a secure, well-segmented network on the `skuas-access-sw1` switch. VLANs have been configured for efficient network segmentation, spanning tree is optimized for rapid convergence, and secure remote access is enabled through SSH. The use of a self-signed certificate ensures basic encrypted communication for management tasks.
  
## Considerations for Future Changes

- Review user privilege settings and ensure that only authorized personnel have access to privilege level 15.
- For enhanced security, consider using a certificate signed by a trusted authority for SSL/TLS communications.
- Regularly update the switch firmware to ensure it remains secure and supports the latest features.

