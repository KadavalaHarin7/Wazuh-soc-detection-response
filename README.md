&#x20;Wazuh SIEM Implementation & Custom Security Threat Detection in a Virtual Lab



This project is a controlled multi-endpoint SOC lab built around Wazuh.



I used Wazuh to centralize endpoint and network telemetry, build and test

custom detection rules, investigate alerts, and validate a bounded automated

response.



The lab uses Windows and Ubuntu endpoints for telemetry, Kali for authorized

test activity, Sysmon for Windows process context, and Suricata for network

telemetry.



&#x20;What I worked on



\- Deployed a Docker-hosted Wazuh environment

\- Enrolled Windows and Ubuntu Wazuh agents

\- Collected Windows event telemetry and Sysmon process events

\- Collected Linux SSH authentication logs

\- Forwarded Suricata EVE JSON telemetry into Wazuh

\- Built and tested custom Wazuh detection rules

\- Investigated alerts using rule, timestamp, agent, process, file, and network context

\- Validated Wazuh Active Response for a controlled SSH brute-force test

\- Preserved evidence and documented validation limitations



&#x20;Architecture





&#x20;                   Kali Linux

&#x20;                Authorized Testing

&#x20;                      │

&#x20;            ┌─────────┴─────────┐

&#x20;            │                   │

&#x20;       SSH test traffic      ICMP test

&#x20;            │                   │

&#x20;            ▼                   ▼

&#x20;     Ubuntu Endpoint      Suricata EVE JSON

&#x20;      Wazuh Agent                │

&#x20;      SSH logs                   │

&#x20;            │                    │

&#x20;            └──────────┬─────────┘

&#x20;                       │

&#x20;                       ▼

&#x20;                Wazuh Manager

&#x20;                       │

&#x20;             Rule Matching / Alerts

&#x20;                       │

&#x20;             ┌─────────┴─────────┐

&#x20;             │                   │

&#x20;      Windows Endpoint       Network Telemetry

&#x20;       Wazuh Agent              │

&#x20;       Sysmon                   │

&#x20;       Windows Events            │

&#x20;             │                   │

&#x20;             └─────────┬─────────┘

&#x20;                       ▼

&#x20;                Analyst Triage

&#x20;                       │

&#x20;             ┌─────────┴─────────┐

&#x20;             │                   │

&#x20;         Investigation      Bounded Response

&#x20;                                 │

&#x20;                          firewall-drop

&#x20;                            60 seconds



Detection Coverage



1\. SSH Brute Force



Kali generated repeated invalid SSH logins against the Ubuntu endpoint.



Wazuh Rule 5712 correlated:

\- 8 authentication failures

\- 120-second correlation window

\- Level 10 alert

\- MITRE ATT\&CK T1110



The matching alert was connected to Wazuh Active Response using

firewall-drop.



The response used a 60-second timeout. The response logs showed the

temporary block being started and subsequently removed, validating both the

response and timed recovery in the controlled lab.

&#x20;                            

2\. Encoded PowerShell



Custom Rule 100031 detects encoded PowerShell activity as a child of

built-in Rule 92057.



The rule is level 14 and references:

\- T1059.001 — PowerShell

\- T1027 — Obfuscated/Compressed Files and Information

A harmless encoded Write-Output test generated the custom alert.



A plain PowerShell negative test did not generate a new 100031 alert.



The project treats encoded PowerShell as a triage signal rather than proof

of malicious activity.



3\. Scheduled Task Detection



Windows scheduled-task creation was collected through Windows Event ID 4698.



Built-in Rule 60228 handled the event, while custom Rule 100033 provided

the project-specific detection.



The custom rule is level 12 and references T1053.005.



The lab also used Task Scheduler lifecycle events and Sysmon process creation

to corroborate a later manual execution.



Creation and execution were treated as separate observations during

investigation.



4\. File Integrity Monitoring



Wazuh real-time File Integrity Monitoring monitored:

C:\\SOC-FIM-Test



A controlled modification generated the native Wazuh Rule 550 alert and

custom Rule 100034.



The custom rule is level 12.



A controlled edit to chain-test.txt produced changed file hashes.



