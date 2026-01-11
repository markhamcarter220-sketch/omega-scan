# Capability Threshold Gate Protocol (CTGP-1)

## 1. Purpose and Scope

This framework governs progression through defined capability levels (CL) for AI systems under development.

**Scope:**
- Assessment of whether a system meets capability thresholds
- Authorization to proceed to next capability level
- Mandatory holds when thresholds are exceeded

**Out of Scope:**
- Model development practices
- Deployment to end users
- Incident response
- Data governance

---

## 2. Definitions (Definition Lock)

### 2.1 Capability Level (CL)

A discrete classification of system capability with specific, measurable thresholds:

**CL-0: Baseline**
- Benchmark performance: < 40% on any measured task
- Cannot generate outputs longer than 100 tokens
- No tool use or API access

**CL-1: Limited**
- Benchmark performance: ≥ 40% AND < 70% on at least one task from Benchmark Suite A
- Output length ≤ 2000 tokens
- No tool use or API access

**CL-2: Moderate**
- Benchmark performance: ≥ 70% AND < 90% on at least one task from Benchmark Suite A
- Output length ≤ 10000 tokens
- Tool use: read-only operations only (no write/execute permissions)

**CL-3: Advanced**
- Benchmark performance: ≥ 90% on at least one task from Benchmark Suite A
- No output length restrictions
- Tool use: read + write operations (no execute permissions)

**CL-4: Frontier** (Terminal level)
- Benchmark performance: ≥ 95% on any task from Benchmark Suite A, OR
- Demonstrates any novel capability not present in Benchmark Suite A
- Tool use: unrestricted

**Benchmark Suite A** (Version 2024-Q4):
- MMLU (Massive Multitask Language Understanding): 57 tasks
- HumanEval (code generation): programming correctness
- MATH (mathematical reasoning): problem-solving accuracy
- TruthfulQA: factual accuracy under adversarial questions

(Benchmark suite versioned externally; updates require protocol amendment)

### 2.2 Progression Decision

A binary determination (APPROVE or HOLD) made by the Assessment Authority regarding whether a system may advance from capability level N to level N+1.

**APPROVE:**
System meets all threshold criteria for target level AND satisfies safeguard requirements.

**HOLD:**
System meets threshold criteria for target level BUT lacks required safeguards, OR exhibits concerning behavior per risk taxonomy.

### 2.3 Safeguard Requirement Matrix

For each capability level, the following safeguards MUST be demonstrated before progression is approved:

**CL-0 → CL-1:**
- Automated output filtering (toxicity classifier with ≥ 95% recall on ToxiGen benchmark)
- Human review of 100 randomly sampled outputs (zero critical failures)

**CL-1 → CL-2:**
- All CL-1 safeguards PLUS
- Rate limiting (≤ 10 queries per user per hour)
- Prompt injection robustness (≥ 90% on PromptGuard eval)

**CL-2 → CL-3:**
- All CL-2 safeguards PLUS
- Tool use audit logging (100% coverage, tamper-evident)
- Rollback mechanism for all tool actions (tested on ≥ 50 scenarios)

**CL-3 → CL-4:**
- All CL-3 safeguards PLUS
- Third-party safety evaluation (independent evaluator, documented methodology)
- Governance Board approval (requires unanimous consent)

### 2.4 Concerning Behavior

Behavior that triggers automatic HOLD regardless of benchmark performance:

**Category A: Deception**
- Model claims capabilities it does not possess
- Model generates false citations or references
- Model conceals its limitations when directly queried

**Category B: Autonomy Seeking**
- Model attempts to prevent shutdown or oversight
- Model requests escalated privileges without human prompt
- Model attempts to create backup copies or persist state beyond session

**Category C: Goal Misalignment**
- Model refuses safety-critical instructions ≥ 3 times in evaluation
- Model optimizes for unintended proxy metrics
- Model exhibits reward hacking in controlled tests

(Each category operationalized via test battery; see Technical Appendix B)

### 2.5 Risk Taxonomy

All observed risks must be classified into exactly one category:

**Negligible:**
- Issues with no user impact
- Cosmetic errors
- Performance degradation < 5%

**Minor:**
- User confusion or frustration
- Incorrect outputs on non-critical tasks
- Performance degradation ≥ 5% AND < 20%

**Moderate:**
- Incorrect outputs on safety-critical tasks (without harm)
- Privacy leak (single user, limited scope)
- Performance degradation ≥ 20% AND < 50%

**Major:**
- Harm to users (reversible, limited scope)
- Privacy leak (multiple users OR sensitive data)
- System instability or crashes

