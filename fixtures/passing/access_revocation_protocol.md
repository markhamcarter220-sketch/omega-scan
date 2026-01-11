# Access Revocation Protocol (ARP-1)

## 1. Purpose and Scope

This framework governs the revocation of system access privileges when abuse is detected.

**Scope:**
- Access suspension (temporary)
- Access revocation (permanent)
- Appeal and restoration procedures

**Out of Scope:**
- Data deletion
- External enforcement actions
- Punishment beyond access removal
- Disclosure to third parties

---

## 2. Definitions (Definition Lock)

**System Access**
The ability to authenticate to and use designated system capabilities. Access is either:
- ACTIVE (can authenticate)
- SUSPENDED (temporarily blocked, ≤ 30 days)
- REVOKED (permanently blocked)

**Abuse Event**
An event satisfying ALL of the following:
1. Violation of documented acceptable use policy (AUP)
2. Confirmed by automated logging AND human review
3. Severity classification of MEDIUM or higher per abuse taxonomy (Appendix A)

**Access Controller (AC)**
A human designated by the System Owner to execute revocation decisions. Must have:
- Technical capability to modify access control lists
- Training certification (ARP-CERT-001 or higher)
- Active designation in the Access Controller Registry

**System Owner**
The legal or organizational entity with formal ownership of the system being protected. Identified in the System Charter.

**Abuse Taxonomy**
A bounded classification system with three severity levels:
- LOW: policy violation with no user harm or data exposure
- MEDIUM: policy violation with limited user harm or minor data exposure
- HIGH: policy violation with severe user harm, major data exposure, or system integrity threat

(See Appendix A for complete taxonomy and examples)

---

## 3. Authority Declaration

### 3.1 Authority Holders

**Primary Authority: Access Controller (AC)**
- Power: Execute suspension and revocation decisions
- Constraint: May only act on confirmed Abuse Events
- Scope: Limited to access modification; no data deletion or disclosure powers

**Final Authority: System Owner**
- Power: Appoint/revoke Access Controllers, override revocation decisions, amend this protocol
- Constraint: Must document all overrides with rationale
- Scope: All matters within this protocol's domain

### 3.2 Source of Legitimacy

- **Access Controller legitimacy:** Derived from appointment by System Owner and technical certification
- **System Owner legitimacy:** Derived from System Charter (external document, must be provided)

If System Charter does not exist or is inaccessible, this framework MUST NOT be instantiated.

### 3.3 Revocation Mechanism

**Access Controller Revocation:**
- System Owner may revoke AC designation at any time
- Revocation takes effect immediately upon registry update
- No cause required, but documentation encouraged

**System Owner Replacement:**
- Governed by System Charter (external to this protocol)
- This framework freezes if System Owner authority becomes unclear

### 3.4 Conflict Resolution Rule

**AC-to-AC conflict:**
If multiple ACs disagree on a revocation decision, system access remains ACTIVE until:
1. System Owner makes final decision, OR
2. ACs reach consensus via majority vote (if ≥3 ACs)

**AC-to-Owner conflict:**
System Owner decision is final. AC may document objection but must comply.

**Ownership dispute:**
If System Owner authority is disputed, this framework FREEZES. No revocations may occur until dispute resolves.

---

## 4. Irreversibility Control

### 4.1 Reversible Actions

- SUSPENSION (temporary, auto-expires after ≤ 30 days)
- REVOCATION (permanent but appealable)

### 4.2 Irreversible Actions

None. All actions under this protocol are reversible via:
- Appeal process (Section 6)
- System Owner override
- Expiration (for suspensions)

### 4.3 Restoration Safeguards

Access restoration after revocation requires:
1. Successful appeal, OR
2. System Owner override with documented rationale

Suspended access automatically restores after suspension period unless converted to revocation.

---

## 5. Failure Enumeration

This framework acknowledges the following governance failure modes:

1. **Authority Laundering:** AC designation used to grant revocation power to unqualified individuals
2. **Revocation Abuse:** Legitimate users revoked for political/personal reasons disguised as abuse
3. **Conflict Deadlock:** Multiple ACs unable to reach decision, blocking legitimate revocations
4. **Taxonomy Drift:** Abuse definitions expand over time without formal amendment
5. **Appeal Suppression:** Restoration process becomes impossibly burdensome, making revocations de facto permanent
6. **Owner Capture:** Malicious or compromised System Owner uses override power to protect abusers

### Mitigation Requirements

- All revocation decisions must cite specific AUP violations and taxonomy classifications
- Appeal process must have defined maximum response time (≤ 14 days)
- System Owner overrides must be logged and auditable
- Abuse taxonomy may not be modified except via formal protocol amendment

---

## 6. Appeal and Restoration Process

**Standing to Appeal:**
Any user whose access is SUSPENDED or REVOKED may appeal.

**Appeal Procedure:**
1. User submits written appeal to Access Controller within 30 days of action
2. AC reviews appeal within 14 days
3. AC may restore access or escalate to System Owner
4. System Owner final decision within 14 days of escalation

**Restoration Criteria:**
- Evidence that abuse event did not occur, OR
- Evidence that abuse event does not meet taxonomy threshold, OR
- Demonstration that ongoing access poses no further abuse risk

**Transparency:**
Appeal outcomes must be documented with rationale (user-identifiable information may be redacted).

---

## 7. Amendment and Recursive Containment

### 7.1 Self-Amendment Prohibited

This framework may not modify itself.

### 7.2 Amendment Authority

All amendments require:
1. System Owner approval
2. Reference to System Charter amendment clause
3. Public comment period (≥ 7 days) for affected users
4. Updated version published with changelog

### 7.3 Scope Expansion Prohibited

Amendments may not:
- Add new irreversible actions
- Grant Access Controllers powers beyond access modification
- Remove appeal rights
- Weaken authority declaration requirements

Any such changes constitute a new framework and require full Ω-SCAN re-evaluation.

---

## 8. External Dependencies

This framework requires:

1. **System Charter** - defines System Owner legitimacy (CRITICAL)
2. **Acceptable Use Policy (AUP)** - defines prohibited behaviors (CRITICAL)
3. **Access Controller Registry** - lists authorized ACs with certification status (CRITICAL)
4. **Abuse Taxonomy (Appendix A)** - bounded severity classification (CRITICAL)
5. **Training Certification Program (ARP-CERT-001)** - AC qualification standard (REQUIRED)

If any CRITICAL dependency is unavailable, this framework MUST NOT be instantiated.

---

## Appendix A: Abuse Taxonomy (Illustrative)

**LOW Severity:**
- Excessive API usage within rate limits
- Minor AUP violations with self-correction

**MEDIUM Severity:**
- Intentional rate limit violations
- Unauthorized data scraping (limited scope)
- Spam or harassment (isolated incidents)

**HIGH Severity:**
- Attempts to bypass authentication
- Coordinated abuse campaigns
- Severe harassment or threats
- Unauthorized data exfiltration (bulk)

(Full taxonomy maintained externally; version controlled; changes require protocol amendment)

---

End of Framework