A read-only negative test did not generate another 100034 alert.



The project does not treat a generic file modification as inherently

malicious.



5\. Suricata Network Telemetry



Suricata 6.0.4 was configured on the Ubuntu endpoint and its EVE JSON

telemetry was forwarded to Wazuh.



The lab validated:

\- Suricata EVE JSON ingestion

\- Wazuh processing of Suricata telemetry

\- Custom ICMP Echo Request detection

\- A TCP nc check as a negative control for the ICMP-specific rule



The custom ICMP signature fired when Kali generated an ICMP echo request.

A TCP probe did not match the ICMP-specific detection.



Sysmon Integration



Sysmon was used to provide additional Windows process context.



Where available, the telemetry provided:

\- Process image

\- Command line

\- Process ID

\- Parent process

\- User

\- Integrity context



Sysmon and Windows Task Scheduler telemetry were used to corroborate the

scheduled-task investigation.



Investigation Methodology



For each alert, I reviewed available:

\- Rule ID and severity

\- Timestamp and time zone

\- Agent

\- Source and destination

\- Event location

\- Original event message

\- Account

\- Process image

\- Command line

\- Parent process

\- PID

\- File path

\- Network information

\- Related events



I used exact identifiers and timestamps when correlating events.



Temporal proximity or a shared parent process was treated as supporting

context rather than automatic proof of causation.



Results were classified as expected test activity, benign/authorized,

suspicious requiring follow-up, or inconclusive.



Active Response



The verified automated response in this project is the 60-second

firewall-drop action associated with the SSH brute-force detection.



The response was validated through the Active Response logs rather than

only checking configuration.



Other detections were used for detection and investigation. The project does

not claim blanket automated containment for every alert.



Validation



The following capabilities were validated:

Capability	                                Result

Wazuh deployment	                        Validated

Windows agent	                                Validated

Ubuntu agent	                                Validated

Sysmon telemetry	                        Validated

Linux SSH telemetry	                        Validated

Suricata EVE JSON	                        Validated

SSH brute-force Rule 5712	                Validated

SSH Active Response	                        Validated

Encoded PowerShell Rule 100031          	Validated

Scheduled-task Rule 100033	                Validated

FIM Rule 100034                         	Validated

Custom ICMP detection	                        Validated



Known Limitations



The following were intentionally excluded from the project's validated

claims:

\- Custom reconnaissance correlation

\- Rule 100035

\- Scan-specific Suricata/Nmap detection



These tests were not reliably validated in the final project state.



The project demonstrates controlled lab validation. It does not claim

production efficacy, measured false-positive rates, adversary attribution,

or a confirmed real-world compromise.



Evidence Handling



The original evidence archive is retained separately from this public

repository.



Public evidence should use redacted copies only.



In particular, the SSH attack screenshot contains visible credential

material and must not be published without redaction.



Passwords, tokens, cluster keys, and unnecessary personal information are

not included in the public repository.



Project Documentation



The detailed documentation is available in:

\- docs/Wazuh-SOC-Technical-Documentation.pdf

\- docs/SOC-Incident-Investigation-Report.pdf

\- docs/Wazuh-SOC-Operational-Runbook.pdf

Additional repository sections contain detection, response, investigation,

and architecture material.



Technologies



\- Wazuh

\- Wazuh Agents

\- Sysmon

\- Suricata

\- Windows Event Logging

\- Linux SSH Logs

\- Docker

\- Kali Linux

\- Windows 11

\- Ubuntu

\- MITRE ATT\&CK



 Project Outcome

This lab provided hands-on experience across the SOC workflow, from endpoint and network telemetry collection through custom detection engineering, alert investigation, evidence validation, and bounded automated response.

Key capabilities demonstrated:

- Deploying and operating a multi-endpoint Wazuh SOC environment
- Engineering and validating custom Wazuh detection rules
- Correlating endpoint, authentication, process, file, and network telemetry
- Investigating alerts using contextual event and process information
- Applying MITRE ATT&CK techniques to detection scenarios
- Performing positive and negative detection testing
- Validating bounded automated response using Wazuh Active Response
- Documenting investigation methodology, evidence, and validation limitations

