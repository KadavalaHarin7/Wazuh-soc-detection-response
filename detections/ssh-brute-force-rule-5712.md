&#x20;SSH Brute-Force Detection — Wazuh Rule 5712



&#x20;Purpose



Detect repeated failed SSH authentication attempts against a monitored

Linux endpoint.



&#x20;Detection



Wazuh Rule `5712` was used to correlate repeated SSH authentication failures.



The validated lab condition was:



\- 8 authentication failures

\- 120-second correlation window

\- Level 10 alert

\- MITRE ATT\&CK T1110



&#x20;Test



Kali Linux generated repeated invalid SSH authentication attempts against

the Ubuntu endpoint.



The resulting Wazuh alert confirmed that the authentication failures were

correlated into a brute-force detection.



&#x20;Response



The detection was connected to Wazuh Active Response using:



firewall-drop

The response used a 60-second timeout.

The Active Response logs were reviewed to verify that the temporary block
was started and subsequently removed.

Investigation

The alert was reviewed using:
- Rule ID
- Alert severity
- Timestamp
- Agent
- Source information
- Original authentication event

The activity was generated intentionally as part of the controlled lab test.

Validation Result

Validated

The project confirmed:
- SSH authentication telemetry
- Rule 5712 correlation
- Alert generation
- Active Response execution
- Timed response recovery
