# Alert: Suspicious Remote Logon and Commands

In this case, we investigate an alert provoked by a remote logon into an administrator account, followed by several alerts of CMD commands used to reveal information.

This could be categorized as a Discovery-type MITRE ATT&CK tactic with potential lateral movement, as it involves obtaining system, accounts and network information by taking advantage of the privileges of a compromised user.

## Context

If a malicious actor gets a hold of a user's credentials (username, password, etc.), they can attempt to access their account and, if successful, they can use the user's privileges to explore the system and network properties, steal valuable information, or plan future attacks. 

The Server Mesage Block (SMB) protocol allows communication between devices, where one of them acts as a server and the other one acts as a client, who can send requests to the server.  For authentication, NTLM is a protocol that can be used by SMB to accept a set of recognized user-password pairs, to validate access to the server's information. 

By using a remote host and the right credentials, an attacker can impersonate a user using SMB to steal sensitive data or assets, if additional security measures are not implemented.

## Analysis

The following image shows the description of the alert:



We pay attention to the following fields:

| Field          | Content | Comments |
| -------------  | --------------- | ------------------ |
| Target User Name        | dexterb         | The compromised user |
| IP Address           | 10.10.10.6      | Not the IP address from the host computer. Effectively, the logon came from an external source        |
| LM Package Name        | NTLM V2         | The authentication protocol used by the attacker |
| Timestamp      | Oct 10, 2026 @ 08:31:58.767          | After this, the CMD command alerts started showing up, coming from the same user |

Some of the commands listed in the following alerts are:
- whoami
- hostname
- ipconfig /all
- net user
- tasklist /svc
- netstat -ano

Since we have the attacker's IP address, we can further investigate the situation through our packet capture in Wireshark:



We observe that the communication protocol used by the attacker was SMB2 on port 445, whose entries take place after the logon alert's timestamp. SMB3 also allowed their commands to be encrypted, so we cannot inspect them in Wireshark and rely on the Wazuh alerts to know what information was obtained.

## Conclusion

Given that it was confirmed that the attacker somehow stole the credentials of the compromised user, the most urgent step is containment. This means, we need to block the attacker's IP address, log the user out of every session and (temporarily) disable the user, to avoid more information getting leaked. Restricting access to port 445 would also be advisable.

After that, a discussion should be held with the person to determine how and why the information got leaked, and how a similar situation could be avoided in the future for him and all colleagues.

Additionally, the SMB communication should be restricted to specific, known servers. Discussions should also take place about implementing SMB signing or other alternatives, as NTLM is weak and authentication relies only on the password.
