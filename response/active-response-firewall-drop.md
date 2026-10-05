&#x20;Wazuh Active Response — firewall-drop



&#x20;Purpose



This document describes the validated automated response used in the Wazuh

SOC lab for the controlled SSH brute-force scenario.



&#x20;Trigger



The response was connected to the SSH brute-force detection based on

Wazuh Rule `5712`.



The detection correlated repeated failed SSH authentication attempts.



Validated condition:



\- 8 authentication failures

\- 120-second correlation window

\- Level 10 alert

\- MITRE ATT\&CK T1110



&#x20;Response Action



The configured Active Response action was:



firewall-drop



The response used a 60-second timeout.



The purpose of the bounded timeout was to demonstrate automated containment

while ensuring that the lab response was temporary.



Workflow



Repeated SSH failures

&#x20;       │

&#x20;       ▼

Wazuh Rule 5712

&#x20;       │

&#x20;       ▼

Brute-force alert

&#x20;       │

&#x20;       ▼

Active Response

&#x20;       │

&#x20;       ▼

firewall-drop

&#x20;       │

&#x20;       ▼

Temporary block

&#x20;       │

&#x20;    60 seconds

&#x20;       │

&#x20;       ▼

Timed recovery



Validation



The response was validated using the Wazuh Active Response logs.

The logs confirmed:

1\. The response was triggered.

2\. The temporary firewall block was applied.

3\. The configured timeout elapsed.

4\. The response was subsequently removed.



This validated the response lifecycle rather than relying only on the

configuration.



Investigation Context



The SSH activity was intentionally generated from Kali Linux against the

Ubuntu endpoint as part of the controlled lab.



The response therefore demonstrates the technical workflow:

Detection → Alert → Automated Response → Timed Recovery



It should not be interpreted as evidence of a real-world compromise.



Scope



The project validated this Active Response specifically for the SSH

brute-force scenario.



Other detections in the project were used for detection and investigation;

the project does not claim that every detection automatically triggers

containment.



Safety



No production account-disable or destructive containment action was used.



The response was intentionally bounded to a temporary 60-second firewall

block within the isolated lab environment.



Validation Result



Validated



The project confirmed:

\- Rule 5712 detection

\- Active Response trigger

\- firewall-drop execution

\- 60-second timeout

\- Response removal

\- Complete response lifecycle

