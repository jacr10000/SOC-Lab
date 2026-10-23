# Alert: Intruder in Linux File System

In this case, we have been notified of a possible data breach, so we need to investigate if the breach occurred and reconstruct the event.

This is categorized as a Privilege Escalation-type MITRE ATT&CK tactic with potential credential access, since the intruder gained higher-level permissions to access sensitive resources.

## Context


## Analysis
The first thing we do is search for suspicious login activity. To do this, we search for the 'Accepted password' general expression inside the authentication logs. 
