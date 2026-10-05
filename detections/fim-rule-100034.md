&#x20;File Integrity Monitoring — Wazuh Rule 100034



&#x20;Purpose



Detect controlled file modifications using Wazuh File Integrity Monitoring

(FIM).



&#x20;Monitored Path





C:\\SOC-FIM-Test



Detection Logic



A controlled modification generated:

\- Native Wazuh Rule 550

\- Custom Rule 100034



Custom Rule 100034 is configured at level 12.



Test



The file:

chain-test.txt



was deliberately modified inside the monitored directory.

Wazuh detected the change and reported modified file hashes.



Negative Control



A read-only test was performed against the monitored location.



The read-only activity did not generate another Rule 100034 alert.



This helped distinguish a file modification from a simple file access.



Investigation



The alert was reviewed using:

\- File path

\- Event timestamp

\- Rule ID

\- File integrity information

\- Hash changes



The project does not treat a generic file modification as inherently

malicious.



The security significance depends on the affected file, user context,

process, and surrounding activity.



Validation Result



Validated



The project confirmed:

\- FIM monitoring

\- File modification detection

\- Rule 550 correlation

\- Custom Rule 100034

\- Hash-change telemetry

\- Negative control behavior

