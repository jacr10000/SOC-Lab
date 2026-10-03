# SOC-Lab

This repository contains the documentation of attack simulations conducted within the virtualization software VirtualBox.

There are three virtual machines involved:

| Role          | Virtual Machine | Private IP address |
| ------------- | --------------- | ------------------ |
| Victim        | Windows 11      | 10.10.10.4         |
| Attacker      | Ubuntu          | 10.10.10.6         |
| SIEM          | Wazuh           | 10.10.10.3         |

The three VMs are connected to each other through a Network Address Translation (NAT) network, which allows communication among them by using their private IP addresses. The private IP addresses are unique only to the NAT network, and any requests to devices outside the network (like HTTP requests) are translated through the NAT so they can reach the host PC's private IP address.

Sysmon, a system monitoring tool, was installed in the Windows VM to track and log its activities. Then, its logs were configured to be forwarded to the Wazuh VM, allowing it to monitor and analyze the event data.

Wireshark was also installed in the Windows VM as a network packet analyzer, to inspect the network traffic.

In addition, a port forwarding rule was created so that the host PC's local host could reach the Wazuh dashboard through the browser, by forwarding the request to port 443 (for HTTPS requests) of the Wazuh VM.

