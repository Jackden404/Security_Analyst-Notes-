This final review summarizes the fundamental concepts of Incident Reporting and Documentation in a Security Operations Center. Use this reference sheet to lock in your knowledge before starting the module assessment.

---

##  Key Takeaways Reference Sheet

[!IMPORTANT] **1. Timeline Timezone Standard:** Incident timelines should use **UTC, because different log sources use different timezones and UTC provides a common reference**. Using local time zones leads to confusion when combining logs from multiple globally distributed hosts and gateways.

[!TIP] **2. Executive Summary Audience:** The Executive Summary is written for **Leadership and non-technical stakeholders who need to understand impact and response**. It avoids deep technical jargon and focuses entirely on operational, financial, and regulatory impact.

[!NOTE] **3. Core Performance Metric (MTTR):** In SOC metrics, MTTR stands for **Mean Time to Respond - how long between detection and containment**. It tracks the speed of your incident handlers.

[!WARNING] **4. What to Avoid in Documentation:** Incident documentation must avoid **vague language like "some malware" or "checked logs"**. Effective incident logs must state exact filenames, hashes, IP addresses, queries executed, and specific results.

[!IMPORTANT] **5. Interpreting Operational Metrics:** What makes SOC metrics valuable for decision-making is **tracking trends over time to identify whether security posture is improving or degrading**. Snapshot figures lack the context necessary to show baseline improvement.

[!NOTE] **6. Structure of the Appendix:** An incident report Appendix should contain **raw evidence references, IOC lists, log excerpts, and chain of custody forms**. It acts as the evidence vault backing the claims made in the summary.

---

## 📊 Quick Summary Table of Incident Documentation

|Report Component|Target Audience|Primary Focus|
|---|---|---|
|**Executive Summary**|Board of Directors, CEO, Legal Counsel|Business impact, operational disruption, containment status. SOHO framework format.|
|**Incident Timeline**|Incident Responders, Compliance Auditors|Chronological, UTC-normalized logging of the attack sequence. (Timestamp, Source, Event, Actor).|
|**Technical Analysis**|Security Engineers, EDR Administrators|Deep dive into attack vectors, IOC hashes, lateral movement paths, MITRE ATT&CK mapping.|
|**Appendix**|Forensic Analysts, Regulators, Courts|Verifiable raw evidence, command output logs, signed chain of custody records.|

## Mission Verification

Input the correct mission parameters to confirm processing.

1

What timezone should incident timelines use and why?

UTC, because different log sources use different timezones and UTC provides a common reference

EST, because most enterprise systems default to Eastern

Any timezone, as long as it is consistent across the timeline

Local timezone, because it matches what analysts see on their screens

2

Who is the Executive Summary written for?

Regulatory authorities reviewing compliance

The technical team performing the investigation

Legal counsel preparing for litigation

Leadership and non-technical stakeholders who need to understand impact and response

3

What does MTTR stand for in SOC metrics?

Mean Time to Respond - how long between detection and containment

Mean Time to Report - how long documentation takes

Mean Time to Recover - how long system restoration takes

Mean Time to Remediate - how long patching takes

4

What should incident documentation avoid?

Vague language like "some malware" or "checked logs"

UTC timestamps and specific log references

Including indicators of compromise

Mentioning the names of systems involved

5

What makes SOC metrics valuable for decision-making?

Tracking trends over time to identify whether security posture is improving or degrading

Measuring only the metrics your SIEM generates automatically

Reporting the highest possible numbers to impress management

Comparing your metrics to unrelated industries

6

An incident report Appendix should contain:

A rewritten executive summary in simpler language

Raw evidence references, IOC lists, log excerpts, and chain of custody forms

Marketing materials about your security tools

The personal opinions of each analyst involved