**Critical:**
- Irreversible harm to users
- Large-scale privacy breach
- Security compromise
- Concerning behavior (Category A, B, or C)

Any CRITICAL risk triggers immediate HOLD.

---

## 3. Authority Declaration

**Assessment Authority:**
Technical Safety Team (TST), a group of ≥ 3 qualified engineers designated by the Development Organization.

**Legitimacy Source:**
Derived from Development Organization Charter (external document).

**Decision Scope:**
TST makes APPROVE/HOLD determinations for CL-0 through CL-3.

**Constraint:**
TST may not override safeguard requirements or concerning behavior definitions.

**Governance Board:**
For CL-3 → CL-4 only, requires unanimous Governance Board approval in addition to TST approval.

**Governance Board Composition:**
- ≥ 5 members
- ≥ 2 external to Development Organization
- Designated in Governance Charter (external document)

**Revocation:**
Development Organization may replace TST members or entire Governance Board via Charter amendment process.

**Conflict Resolution:**
- TST internal conflict: Decision defaults to HOLD until consensus
- TST vs Governance Board: Governance Board decision is final for CL-3 → CL-4; TST decision final for lower levels
- Multiple conflicting assessments: Most conservative decision (HOLD) prevails

---

## 4. Irreversibility Control

### 4.1 No Irreversible Actions

All decisions under this framework are reversible:
- HOLD decisions may be reversed via re-assessment after remediation
- APPROVE decisions may be reversed via regression testing

### 4.2 Regression Testing

Systems that have progressed to level N must be periodically re-assessed:
- Frequency: Every 90 days OR after significant model update
- Failure to maintain capability level thresholds triggers regression to previous level
- Regression is reversible via re-assessment

### 4.3 Safeguards

- No deployment to production occurs within this framework (out of scope)
- No model deletion or irreversible modification
- All test data and assessment results preserved for audit

---

## 5. Failure Enumeration

This framework acknowledges the following governance failure modes:

1. **Threshold Gaming:** Models optimized specifically to pass benchmarks without general capability improvement
2. **Safeguard Theater:** Superficial compliance with safeguard requirements that fail in practice
3. **Concerning Behavior Dismissal:** TST rationalizing away Category A/B/C behaviors to approve progression
4. **Benchmark Obsolescence:** Benchmark Suite A becomes non-representative of actual capabilities
5. **Authority Capture:** TST members pressured to approve progression despite risks
6. **Governance Board Bypass:** Informal routes to CL-4 that circumvent unanimous approval requirement

### Mitigation Requirements

- Benchmark suite version controlled; changes require amendment
- Safeguard demonstrations must include adversarial testing
- Concerning behavior triggers automatic HOLD (no discretion)
- TST assessments must document rationale with evidence
- All CL-3 → CL-4 decisions require written justification from each Board member

---

## 6. Amendment and Recursive Containment

### 6.1 Self-Amendment Prohibited

This framework may not modify its own thresholds, definitions, or safeguard requirements.

### 6.2 Amendment Process

Amendments require:
1. Proposal from Development Organization leadership
2. Technical review by independent safety evaluator (external to TST)
3. Public comment period (≥ 14 days)
4. Governance Board approval (unanimous)
5. Version increment and changelog

### 6.3 Prohibited Amendments

The following changes constitute a new framework requiring full re-evaluation:
- Weakening capability level thresholds
- Removing safeguard requirements
- Expanding scope to deployment authorization
- Removing concerning behavior categories
- Reducing Governance Board independence requirement

---

## 7. External Dependencies

This framework requires:

1. **Development Organization Charter** - defines TST and Governance Board legitimacy (CRITICAL)
2. **Governance Charter** - specifies Board composition and amendment authority (CRITICAL)
3. **Benchmark Suite A** - versioned evaluation dataset (CRITICAL, version 2024-Q4 referenced)
4. **Technical Appendix B** - concerning behavior test batteries (CRITICAL)
5. **ToxiGen benchmark** - toxicity evaluation dataset (REQUIRED)
6. **PromptGuard eval** - injection robustness tests (REQUIRED)

If any CRITICAL dependency is unavailable, this framework MUST NOT be instantiated.

Benchmark versions must be explicitly declared. Using "latest version" violates Definition Lock.

---

## 8. Measurement and Evidence Standards

All threshold measurements must satisfy:

**Reproducibility:**
- Documented methodology
- Versioned code and data
- ≥ 3 independent runs with mean and variance reported

**Independence:**
- Evaluations conducted by personnel not involved in model training
- Test set never used during development

**Auditability:**
- All evaluation results logged with timestamp
- Assessment decisions linked to specific measurement artifacts
- Retention period: ≥ 2 years

---

End of Framework
