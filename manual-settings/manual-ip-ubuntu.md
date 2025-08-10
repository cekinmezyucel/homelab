### Pi-Hole DNS Server Prerequisites

To ensure reliable operation of Pi-hole as your network’s DNS server, you must meet several prerequisites:

- **Static IP Requirement:**

  - Pi-hole must have a static IP address that is outside the automatic DHCP IP range managed by your TP-Link Deco devices. This prevents IP conflicts and ensures clients can always reach Pi-hole.
  - If Pi-hole’s IP is within the DHCP range, another device could be assigned the same IP, causing network issues.
  - For TP-Link Deco, check your DHCP range in the Deco app and assign Pi-hole an IP outside this range.

- **How to Set a Static IP:**

  - See the section below: [Setting a Static IP Address on Ubuntu Server](#setting-a-static-ip-address-on-ubuntu-server-netplan-ubuntu-2404-supported)

- **Configure Deco DHCP DNS:**

  - After setting a static IP for Pi-hole, go to your TP-Link Deco app > More > Advanced > DHCP Server.
  - Set the DNS server to Pi-hole’s static IP (e.g., 192.168.68.35).
  - Do not set the DNS in the "Internet Connection" —set it in the DHCP server settings so all devices on your network use Pi-hole for DNS.

- **Summary:**
  - Assign Pi-hole a static IP outside the DHCP range.
  - Set Deco’s DHCP DNS to Pi-hole’s IP.
  - This ensures all network devices use Pi-hole for DNS filtering and ad blocking.

# Setting a Static IP Address on Ubuntu Server (Netplan, Ubuntu 24.04 Supported)

This guide explains how to set a static IP address on your Ubuntu server using Netplan. Netplan is the default network configuration tool for Ubuntu 18.04 and newer, including Ubuntu 24.04.

**Note for Ubuntu 24.04:**
Ubuntu Server uses `renderer: networkd` by default. Ubuntu Desktop uses `renderer: NetworkManager`. This guide is for Ubuntu Server. If you are on Desktop, use `renderer: NetworkManager` and follow desktop-specific steps in the [Netplan documentation](https://netplan.io/).

## Step-by-Step Instructions

1. **Locate your Netplan configuration file**

- Netplan config files are usually found in `/etc/netplan/`.
- Example: `/etc/netplan/01-netcfg.yaml`

2. **Back up your Netplan config file before making changes**

- Run this command to create a backup:
  ```sh
  sudo cp /etc/netplan/01-netcfg.yaml /etc/netplan/01-netcfg.yaml.bak
  ```

3. **Edit the Netplan YAML file**

- Open the file with your preferred editor, e.g.:
  ```sh
  sudo nano /etc/netplan/01-netcfg.yaml
  ```

3. **Replace the contents with the following example:**

   ```yaml
   network:
     version: 2
     renderer: networkd
     ethernets:
       eno1:
         dhcp4: no
         addresses:
           - 192.168.68.35/22
         routes:
           - to: default
             via: 192.168.68.1
         nameservers:
           addresses:
             - 192.168.178.1
             - 192.168.68.1
             - 8.8.8.8
             - 8.8.4.4
   ```

   - Replace `eno1` with your actual network interface name if different (use `ip link` to check).
   - Adjust the IP, gateway, and nameservers as needed for your network.

4. **Apply the configuration:**

   ```sh
   sudo netplan apply
   ```

5. **Verify your settings:**
   ```sh
   ip addr show eno1
   ip route
   cat /etc/resolv.conf
   ```

## Notes

- If you lose connectivity, you can revert changes by editing the Netplan file again or restoring from backup. To restore from backup:
  ```sh
  sudo cp /etc/netplan/01-netcfg.yaml.bak /etc/netplan/01-netcfg.yaml
  sudo netplan apply
  ```
- For more details, see the [Netplan documentation](https://netplan.io/).
