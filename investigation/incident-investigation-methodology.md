&#x20;Wazuh SOC Incident Investigation Methodology



&#x20;Purpose



This document describes the investigation approach used in the Wazuh SOC

lab.



The objective was to move from an alert to supporting telemetry and determine

whether the activity represented expected controlled testing, benign or

authorized activity, suspicious activity requiring follow-up, or an

inconclusive result.



&#x20;Investigation Process



The investigation followed this general sequence:





Wazuh Alert

&#x20;   │

&#x20;   ▼

Identify Rule and Severity

&#x20;   │

&#x20;   ▼

Establish Timestamp

&#x20;   │

&#x20;   ▼

Identify Agent / Host

&#x20;   │

&#x20;   ▼

Review Original Event

&#x20;   │

&#x20;   ▼

Correlate Supporting Telemetry

&#x20;   │

&#x20;   ▼

Assess User / Process / Network Context

&#x20;   │

&#x20;   ▼

Determine Disposition



Initial Alert Review



For each alert, I reviewed the available:

\- Rule ID

\- Alert severity

\- Timestamp

\- Agent

\- Source and destination information

\- Original event message



The alert was first treated as an indicator requiring investigation rather

than automatically being classified as malicious.



Host and Process Context



Where available, investigation included:

\- Account

\- Process image

\- Command line

\- Parent process

\- Process ID

\- File path

\- Integrity context



Sysmon telemetry was used to provide additional Windows process context.



For scheduled-task investigations, Windows Task Scheduler events and Sysmon

process creation telemetry were used together where available.



Network Context



For network-related alerts, the investigation considered:

\- Source address

\- Destination address

\- Source port

\- Destination port

\- Protocol

\- Suricata event information



Suricata EVE JSON telemetry was forwarded into Wazuh and used as supporting

network context.



Temporal Correlation



Exact timestamps were used when correlating related events.



Temporal proximity was treated as supporting evidence rather than automatic

proof that two events were causally related.



The same approach was used when reviewing process relationships and related

endpoint activity.



Example — SSH Brute Force



The SSH investigation started with Wazuh Rule 5712.



The alert represented correlated failed SSH authentication attempts.



The investigation considered:

\- Authentication failure count

\- Correlation window

\- Source information

\- Target Ubuntu endpoint

\- Original SSH authentication events



The activity was intentionally generated from Kali Linux as part of the

controlled lab.



The associated Active Response was then reviewed separately through the

Wazuh response logs.



Example — Encoded PowerShell



The encoded PowerShell investigation used Custom Rule 100031.



The investigation reviewed the PowerShell process and command-line context.



A harmless encoded Write-Output test was used for validation.



A plain PowerShell negative control did not generate a new 100031 alert.



The detection was therefore treated as a triage signal rather than proof of

malicious PowerShell activity.



Example — Scheduled Task



The scheduled-task investigation used Windows Event ID 4698 and Custom

Rule 100033.

The investigation considered:

\- Task creation event

\- Timestamp

\- Task information

\- Process image

\- Command line

\- Parent process

\- Process ID

\- User context



Task creation and later execution were treated as separate observations.

Sysmon process telemetry provided additional context for process execution.



Example — File Integrity Monitoring



The FIM investigation used Wazuh telemetry from:

C:\\SOC-FIM-Test



A controlled modification of chain-test.txt generated the expected FIM

alerts.

The investigation considered:

\- File path

\- Rule ID

\- Timestamp

\- File integrity information

\- Hash changes

A read-only negative control did not generate another Custom Rule 100034

alert.



Example — Suricata Telemetry



Suricata EVE JSON data was used to investigate network telemetry.



The project validated custom ICMP Echo Request detection.



A TCP nc check was used as a negative control for the ICMP-specific rule.



The TCP probe did not match the ICMP-specific detection.



Disposition

The investigation framework used the following classifications:



Expected Test Activity

Activity intentionally generated as part of the controlled SOC lab.



Benign / Authorized

Activity that was legitimate or authorized and did not indicate malicious

behavior.



Suspicious — Follow-Up Required

Activity with security-relevant characteristics requiring additional

investigation.



Inconclusive

Insufficient evidence to make a stronger determination.



Evidence Handling

Investigation conclusions were based on available telemetry and captured

evidence.



The project avoided treating a single event as definitive proof of

compromise when additional context was required.



Screenshots and supporting evidence were retained as part of the project

documentation.

Sensitive credentials, tokens, and authentication material are not intended

for public repository publication.



Investigation Principle



The core investigation approach was:

Alert

&#x20; ↓

Verify telemetry

&#x20; ↓

Understand context

&#x20; ↓

Correlate related events

&#x20; ↓

Assess security significance

&#x20; ↓

Document evidence

&#x20; ↓

Assign disposition



This approach was used to keep the investigation evidence-driven rather than

assuming that every generated alert represented a real attack.

