

&#x20;2. `detections/encoded-powershell-rule-100031.md`





&#x20;Encoded PowerShell Detection — Wazuh Rule 100031



&#x20;Purpose



Detect encoded PowerShell activity using a custom Wazuh rule.



&#x20;Detection Logic



Custom Rule `100031` detects encoded PowerShell activity as a child of

built-in Rule `92057`.



The rule is configured at level 14 and references:



\- T1059.001 — PowerShell

\- T1027 — Obfuscated/Compressed Files and Information



&#x20;Test



A harmless encoded PowerShell `Write-Output` command was used to generate

the telemetry.



The controlled test generated the expected custom alert.



&#x20;Negative Control



A plain PowerShell test was also performed.



The plain PowerShell activity did not generate a new Rule `100031` alert.



This negative control helped distinguish encoded PowerShell activity from

ordinary PowerShell execution.



&#x20;Investigation



The alert was reviewed using the available process and command-line

context.



The detection is treated as a triage signal. An encoded PowerShell command

by itself is not considered proof of malicious activity.



&#x20;Validation Result



\*\*Validated\*\*



The project confirmed:



\- Encoded PowerShell telemetry

\- Custom Rule 100031

\- Expected alert generation

\- Negative control behavior

