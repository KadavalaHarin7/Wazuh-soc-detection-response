&#x20;Scheduled Task Detection — Wazuh Rule 100033



&#x20;Purpose



Detect Windows scheduled-task creation using Windows event telemetry.



&#x20;Detection Logic



Windows Event ID `4698` provides scheduled-task creation telemetry.



Custom Rule `100033` was used for the project-specific detection.



The rule is level 12 and references:



\- T1053.005 — Scheduled Task/Job: Scheduled Task



&#x20;Test



A controlled scheduled task was created on the Windows endpoint.



The resulting Windows event was collected by Wazuh and matched the custom

detection.



Task Scheduler lifecycle information and Sysmon process creation telemetry

were also used to provide additional investigation context.



&#x20;Investigation



Creation and execution were treated as separate observations.



The investigation considered:



\- Event ID

\- Timestamp

\- Task information

\- Process image

\- Command line

\- Parent process

\- Process ID

\- User context



Temporal proximity and shared process context were treated as supporting

evidence rather than automatic proof of causation.



&#x20;Validation Result



\*\*Validated\*\*



The project confirmed:



\- Windows Event ID 4698 telemetry

\- Custom Rule 100033

\- Scheduled-task detection

\- Sysmon/process context for investigation

