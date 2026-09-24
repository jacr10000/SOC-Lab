# SOC-Lab

This repository contains the documentation of attack simulations conducted within the virtualization software VirtualBox.

There are three virtual machines involved:
  1. Windows 11 VM (Victim)
  2. Ubuntu VM (Attacker)
  3. Wazuh VM (SIEM)

The three VMs are connected to each other through a Network Address Translation (NAT) network, which allows communication among them by using their private IP addresses. The private IP addresses are unique only to the NAT network, and any requests to devices outside the network (like HTTP requests) are translated through the NAT so they can reach the host PC's private IP address.
