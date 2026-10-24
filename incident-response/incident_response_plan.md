# NoorPay Incident Response Plan

Based on **NIST SP 800-61 Rev. 2**. Covers unauthorized access to customer financial data, using the 2024 breach of 12,000 records as the reference case.

## 1. Preparation

### Incident Response Team

NoorPay has no CISO, so the team is drawn from current staff plus external legal counsel.

| Role | Assigned to | Responsibilities | Escalation |
|---|---|---|---|
| Incident Commander | IT Manager | Declares incidents, coordinates the response, makes containment decisions, owns the timeline | To CEO if more than 1,000 records are affected or regulators must be notified |
| Compliance & Notification Lead | Compliance Officer | Tracks GDPR/PDPL 72-hour deadlines, drafts regulator notices, contacts SDAIA and the EU authority | Reports to IT Manager; brings in External Counsel for legal review |
| Customer Communication Lead | Customer Support Lead | Customer messaging, notification templates, post-breach enquiries | Reports to Compliance Officer; media enquiries go to CEO |
| Legal Advisor | External Counsel (retained) | Reviews all external communications, advises on notification duties | Engaged within 2 hours of incident confirmation |

### Preparation measures

1. **Log retention:** keep cloud access logs for at least 12 months. The 2024 breach could not be investigated because no logs existed.
2. **Offline regulator contact list:** SDAIA and EU authority contacts, kept on paper and digitally, so they're available even if systems are down.
3. **Pre-approved notification templates:** an Arabic one for SDAIA and an English one for the EU authority, approved by legal in advance to save time inside the 72-hour window.
4. **Severity table defined in advance**, shown below.

## 2. Detection & Analysis

### Indicators of compromise

| IoC | Description |
|---|---|
| Off-hours bulk export | Bulk export from cloud storage or the database between 22:00 and 06:00 AST, from an IP outside the office network and known cloud services |
| Credential stuffing then success | 10+ failed logins on an employee or admin account, then a success from an unfamiliar location or IP |
| Unusual API activity | More than 100 calls to transaction-history endpoints in under 5 minutes from a service account not normally used this way |

### Severity classification

| Severity | Criteria | Response |
|---|---|---|
| Low | Suspicious activity, no confirmed access; fewer than 10 accounts | Investigate within 24 h; log it |
| Medium | Confirmed access; 10–499 records; no financial or ID data | Contain within 4 h; notify IT Manager |
| High | 500–11,999 records; financial or ID data involved | Contain within 2 h; engage counsel; draft regulator notice |
| Critical | 12,000+ records, transaction histories and ID documents together, or system-wide compromise | Immediate containment; full team within 1 h; notify SDAIA and EU authority within 72 h |

## 3. Containment, Eradication & Recovery

### First two hours

| Time | Action | Owner |
|---|---|---|
| 0:00 | Declare incident and record the time of discovery. This starts the 72-hour clock under PDPL and GDPR | IT Manager |
| 0:15 | Revoke API tokens and cloud IAM credentials for affected service accounts. Don't shut systems down; preserve the logs | IT Manager |
| 0:30 | Apply a deny-all policy to affected storage buckets, keeping data and logs intact | IT Manager |
| 0:45 | Notify External Counsel, start the SDAIA draft, record the exact 72-hour deadline | Compliance Officer & Counsel |
| 1:00 | Brief internal staff; no external statements without counsel approval | Customer Support Lead |
| 1:30 | Capture and preserve access logs; take cloud snapshots for forensics; timestamp every action | IT Manager |
| 2:00 | Full team call: confirm severity and notification drafts; decide on an external forensic firm | Full team |

### Eradication & recovery

1. Analyze logs to find the attack vector: which credentials, endpoints and data were involved, and how much.
2. Close the vulnerability before restoring anything: rotate credentials, fix the misconfiguration, close the entry point.
3. Search the rest of cloud storage for other attack vectors to confirm the full scope.
4. Restore from the last known-good backup only once the vector is closed.
5. Monitor restored services for 48 hours, with daily log review by the IT Manager.

### Regulatory notification

| Regulation | Notify | Deadline | Content |
|---|---|---|---|
| Saudi PDPL Art. 20 | SDAIA via official portal | 72 h from awareness | Nature of breach, data categories, record count, likely consequences, measures taken |
| GDPR Art. 33 | Lead EU supervisory authority | 72 h from awareness | Same as PDPL, plus DPO contact and approximate number of data subjects |
| GDPR Art. 34 | Affected EU individuals | Without undue delay if high risk | Plain-language description, contact point, consequences, steps taken |

## 4. Post-Incident Activity

### Review template (complete within 2 weeks of closure)

| Field | Entry |
|---|---|
| Incident ID | e.g. INC-2026-001 |
| Date of incident / review | |
| Reviewed by | IT Manager, Compliance Officer, External Counsel |
| Severity | Low / Medium / High / Critical |
| **Timeline** | Timestamp, event |
| **Root cause** | Initial attack vector? Which control failed? Why was detection delayed? |
| **Lessons learned** | What worked? What failed or was delayed? What control would have prevented it? |
| **Corrective actions** | Action, owner, priority, deadline, status |

### Feeding results back into the risk register

After each review, the IT Manager updates the risk register in CISO Assistant:

- **Incident matches an existing risk:** raise its likelihood to reflect the real event.
- **New risk found:** add it as a new scenario.
- **Corrective action fully implemented:** lower likelihood or impact and record the control as existing.
