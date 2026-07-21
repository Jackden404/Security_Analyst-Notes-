# 1. Defense in Depth
## Layered Security

Perimeter: firewalls, IDS/IPS. 
Network: segmentation, monitoring. 
Host: antivirus, EDR. 
Application: input validation, auth. 
Data: encryption, access controls.

## Security Controls

Preventive: stop attacks. 
Detective: identify attacks. 
Corrective: respond and recover.
## Control Effectiveness

Measure: detection rate, response time, false positive rate, coverage percentage.
## Defense in Depth Examples

Email gateway + endpoint + network monitoring. 
Authentication + authorization + audit logging. Backup + DR + BCP.

---
# 2. Zero Trust Architecture

## Zero Trust Principles

Never trust, always verify. Verify explicitly, least privilege access, assume breach.
## Micro-Segmentation

Small isolated segments with individual access controls. Lateral movement becomes extremely difficult.
## Identity-Based Access

Access based on: user identity/role, device health, location/time, risk score.

## Implementation Steps

1. Identify sensitive data 
2. Map data flows 
3. Implement strong identity verification 
4. Segment by identity 
5. Monitor all access 
6. Continuously validate trust.

---
# 3. Network Segmentation

## VLAN Design

Segment by function: DMZ (public), Production (internal), Development (test), Management (admin), Guest (visitor).
## DMZ Architecture

Place external-facing services in DMZ: web servers, email gateways, VPN concentrators, DNS servers.
## Micro-Segmentation

Smaller segments within VLANs: application-tier isolation, database-tier isolation, user-based segmentation.
## Network Monitoring

Monitor between segments: flow analysis, alert on unexpected cross-segment traffic, track baselines, detect lateral movement.

---
# 4. Security Tool Integration
## SIEM + EDR + NDR

Combine for comprehensive visibility: SIEM for log analysis, EDR for endpoint visibility, NDR for network analysis. Benefits: correlated alerts, reduced blind spots, faster investigation.

## SOAR Playbooks

Automate: alert triage, phishing analysis, user account actions, evidence collection.

## API Integrations

Pull threat intel into SIEM, push alerts to ticketing, sync between platforms.

## Tool Selection

Evaluate: integration capabilities, scalability, cost, team expertise.

---
# 5. Security Monitoring Design
## Log Collection

Centralize: syslog forwarders, endpoint agents, API connectors, database audit logs.
## Monitoring Use Cases

Authentication anomalies, privilege escalation, data exfiltration, malware execution, policy violations.

## Alert Routing

Critical: page IR team. High: Slack to SOC. Medium: Email to queue. Low: Dashboard review.

## Capacity Planning

Plan for: log volume, storage, processing, licenses.

---

